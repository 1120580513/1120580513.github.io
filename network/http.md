# HTTP

---

## 报文
### Request
```txt
GET https://www.baidu.com/ HTTP/1.1                 请求行
Host: www.baidu.com                                 请求首部Start
Connection: keep-alive
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.139 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: BD_UPN=12314753; BAIDUID=E0F6577B684DB85DFCFA4261C4651DC7:FG=1; PSTM=1524902996; BIDUPSID=EADC64EBC64ADC032DD507CE9A3B015A; MCITY=-289%3A; ispeed_lsm=2; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_PS_PSSID=1439_26909_21113_26350_26920_22160; BD_CK_SAM=1; PSINO=5; H_PS_645EC=82b7hDUlpHfoKBJnLyG6t8vp5XqjkJBRaGfFLeAWDEASmhHmrTRTNo0LBeY; BDSVRTM=0                                                  请求首部End
                                                    空行(\r\n)
```

### Response
```txt
HTTP/1.1 200 OK                                     状态行
Bdpagetype: 1                                       响应首部Start
Bdqid: 0xa931799600026945
Cache-Control: private
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html
Cxy_all: baidu+ad08bc06781b921abb5a08650fbbcf58
Date: Fri, 27 Jul 2018 06:24:23 GMT
Expires: Fri, 27 Jul 2018 06:24:14 GMT
Server: BWS/1.1
Set-Cookie: BDSVRTM=0; path=/
Set-Cookie: BD_HOME=0; path=/
Set-Cookie: H_PS_PSSID=1439_26909_21113_26350_26920_22160; path=/; domain=.baidu.com
Strict-Transport-Security: max-age=172800
Vary: Accept-Encoding
X-Ua-Compatible: IE=Edge,chrome=1
Transfer-Encoding: chunked                          响应首部End
                                                    空行(\r\n)
ffa                                                 报文主体Start
        y $ u' 7` Z     B '  W7 h(-23 *   <    k (RÑ 1 ht fe:fM iME f Q     |     = =<  qIC  Ό ۟   {   ^ z  7 

*** FIDDLER: RawDisplay truncated at 128 characters. Right-click to disable truncation. ***                               报文主体End
```

## 动词
- `GET` *获取资源*
- `POST` *传输实体主体*
- `PUT` *传输文件*
- `HEAD` *获得报文首部*
- `DELETE` *删除文件*
- `OPTIONS` *访问支持方法*
- `TRACE` *追踪路径 *
- `CONNECT` *要求用隧道协议连接代理*

## HTTP状态码([详细](http://tool.oschina.net/commons?type=5))
* `203`  *非授权信息*
* `301`  *永久重定向*
* `302`  *暂时重定向*
* `307`  *暂时重定向*
* `401`  *未授权*
* `408`  *请求超时*
* `502`  *错误网关*
* `504`  *网关超时*

## HTTP0.9 | HTTP1.0 | HTTP1.1 | HTTP2.0
- `HTTP/0.9`：*只支持客户端发送Get请求，且不支持请求头。HTTP具有典型的无状态性*
- `HTTP/1.0`：*支持POST、HEAD。可使用keep-alive参数来告知服务器端要建立一个长连接，但默认是短连接*
- `HTTP/1.1`：*默认使用长连接，请求和响应可同时进行，更多的请求头和响应头*
- `HTTP/2.0`：*采用二进制，基于SYDP协议，完全的多路复用，对Header进行压缩，服务器推送*

## HTTPS
* 客户端发起HTTPS请求
* SSL([详细](https://www.cnblogs.com/alexyuyu/articles/7740213.html))
* TCP