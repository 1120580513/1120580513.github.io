# Coding Interview

---

## Network
### 在地址栏输入网址按下回车会发生什么？[源](https://blog.csdn.net/u014600626/article/details/78720763)
1. 域名解析（DNS解析）
    1. 搜索自身浏览器DNS缓存
    2. 搜索操作系统DNS缓存
    3. 读取hosts文件
    4. 向本地配置的首选DNS服务器发起域名解析请求（电信运营商）
    5. 运营商DNS服务器先找自身的缓存
    6. 运营商DNS服务器递归请求（一般都能成功）
    7. 查找NetBIOS名称缓存
    8. 查找WINS服务器
    9. 进行广播查找
    10. 读取LMHOSTS文件
2. 发起TCP三次握手
	1. Client->Server发送连接试探
	2. Server->Client发送确认
	3. Client->Server再次确认
3. 发起HTTP请求
4. 服务器端响应http请求
### TCP三次握手、四次挥手过程，为什么建立连接协议是三次，而关闭连接却是四次，为什么TIME_WAIT状态还需要等2MSL后才能返回到CLOSED状态？
[查看](../network/tcp.md)
### Ping工作原理？
> 利用网络上机器IP地址的唯一性，给目标IP地址发送一个数据包，再要求对方返回一个同样大小的数据包来确定两台网络机器是否连接相通，时延是多少
### 了解哪些常用的端口号？
* FTP：20、21
* SMTP：25
* DNS：53
* 远程桌面：3389
* Redis：6379
### 网站有多个域名有什么好处？
> 为了并发，突破浏览器对同一域名下对图片请求的限制
### 利用Socket建立网络连接的步骤？
[查看](../network/socket.md)

## DB

## CSharp


## RabbitMq、ActiveMq、ZeroMq、kafka区别
* TPS：ZeroMq > RabbitMq > Kafka > ActiveMq
* 持久化：zeroMq不支持
* 可靠性(路由、事务、集群、高可用队列、工具、插件、社区…)：RabbitMQ>ActiveMQ
* 并发：RabbitMQ最好，l因为他用的是erlang
