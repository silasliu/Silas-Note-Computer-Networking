
## TCP特性
* 累计确认cumulative acknowledgement     
  * 接收端收到失序分组会发送冗余ACK
* 重传->超时间隔(TimeoutInterval)翻倍->收到ACK或新的上层数据->重置为EstimatedRTT和DevRTT的计算结果
* 使用重复确认而不是NACK
* 使用快速重传(fast retransmit)--在定时器过期之前重传丢失的片段
  * 基于冗余ACK(duplicate ACK)检测的快速重传机制(收到3个冗余ACK则重传)
* TCP使用单一重传定时器
* 是公平的