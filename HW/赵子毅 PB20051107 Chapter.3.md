## 赵子毅 PB20051107 Chapter.3

#### R3:

源y目x

#### R7:

是的，UDP协议虽然只包含两个端口号，但其运行在IP协议上，IP协议中记录了源ip和目的ip，所以是可以区分源主机的

#### R9:

接收方需要序号来区分当前数据是来自重传还是一个新的数据

#### R10：

防止网络链路出现丢包，若ACK和NAK在传输的过程中丢失，如果没有计时器，双方将一直这么等下去

#### R14：

1. false，没数据也得发送ack等信号
2. false，根据接收方缓存剩余动态变化
3. true
4. false，可能失序到达
5. true
6. false，sampleRTT并不会严重影响DevRTT和EstRTT，Interval变化较慢，不一定已经大于1秒
7. false，确认号是对对方下一个数据的希望

#### R15:

1. 20bytes
2. ack=90

#### R17：

R/2

#### R18：

不是，是设置到当前rwnd的一半

#### P1：

1. A.port S.port
2. B.port S.port
3. S.port A.port
4. S.port B.port
5. yse
6. no

#### P3：

原码00101110 反码11010001

检验的时候直接求和看是不是0就好，方法简单。1bit必出，2bit不一定行

#### P8：

![rdt3](./image/c3_rdt3recv.png)

#### P15:


$$
RTT=30ms\\
L/R = 1500/10^9=0.0015ms\\
U_{利用}=\frac{L/R * cwnd}{RTT+L/R}=0.9\\
cwnd \approx 18001
$$

#### P23:

考虑区分极限情况

1. 情况一，所有的ack确认均丢失，假设窗口长度为m，记send_base = sd,则接收方窗口为sd+m to sd + 2m - 1，而发送方窗口为sd to sd + m -1
2. 情况二，所有的ack均收到，则发送方窗口为sd + m to sd + 2m -1 ,接收方窗口也为此
3. 所以有：情况二发送方窗口的最后一个元素的序号必须小于情况一发送窗口的第一个元素的序号+ k ， 所以有sd + 2m - 1 - （sd + k)<0,所以k >2m-1,**k>=2m**

#### P24:



1. true，考虑发送方在t1时刻发送了帧，然后发生了超时重传该帧，然后收到了来自接收方对第一次的帧的确认，移动窗口，然后受到了接收方对重传帧的确认
2. true，1中的例子也会发生这种情况
3. true
4. true

#### P27：

1. 207，302，80
2. 207，80，302
3. 127
4. ![c3p27](./image/c3p27.png)

#### P32:

1. 
$$
DevRTT = (1 - \beta)DevRTT + \beta \lvert SampleRTT - EstimatedRTT \rvert\\
EstimatedRTT = (1-\alpha)*EstimatedRTT + \alpha * SampleRTT\\
EstimatedRTT^{(4)} =x*SampleRTT^{(1)}+x(1-x)^{}*SampleRTT^{(2)}\\
+x(1-x)^{2}*SampleRTT^{(3)}+(1-x)^{3}*SampleRTT^{(4)}
$$

2. 
$$
EstimatedRTT^{(n)}=x\sum_{i=1}^{n-1}(1-x)^{i-1}*SampleRTT^{(i)}  + (1-x)^{n-	1}*SampleRTT^{(n)}
$$

3. 
$$
n->\infin 可以忽略最后一项\\
EstimatedRTT^{(\infin)}=x(1-x)\sum_{i=1}^{\infin}(1-x)^{i}*SampleRTT^{(i)}\\
=\frac19\sum_{i=1}^{\infin}(0.9)^{i}*SampleRTT^{(i)}\\
过去的样本呈指数型下降
$$

   

#### P40:

1. [1,6], [23,26]
2. [6,16]
3. 3个冗余ack，否则cwnd应该是1而不是24
4. 超时，cwnd被设置为1而不是+1（快速重传时受到冗余ack会继续加窗口）
5. 32
6. 21
7. 14（14.5，如果取下整则为14）
8. 简单计算可知在第7个
9. 7和4
10. ssthresh=21 ,cwnd = 4
11. (round,packet):(17,1),(18,2),(19,4),(20,8),(21,16),(22,21) 一共32+21 = 52个packet

#### P45：

$$
packets =  \sum_{n=0}^{W/2}(\frac{W}{2}+n) = \frac38W^2+\frac34W\\
L = 1/packets = \frac{1}{\frac38W^2+\frac34W}\\
W = O(W^2), L \approx \frac8{3W^2}, W = \sqrt\frac8{3L}\\
平均速率:\frac34W\frac{MSS}{RTT} = \frac{1.22*MSS}{RTT * \sqrt{L}}
$$

#### P53：

$$
Speed =\frac{1.22*MSS}{RTT * \sqrt{L}}
10Gbps = 1.22*(1500*8bits)/(0.1s*\sqrt{L})\\
解得L = 2.14*10^{-10}
$$

