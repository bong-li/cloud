# HTTP
### 概述
#### 1.HTTP协议特点
* HTTP协议是**无状态**的
为了追踪用户的身份和访问记录,就有了cookie和session
</br>
* 在HTTP中 一个request只能有一个response，而且这个response也是**被动的**，不能主动发起
</br>
* 一个页面一般包含多个资源,每个资源都需要单独的请求

#### 2.HTTP/1.0 和 HTTP/1.1 比较
* HTTP/1.0，**短链接**
一次连接就是一个 Request 一个 Respons
</br>
* HTTP/1.1，**长连接**
有一个keep-alive，在一个HTTP连接中，可以发送多个Request，接收多个Response，在一定时间内没有请求，才断开连接
</br>
* HTTP/1.1协议开始,使用**MIME标准**
不仅仅可以输出文本文件,还有其他的文件类型（MIME:multipurpose internet mail extensions,定义了多种文件类型）

#### 3.一次http请求的过程
```mermaid
graph TD
A("建立TCP连接")-->B("接收请求")
B-->C("处理请求(对请求报文进行解析)")
C-->D("访问资源")
D-->E("构建响应报文")
E-->F("发送响应报文")
F-->G("记录日志")
```

#### 4.请求报文格式
```shell
Method  Path  protocol/version
Headers(key:value)
          #空行（回车符和换行符）
content   #非必须，GET是没有请求主体的
```

#### 5.响应报文格式
```shell
protocol/version  StatusCode  StatusMessage
Headers(key:value)
content
```

#### 6.常用请求方法
* GET - 从指定的资源请求数据
用来获取数据，也可以上传数据，通过在url后面加？加键值对，由于地址栏长度有限，所有只能上传少量的数据，且数据是明文传输的
</br>
* POST - 提交表单
</br>
* PUT - 操作是幂等的,所谓幂等是指不管进行多少次操作，结果都一样
PUT是上传一个资源,用的少,不安全

##### 7.模拟http请求
（1）http/1.0
```shell
exec 8<>/dev/tcp/10.0.36.1/80
echo -n -e "GET / HTTP/1.0\n\n" >&8
cat <&8

#或者

echo -n -e "GET / HTTP/1.0\n\n" | nc 10.0.36.1 80
```
（2）http/1.1
```shell
echo -n -e "GET / HTTP/1.1\nHost:10.0.36.1\n\n" >&8
```
**备注：\n 表示空行**
**当时windows等系统，\n 需要替换为 \r\n**