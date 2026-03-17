- > 原文链接
  [RTP协议全解析（H264码流和PS流）](https://blog.csdn.net/chen495810242/article/details/39207305)
-
- ## RTP Header解析
	- ![](../assets/RTP_Header.jpg){:height 260, :width 748}
	- 如上图所示为**RTP Header**的结构（大端模式展示）
	- `V`：RTP协议的版本号，占2位，当前协议版本号为2
	- `P`：填充标志，占1位。如果`P=1`，则在该报文的尾部填充一个或多个额外的8位组，它们不是有效载荷的一部分。
	- `X`：扩展标志，占1位。如果`X=1`，则在RTP报头后跟一个扩展报头。
	- `CC`：CSRC计数器，占4位，指示CSRC标识符的个数。
	- `M`：标记，占1位，不同的有效载荷有不同的含义，对于视频，标记一帧的技术；对于音频，标记会话的开始。
	- `PT`：有效载荷类型，占7位，用于说明RTP报文中有效载荷的类型，如GSM音频、JPEM图像等。在流媒体中大部分用来区分音频流和视频流的，这样便于客户端解析。
	- `sequence number`：序列号，占16位，用于标识发送者所发送的RTP报文的序列号，每发送一个报文，序列号增1。这个字段当下层的承载协议用UDP的时候，网络状况不好的时候可以用来检查丢包。同时出现网络抖动的情况可以用来对数据进行重新排序，序列号的初始值是随机的，同时音频包和视频包的序号是分别计数的。
	- `timestamp`：时戳，占32位，必须使用90kHz的时钟频率。时戳反应了该RTP报文的第一个8位组的采样时刻。接收者使用时戳来计算延迟和抖动，并进行同步控制。
	- `同步信源标识符`：占32位，用于表示同步信源。该标识符是随机选择的，参加同一视频会议的两个同步信源不能有相同的SSRC。
	- `特约信源标识符`：每个CSRC标识符占32位，可以有0-15个。每个CSRC标识了包含在该RTP报文有效载荷中的所有特约信源。
	- ==注意：基本的RTP说明并不定义任何扩展头本身，如果遇到`X=1`，需要特殊处理。==
	- 取一段码流如下：
	- <table>
	    <tr>
	      <th style="text-align: center;">原始码流</th>
	      <th style="text-align: center;">ASCII码值</th>
	    </tr>
	    <tr>
	      <td style="text-align: center;">80 e0 00 1e 00 00 d2 f0 00 00 00 00 41 9b 6b 49</td>
	      <td style="text-align: center;">€?....??....A?kI</td>
	    </tr>
	    <tr>
	      <td style="text-align: center;">e1 0f  26 53 02 1a ff  06 59 97 1d d2 2e 8c  50 01</td>
	      <td style="text-align: center;">?.&S....Y?.?.?P.</td>
	    </tr>
	    <tr>
	      <td style="text-align: center;">cc 13  ec 52 77 4e e5 0e 7b fd 16 11 66 27  7c b4</td>
	      <td style="text-align: center;">?.?RwN?.{?..f\|?</td>
	    </tr>
	    <tr>
	      <td style="text-align: center;">f6 e1  29 d5 d6 a4 ef 3e 12 d8 fd  6c 97 51 e7 e9</td>
	      <td style="text-align: center;">??)????>.??l?Q??</td>
	    </tr>
	    <tr>
	      <td style="text-align: center;">cf c7  5e c8  a9 51 f6 82 65 d6 48 5a 86 b0 e0 8c</td>
	      <td style="text-align: center;">??^??Q??e?HZ????</td>
	    </tr>
	  </table>
	- 其中
		- `80`：`V_P_X_CC`
		- `e0`：`M_PT`
		- `00 1e`： `SequenceNumber`
		- `00 00 d2 f0`：`Timestamp`
		- `00 00 00 00`：`SSRC`
	- 把前两个字节转为二进制序列：`1000 0000 1110 0000`
	- 按顺序解释如下：
		- `10`：`V`
		- `0`：`P`
		- `0`：`X`
		- `0000`：`CC`
		- `1`：`M`
		- `110 0000`：`PT`
- ## RTP载荷H264码流
	- ![Nalu Header](../assets/Nalu Header.jpg)
	- 载荷格式定义三个不同的基本载荷结构，接收者可以通过RTP载荷的第一个字节后5位（上图所示）识别荷载结构。
		- 1)   单个NAL单元包：载荷中只包含一个NAL单元。NAL头类型域等于原始 NAL单元类型，即在范围1到23之间
		- 2)   聚合包：本类型用于聚合多个NAL单元到单个RTP荷载中。本包有四种版本，单时间聚合包类型A (STAP-A)，单时间聚合包类型B (STAP-B)，多时间聚合包类型(MTAP)16位位移(MTAP16)，多时间聚合包类型(MTAP)24位位移(MTAP24)。赋予STAP-A，STAP-B，MTAP16，MTAP24的NAL单元类型号分别是 24，25，26，27
		- 3)   分片单元：用于分片单个NAL单元到多个RTP包。现存两个版本FU-A，FU-B，用NAL单元类型 28，29标识
	- 常用的打包时的分包规则是：如果小于MTU采用单个NAL单元包，如果大于MTU就采用FUs分片方式。
	  因为常用的打包方式就是单个NAL包和FU-A方式，所以我们只解析这两种。
	- ### 单个Nalu单元包
		-