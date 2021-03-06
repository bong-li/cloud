[toc]
### 挂载镜像
#### 1.挂载raw格式镜像(用下面方法挂载即可)
##### (1)创建一个镜像文件
```shell
  用dd或qemu-img
```
##### (2)与回环设备关联   //回环设备可以将文件当块设备使用
```shell
  losetup -f                #查看空闲的回环设备
  losetup /dev/loopn 文件   #将文件与回环设备关联
```
##### (3)进行分区和格式化
```shell
  fdisk /dev/loopn
  mkfs.xfs /dev/loopn
```
##### (4)根据设备的分区表，创建设备映射
```shell
  kpartx -av /dev/loopn
```
##### (5)挂载
```shell
  mount /dev/mapper/xx 挂载点
```

#### 2.挂载所有类型的镜像
```shell
  guestmount -a 镜像名 -m 分区 挂载点   
#m:mount,当不知道有什么分区时，随便写，错误之后会提示
#-i:inspect,会自动检测操作系统
```

### 制作后端盘

#### 1.创建一个虚拟机，并在其中操作，然后把这个盘作为后端盘

#### 2.对虚拟机剩余空间进行写零操作
```shell
#基本原理是向磁盘中写入一个全0的文本文件，直到磁盘被填充满，然后将文件删除
  dd if=/dev/zero of=/zero.dat
```

#### 3.对qcow2镜像进行压缩，生成新的镜像
```shell
  qemu-img convert -c -O qcow2 xx yy       
#c:compress,压缩
#O:out_format
```
