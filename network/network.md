# Network

---

![计算机网络体系结构](file/network_structure.png)

## OSI
1. 物理层
2. 数据链路层
3. 网络层
4. 运输层
5. 会话层
6. 表示层
7. 应用层

## TCP/IP
1. 网络接口层
2. 网际层IP
3. 运输层(TCP | UDP)
4. 应用层(TELNET | FTP | SMTP)

## 五层协议
1. 物理层
2. 数据链路层
3. 网络层
4. 传输层
5. 应用层

## 在地址栏输入网址按下回车会发生什么？[源](https://blog.csdn.net/u014600626/article/details/78720763)
1. 域名解析（DNS解析）
    * 搜索自身浏览器DNS缓存
    * 搜索操作系统DNS缓存
    * 读取hosts文件
    * 向本地配置的首选DNS服务器发起域名解析请求（电信运营商）
    * 运营商DNS服务器先找自身的缓存
    * 运营商DNS服务器递归请求（一般都能成功）
    * 查找NetBIOS名称缓存
    * 查找WINS服务器
    * 进行广播查找
    * 读取LMHOSTS文件
2. 发起TCP三次握手
    * Client->Server发送连接试探
    * Server->Client发送确认
    * Client->Server再次确认
3. 发起HTTP请求
4. 服务器端响应http请求