
## 协议特点
1. 面向连接
2. 全双工服务(full-duplex service)
3. 点对点(point-to-point)  
4. 可靠的运输层协议

面向连接:由建立连接开始,以断开连接结束.  
"连接"是指逻辑连接,共同状态保存在端对端系统的TCP程序中  

## 提供的服务
1. 可靠数据传输
   1. 无差错
   2. 有序
   3. 无冗余
   4. 无丢失
2. 流量控制
3. 拥塞控制

## 不提供的服务
1. 时延保证
2. 带宽保证


## 可靠数据传输服务
### 差错恢复
* GBN风格(累计确认)
* 缓存失序分组
* 不会重传丢失分组之后的分组??

### 重发(往返时间估计与超时)
TCP不定期测量SampleRTT  
> SampleRTT 某报文交付给网络层到收到确认的时间间隔    
TCP使用SampleRTT维护EstimatedRTT  
#### EstimatedRTT 
EstimatedRTT->RTT均值   
[一种指数加权移动平均 exponential weighted moving average (EWMA)]
```
EstimatedRTT=(1−α) ⋅ EstimatedRTT+α ⋅ SampleRTT
```
α建议值为0.125
#### DevRTT
DevRTT->RTT偏差  
```
DevRTT=(1−β) ⋅ DevRTT+β ⋅ |SampleRTT−EstimatedRTT|
```
β 推荐值0.25  
#### 重传超时间隔
计算方法:  
```
TimeoutInterval=EstimatedRTT+4 ⋅ DevRTT  
```
TimeoutInterval初始值1s  
#### 重传定时器
TCP使用单一重传定时器
 



## 流量控制服务 flow-control service
### 实现方式
* 发送方维护接收窗口(receive window,rwnd)变量(指示接收方还有多少空间)
* 接收方会将rwnd(缓冲区可用空间大小)放在TCP报文段接收窗口字段
* 发送方确保```LastByteSent−LastByteAcked≤rwnd```
![](https://oss.silas.fun/markdown/20200221104837.png)

## 拥塞控制服务
### 术语
#### AIMD(Additive-Increase,Multiplicative-Decrease)
加性增,乘性减
### 方案
TCP采用端到端拥塞控制  
### 实现细节
* 发送方维护拥塞窗口(congestion window,cwnd)
* 发送方确保```LastByteSent−LastByteAcked≤cwnd```,间接限制发送速率
* 发送方感知方式:
  * 出现丢包->拥塞的指示
  * 自计时(self-clocking)->ack的速率影响TCP的发送速率
* TCP确定发送速率的方式:
  * 丢失报文段->降低发送速率
  * ACK首次到达->增加发送速率
  * 带宽探测

### TCP 拥塞控制算法(TCP congestion control algorithm)
#### 1 慢启动(slow-start)
##### 特点
强制要求.  
初始发送速率=```MSS/RTT```  
随后发送速率翻倍  
##### 慢启动阶段结束时机
1. 超时丢包->```set cwnd=1MSS,ssthresh=cwnd/2```,进入慢启动
2. 达到/超过ssthresh->进入拥塞避免状态
3. 收到三个冗余ACK->快速重传,进入快速恢复状态
#### 2 拥塞避免
强制要求.  
线性增长-每个RTT只将cwnd增加一个MSS  
##### 拥塞避免阶段的结束时机
1. 超时丢包->```set cwnd=1MSS,ssthresh=cwnd/,进入慢启动,进入慢启动
2. 收到3个冗余ACK->``` set cwnd=cwnd/2,ssthresh=cwnd/2```,进入快速恢复状态
#### 3 快速恢复
##### 特点
非强制要求  
##### 状态变化
1. 收到一个冗余ACK->``` set cwnd+=MSS```
2. 收到丢失报文段的一个ACK->降低cwnd进入拥塞避免
3. 超时丢包->```set cwnd=1MSS,ssthresh=cwnd/2```,进入慢启动


### 理想化的TCP稳态动态性模型
```一条连接的吞吐量=0.75W/RTT```  
W为拥塞窗口长度  

