[toc]

### tcpdump
抓包阶段 是在 包被iptables 处理 **之前**
#### 1.在宿主机上抓取容器中某个网卡的数据包
##### （1）方法一
默认`ip netns`无法显示和操作容器中的netns
* 获取容器的pid
```shell
pid=`docker inspect -f '{{.State.Pid}}' <CONTAINER_ID>`
#根据pid可以找到netns：
#  /proc/<PID>/net/ns
```
* 创建`/var/run/netns/`目录
```shell
mkdir -p /var/run/netns/
```

* 将netns连接到`/var/run/netns/`目录下
```shell
ln -s /proc/<PID>/ns/net /var/run/netns/<CUSTOME_NAME>

#ip netns list就可以看到该netns
```
* 监听
```shell
ip netns exec <CUSTOME_NAME> <COMMAND>
```
##### （2）方法二
* 进入容器执行
```shell
$ cat /sys/class/net/<INTERFACE>/iflink

28
```
* 在宿主机执行
```shell
$ ip link

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 00:50:56:b8:6d:a3 brd ff:ff:ff:ff:ff:ff
... ...
28: cali5ddcf4a2547@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default
    link/ether ee:ee:ee:ee:ee:ee brd ff:ff:ff:ff:ff:ff link-netnsid 7
```

* 在宿主机上 抓取 容器中指定网卡 的数据包
```shell
tcpdump -i cali5ddcf4a2547 -nn
```