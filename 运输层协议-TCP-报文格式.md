
# TCP报文段结构  
![](https://oss.silas.fun/markdown/20200217101120.png)
### 特点
TCP首部典型长度=20字节
## 标志字段 flag field
### ACK比特
只是确认字段中的值是有效的
### RST,SYN和FIN比特
用于连接建立和拆除
### CWR和ECE比特
拥塞通告
### PSH比特
只是接收方应立即将数据交给上层  
(未使用)
### URG比特
指示报文段里存在着被发送端的上层实体置为"紧急"的数据  
(未使用)

## 序号Sequence number
报文段首字节的字节流序号  
空数据的报文段也需要填入序号
## 确认号Acknowledgement number
期望收到的下一字节的序号  
确认号是被捎带(piggybacked)在数据报文段中
## 接收窗口Receive window
用于实现流量控制
