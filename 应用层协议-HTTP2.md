## RFC 
[RFC](https://httpwg.org/specs/rfc7540.html)  
## 特点
* 二进制协议

## 功能
* 多路复用
* 头压缩
* 服务器推送
 

## 多路复用
HTTP请求响应与其自身Stream 相关联,实现多路复用  
> Multiplexing of requests is achieved by having each HTTP request/response exchange associated with its own stream (Section 5). Streams  
> todo:2020年02月13日 有时间看看多路复用原理

### 作用
解决浏览器对单域名TCP连接数的限制  
### 特点
多路复用允许同时通过单一的 HTTP/2 连接发起多重的请求-响应消息。  
## 协议格式 frame 
frame 是HTTP2 协议的基本单元(Unit)  

frame 的 结构  
![](https://oss.silas.fun/markdown/20200213214957.png)
HTTP1.1 跟 HTTP2 的比较  
![](https://oss.silas.fun/markdown/20200213174908.png)  
一个HTTP1.1的请求封装成HTTP2的格式  
![](https://oss.silas.fun/markdown/20200213173838.png)   