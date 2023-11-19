# Chapter 1 计算机网络概述

## 1）引入：
互联网连接着世界上数以亿记的电子设备，这是一个庞大的树状系统，盘根错节地分布着不同的网络系统，形象地说，互联网是网络的网络，在互联网这个最大的层级之下，还有各类规模各不相同的较小的网络系统相互连接。

这颗参天大树生长出的不计其数的树叶是连接到互联网的各类电子设备，称为端系统(End system)，或是主机(Host)，根据在不同通讯过程中发挥的不同作用，这些主机可以进一步分类为伺服器(Server)和客户端(Client)。通常来讲，主动发起通讯请求的是客户端，而接收通讯请求的是伺服器。

## 2）网络服务提供商(ISP)的层级结构

当今世界的网络服务（包括基站、网线等基础设施和在基础设施上运行的程序）主要是由网络服务提供商(Internet Service Provider, ISP)提供的。和互联网复杂的层级关系相对应，网络服务提供商也有不同的层级，主要以提供服务的对象作为区分。主机通过提供接入网络服务提供商(Access ISPs)的服务来连接到互联网，主机借由连接到互联网的网络系统被称为接入网(Access network)。要让这些连接到了不同的接入网络服务提供商的主机能够互相通信（这是互联网的主要目的），我们也需要将这些不同的接入网络服务提供商连接起来。

正如开头所说，互联网是一个庞大的树状系统，因此它对于这一问题的解决方案便是使用更高一层的网络服务提供商来将接入网络服务提供商连接起来，我们一般称这一层级的服务商为区域性网络服务提供商(Regional ISPs)，接入网络服务提供商是主机的服务商，区域性网络服务提供商则是接入网络服务提供商的服务商，而区域性网络服务提供商上一层还有最高一层的网络服务提供商(Tier-1 ISPs)，它们总揽全球互联网的运行。服务商的客户（主机或者低一层的网络服务提供商）通过网络服务提供点(Points of Presence, PoP)来连接到服务商；任何低一层的网络服务提供商或者主机可以选择连接到多个更高层的网络服务提供商，这被称为多宿主(Multi-homing)，这是一种提高连接性能和可靠性的策略；两个邻近的同层级的网络服务提供商可以选择对等互联(Peering)，在网络的“下游”用户之间进行流量交换，这样连接到这两个网络服务商的主机的数据可以不用再通过更高一层的服务商网络而直接到达与之对等互联的另一个 服务商网络中；基于对等互联的概念，一个第三方企业可以提供互联网交换中心(Internet Exchange Point, IXP)，多个服务商的网络在此处进行对等互联。而随着互联网的发展，为了缓解互联网性能瓶颈,内容分发网络(Content Distribution Network, CDN)应运而生，它们往往和各个层级的服务商以及互联网交换中心建立连接，尽可能为邻近的主机提供高速连接以及减少服务商方面的开支。


注意，区域性网络服务提供商往往内部还有复杂的层级系统，而非单一的一层网络，例如在中国，每个城市都有服务商，连接到每个省份的服务商，最后连接到全国区域的服务商，这三层服务商都是区域性网络服务提供商。

## 3）接入网的物理形式

数字用户线路(Digital Subscriber Line, DSL)
数字用户线路利用已有的电话线路（双绞线），通过电话线路将数字信号转化为模拟信号传输到本地的电话服务提供商，后者再将模拟信号转化为数字信号后传输到网络服务提供商的网络中。

正如图中所示，数字用户线路利用不同的频率来同时传输网络数据和电话信号：

高速下载通道，50 kHz - 1 MHz
中速上行通道，4 kHz - 50 kHz
原有的电话信号通道，0 - 4 kHz
在用户段，一个分离器负责合成和分离电话信号和网络数据，而在服务商端，数字用户线接入复用器(Digital Subscriber Line Access Multiplexer, DSLAM)则透过电话线提供高速互联网接入服务。值得一提的是，如果客户和数字用户线接入复用器距离过远（一个数据是5.5公里），则服务质量会因为干扰而急剧下降。

## 4）有线互联网接入(Cable Internet access)
这一形式的接入网则是在电视线缆的基础上构建的，每一户都通过同轴电缆(Coaxial cable)与所在社区的接入点连接，每个接入点又通过光纤和当地的电视公司的有线电视头端(Cable head end)连接，进而连接到互联网。

和电话线接入类似，有线网络接入也通过频率分为上行和下载两个通道，分配给下载的通道的速度更快。值得注意的是，有线互联网接入会在相邻的接入点之间分享带宽，对于连接到同一个有线电视头端的主机，当它们同时上传或者下载时，它们访问互联网的速度就会因为带宽被其他主机挤占而显著降低。

## 5）光纤到家(Fiber To The Home, FTTH)
顾名思义，就是家庭直接与服务商通过光纤建立连接。这一连接方式又有两种类型：主动的(Active Optical Networks, AONs)和被动的(Passive Optical Networks, PONs)，在此仅仅介绍后者。

如图所示，每一户都装有光网络终端(Optical Network Terminal, ONT)，作为数字信号和光信号的分野，而每一户的光网络终端又连接到本地社区的光分路器(Optical fibers)，光分路器和光学线路终端(Optical Line Termination, OLT)连接，后者负责光信号和数字信号转换，并最终和互联网连接。

和有线互联网接入类似，光纤到家也会在邻近主机之间分享带宽。
卫星链路(Satellite link)
拨号上网(Dial-up access)
以太网(Ethernet)
在这一形式中，主机通过双绞线以星状连接到交换机(Ethernet switch)形成局域网(Local Area Network, LAN)，进而连接到互联网。
WiFi
4G

## 6）数据是如何在不同主机之间传输的
在讨论完互联网的物理架构之后，让我们来看看数据是如何在这些复杂的网络中传输的。网络数据的传输和传统的电话通信不同，后者使用电路交换(Circuit Switching)的通信范例，在每一次通信时都会为通信的双方建立单独的通信路径，这个通信对独享这个通信路径上的所有资源；而互联网则使用分组交换(Packet Switching)的方法来应对远比电话通信大的数据流量。简单来说，分组交换就是将所有数据，无论大小，都分割成不大于某一数据量的数据段进行传输，我们称之为包(Packet)。这些包在经过数个连接（通常是线缆）和分组交换机路由(Routing)到目的地。这一过程可以类似地看作已经存在了几百年的邮政服务——根据信件和包裹上的信息，各个邮局可以将它们送到指定的目的地去。与邮政包裹有所不同的地方在于，较大的数据一般会被拆分成小数据再包装进行传输，在到达目的地之后重组成原来的数据。

## 7）路由表(Forwarding tables)

你来到一栋陌生的写字楼，你想要知道甲公司的财务部的李先生在哪里，于是你走向前台，询问甲公司在哪一楼层，接着知道了楼层的你坐电梯来到了这个楼层，这一层楼很大，你被绕晕了，于是你找到了厕所门前的清洁工询问甲公司在什么地方，清洁工告诉你它在那个方向的角落，然后用手指了指，你便循着那个方向找去，功夫不负有心人，你终于来到了甲公司的门口，于是你顺着门牌找到了他们的财务部，你无助地看着混杂排列的几十个座位，决定向进门左手边的员工询问李先生的座位在哪，他很热心地领你来到了空无一人的座位前，旁边的同事看了你一眼，挠了挠头告诉你李先生今天有事请假在家...

这便是一个包裹在传输途中会遇到的事情，包裹上附着的地址会告诉路途中的每一个路由器这个包裹的最终目的地在哪，于是路由器就为这个包裹指出一个大致的方向，然后包裹就随着那个方向继续传输，直到最终到达目标地址代表的主机。路由器就像是上面那个故事里的前台工作人员、清洁工、热心的同事，他们拥有这栋楼层的相关信息，可以帮助一个迷茫的路人找对方向。路由器拥有的这些相关信息就被存储在路由表中，用术语说，路由表中含有周边路径的拓扑信息。这一复杂而又实时变动的信息显然不可能由专人来维护，它是由一系列路由协议(Routing protocol)自动维护的，它使路由器可以自动学习到其他路由器的网络，在网络结构发生改变时相应地更新路由表。后面会详细介绍这一系列协议。

## 8）储存并转发(Store-and-forward)

由于数据是以类似包裹为单位传输，但它们在线缆中却又是以数据流的形式传输，因此在传输的路径上的交换机需要等待同一个包裹的数据完全到达，也即该包裹的最后一比特进入交换机之后，该交换机才能读取完整的包裹中的内容，决定它接下来要传输的方向。因此，交换机必须要有一个输入缓冲区域(Buffer)来储存已经到达的包裹数据。也即包裹会在每个交换机处等待一段时间，在包裹完整到达交换机之后才会继续传输。

为了更透彻地理解这一过程，让我们做一些简单的数学题，现在假设有一个简单的网络，由两台主机、一个交换机和两条网线组成：主机甲 ---网线甲--- 交换机 ---网线乙--- 主机乙。规定每一个数据包裹的大小都是L比特，两根网线每秒能够传输R比特的数据（这一速度有限也导致了网络存在传输延迟）。同时我们假设每一比特在网线的传输速度无限快（现实中通常是光速的三分之二大小），以及忽略交换机中其他操作耗费的时间。

如何计算一个包裹的传输时间呢？一个包裹的传输时间应该是从第一个比特进入网线甲到最后一个比特进入主机乙经过的时间，我们将目光集中于最后一个比特。由于网线的承载能力有上限，每一秒都只有R比特可以进入网线甲，在第L/R秒时最后一个比特进入网线甲，而此时第一个比特已经进入了交换机（比特在网线中传播得无限快），在经过了可忽略的极小时间之后，最后一个比特也进入了交换机；类似地，每一秒都只有R比特可以进入网线乙，因此在第L/R+L/R秒时最后一个比特才进入网线乙，接着在可以忽略的时间内进入了主机乙，所以一个包裹的传输时间就是2L/R秒。

可以证明，一个长为L比特的包裹经过一个有P条网线的路径的传输时间为PL/R秒。
在得到了一个包裹的传输时间后，我们可以接着计算N个大小为L比特的包裹的传输时间，图示如下：
类似上文讨论，我们考虑最后一个包裹到达时经过的时间，在最后一个包裹出发之前，经过了(N-1)L/R秒，而最后一个包裹到达目的地的时间是2L/R，所以总的时间为(N+1)L/R。
综合起来，N个大小为L比特的包裹经过有P条传输速率为R比特/秒的网线的路径的时间为(N-1+P)L/R秒。

## 9）延迟
和任何传输一样，互联网中数据的传输也需要花费时间，相信这一概念对常打网络游戏的同学并不陌生。网络延迟可以分为四类：
#### 1. 处理延迟(Processing delay)
处理延迟通常指包裹在交换机中被分析处理花费的时间，比如找到包裹的传输方向，检查包裹是否有错误等等。在现代的高速交换机中，这一延迟通常是微秒级别的。
#### 2. 排队延迟(Queueing delay)和丢包(Packet loss)
通常来说每个交换机都有三个或以上的连接（否则就没有必要在这个地方设置交换机了），而对每个连接，交换机都有一个输出缓冲区。这和前文的输入缓冲区不同，交换机只有在确定了这个包裹的传输方向之后，才会将它放入输出缓冲区中，而交换机是无从知道输入缓冲区中的不完整的包裹的传输方向的。输出缓冲区是在包裹输入速度大于该连接的输出速度时，储存后面来的包裹的，类似于排队。每个包裹在这个交换机经历的排队时间就是所谓的排队延迟。这个缓冲区本身也是有大小限制的，当排队的包裹已经占满了整个缓冲区时，新来的包裹会因为无法进入缓冲区而被直接丢弃，这被称为丢包。

排队延迟有很大的不确定性，因为它的大小完全是由传输时的网络状况决定，两个相同的主机在不同的时间发送同一个包裹，体验到的延迟可能完全不同，排队延迟是其中很重要的因素。但不确定性并不意味着我们不能定量地讨论这个问题，我们可以从统计的角度上去看每个包裹体验到的平均的排队延迟。

为了简化问题，在这里我们假设交换机的输出缓冲区无限大，令某一输出缓冲区对应的网线的传输能力为R比特每秒，每个包裹的大小为L比特，每秒平均有a个包裹（可能在每一秒的不同时刻）到达该输出缓冲区。那么，从平均来看该缓冲区每秒有La比特到达，有R比特的输出能力，那么平均输入和平均输出比就为La/R，我们称之为流量强度(Traffic intensity)。我们要研究排队延迟，就需要研究平均输入和平均输出的大小关系，如果平均输入大于平均输出，那么无论其真实分布如何，这条队一定是越排越长的，而平均输入小于平均输出时，队一定是越排越短的，前者对应无穷大的平均延迟，后者对应有限的平均延迟。

那么平均输入等于平均输出时呢？实际上这个时候平均延迟也趋向于无穷大，因为在这个条件下最好的情况就是队伍长度不变，此时每一时刻进入这一缓冲区的比特都是常数。然而这是极其特殊的情况，绝大多数时候平均输入的分布是不均匀的。当平均输入不均匀时，就会存在输入大于输出能力的时间段，在这一时间段内，队伍长度就会开始增加。而输入的比特流是不连续的、以包裹为单位的，因此在队伍长度的增加不能在输入小于输出能力的时间段完全消除，队伍长度因此趋向于无限大，后续到达的包裹体验到的延迟也因此趋向于无穷大。

显然在没有包裹到达时，平均延迟为零，大致可以得到下列示意图：

现实中，输出缓冲区的大小是有限制的，当队伍长度趋向无穷大增长时，后续到达的包裹会被丢弃，此时在发出包裹的主机看来就是网络无响应，其可能会（取决于应用的需要）选择重发包裹(Retransimit)来保证包裹的正确传输。
排队延迟的数量级表现为从微秒级到毫秒级不等。

#### 3. 传输延迟(Transimission delay)
这一延迟类型在上一节中已多有讨论，在这里简单再复习一遍：对于一个大小为L比特的包裹，其经过一个承载能力为R比特每秒的传输介质时，传输延迟为L/R秒。这一延迟等于交换机或主机将包裹完全放入连接的时间，其数量级表现为从微秒级到毫秒级不等。

#### 4. 传播延迟(Propogation delay)
传播延迟是指单个比特通过任意传输介质的时间，这一概念和传输延迟是有很大不同的。传输延迟是进入传输介质花费的时间，而传播延迟是在传输介质中传播花费的时间。比特在各种介质中的传播速度从2x10^8 m/s到3x10^8 m/s不等，传播延迟等于传播距离/传播速度。传播延迟数量级通常表现为微秒级。
总的延迟

以上讨论的各个延迟都是针对不同的基础设施讨论的，处理延迟和排队延迟是针对交换机的，传输延迟和传播延迟是针对传输介质的。我们假设通信路径上有N条网线，那么就有N-1个交换机，通信的总延迟应当为： 
总传输+传播+处理+排队

## 10）吞吐量(Throughput)
什么是吞吐量？在我们下载任何东西时，显示的以Kb/s、Mb/s为单位的下载速度，就是吞吐量。我们通过两种简单的情形来研究这一概念，假设一个简单的网络仅由服务器和客户端组成，它们有a, b两种连接方式：
当两台主机之间只有一台交换机，左边的网线传输能力为Rs，右边的为Rc，那么假设服务器要向客户端传输一个大小为F比特的文件，由于该网络的构造简单，排队延迟和处理延迟为零，假设两台主机距离足够短，传播延迟也可以忽略。此时通信的延迟就只有传输延迟一项，那么这时候的吞吐量/下载速度也就是Rs和Rc中的最小值。这和管道中流动的液体的速度是一样的，瓶颈处的流动速度就是整个管道的流动速度。
推而广之，当两台主机之间有N个连接时，吞吐量/下载速度就是min{R1, R2, ..., Rn}。
在现实世界中问题往往没有这么简单，传输介质中并不是只有一个包裹在传输，因此每个包裹分配到的传输能力有所不同，而加上传播延迟等变量，吞吐量会呈现复杂的波动行为。不过基本上互联网的核心部分的速度是足够快的，在大多数时候网络终端处的连接决定了通信的吞吐量，接入网的传输能力是通信吞吐量的瓶颈。

## 11）什么是协议(Protocol)
在大致讨论完了构成互联网的硬件的全貌之后，我们将注意力放在软件上来。首先映入眼帘的就是大名鼎鼎的协议，我们可以在各种互联网相关的文档中发现这个词的大量出现。我们可以用两个人之间的交流来比喻协议的作用，例如小明想要向一个陌生人小李询问现在的时间（假设他手机没电了），他大概会这样开头：
你好，请问能打扰一下吗？
这句话代表了小明想要向小李对话，也即“初始化”这一次沟通。这个时候如果小李听到了并愿意作出回答，那么他会说：
好的，请问有什么事。
这一句话告诉小明小李成功接收到了对话的请求并且等待小明继续对话，于是小明问到：
请问现在的时间是多少，我的手机没电了，现在急着要知道时间。
小李听到了这一句话，并且他也拥有当前时间这一信息（或许是打开手机看了一眼），于是他回答：
现在是两点钟。
小明终于得到了他想要的信息，于是他感激地回答：
谢谢。
这句话同时也传达了结束对话的意愿，小李听到后点头微微一笑，于是这次对话就顺利结束了。

上面完整描述了一次顺利的对话，但不是每次对话都能这么顺利，如果在小明传达出想要对话的信号之后，小李默不作声，或者小李回答"What?"（小明不懂英文），那么这次对话请求就失败了。在这两种情况中，一种是小李拒绝对话，而另一种则是双方交流的协议（也即语言）不相同，因此也无法进行对话。

互联网中处处存在协议，可以说这本书大半部分都在处理协议相关的内容。协议定义了多个通信实体之间交换信息的格式和顺序，以及在传输和接收到某个信息或事件时采取的行动。

为了说明这一定义的涵义，我们以浏览器请求网页为例研究互联网上的协议的运作过程。当用户打开浏览器，在地址栏输入某个网址后，这台电脑就会首先向服务器发出TCP连接请求，服务器受到TCP连接请求后回复这一请求表示同意连接，在电脑受到回复后，TCP连接就成功建立了，接着电脑向服务器发出GET请求，要求获得这一网址对应的文件，服务器接着就将这些文件发送给电脑，最后两边关闭TCP连接，这一请求过程便结束了。


## 12）互联网协议栈
正如我们已经看到的，互联网是一个极其复杂的系统，即使是想要在单独一个方面去描述它也是不容易的，为了方便地描述互联网的全貌，我们使用层级结构来帮助我们。

什么是层级结构？我们用航空业来打比方（这也是书中的例子），让我们捋一捋一名乘客坐飞机时，会经历哪些步骤：首先他会通过各种渠道买到一张机票，来到机场登机楼之后，他会和他的行李一起经过安检，随后他的大件行李会被带走托运，接着通过登机口进入飞机，坐着飞机起飞，经过一段时间的飞行之后（可能中途会有转机），飞机降落，乘客从登机口下飞机，来到领取行李处等待认领自己的行李，最后在完成这趟旅程之时，他可能还会在网上给这次旅行打分。

仔细的观察这段描述，我们可以发现，这名乘客经过的步骤呈现出了一定的对称性：购票对应打分评价，行李安检和托运对应领取行李，登上飞机对应走下飞机，飞机起飞对应飞机降落。乘客的行为模式就像一个栈一样，相应的行为依次放入，再依次取出。

机票（购买）		机票（评价）
行李（安检、托运）		行李（认领）
登机口（登机）		登机口（下飞机）
飞机起飞		飞机降落
飞机沿航线飞行	飞机沿航线飞行	飞机沿航线飞行
这些分层之间是存在明确的逻辑关系的，例如，只有买了票的乘客才能进入登机楼进行行李安检，而经过了安检的乘客才能来到登机口登机。下一个分层的运作，是依赖于上一层提供的服务的。负责安检的员工不会去检查乘客的票，而到了飞机起飞的时候，也没有人会操心安检的问题，这些服务已经由上一层提供了，每一层都只需要做好自己事情即可。

从横向看，每一个分层的两端也是有一定的联系的，例如行李的安检、托运环节明确联系着行李的认领环节，乘客在托运和认领时拿着的行李是同一个行李，而托运本身也遵循一定步骤，才能确保乘客的行李能够即时地送到乘客手中。每一个分层都有自己独有的“协议”，通过出发地一端和目的地一端同层级之间的交流，整个乘坐飞机的流程就能够正常运作。

现在回到互联网协议栈上来，它分为五层，从上到下分别是：

- 应用层(Application)
这一层是最贴近我们日常生活使用的各类网络应用，如浏览器，电子邮箱，网络游戏等等。但是不要被字面意思误导了，这一层不等于网络应用本身，它只是提供了不同网络应用所需要的各类协议，如HTTP（浏览器用）、SMTP（电子邮箱用）、DNS服务（后面会提到）等等。数据在这里就如同机场的乘客，在不同层级会有不同的状态，在应用层的数据我们称之为信息(Message)。

- 传输层(Transport)
应用层的下一层是传输层，传输层负责包装传输应用层的信息(Message)。传输层有两种协议——传输控制协议(Transimission Control Protocol, TCP)和用户数据报协议(User Datagram Protocol, UDP)。前者提供了连接导向式通信，它保证了应用层信息的的有序、完整传输，还提供了数据流量控制，使得连接速度不同的两个主机可以顺利通信（连接速度不同可能会导致数据传输的不完整，这一点后面会讲），以及拥塞控制，这一功能可以在整个网络较为拥挤时对传输速度进行控制从而降低整个网络的堵塞程度。传输控制协议同时还会将较大的数据包拆分成小得数据包，增快传输速度（这一点在数学上可以证明）。与之相对的，用户数据报协议可以说是一名“一无所有者”，它仅仅提供最简单的打包和最基础的错误检查，但与之相对的，因为免去了这么多步骤，它的速度更快，可以说传输控制协议和用户数据报协议各有优点，不同的网络应用根据自己的需求可以选择不同的传输协议。经过传输层打包的数据我们称之为数据段(Segment)。

- 网络层(Network)
在这一层的数据我们称之为数据报(Datagram)，是在分组交换网络中传输的最小单位。可以看到，到了这一层之后，我们已经开始接触到互联网的基础设施了。网络层往往又被称为IP层，尽管它所涉及到的协议不止IP(Internet Protocol address)协议。IP协议是互联网非常重要的一个协议，在上一层传输层中有两种协议可供选择，但到了网络层，我们只能选择IP协议，也就是说，任何涉及到互联网的应用都必然绕不开IP协议。

那么IP协议具体是做什么的呢？IP就相当于网络世界当中的地址，发挥着类似邮政服务中的邮政编码的作用，只有提供了地址，通信路径上的交换机才能找到这个包裹接下来的传输方向（路由表也是基于IP建立的）。

除了IP协议之外，网络层也是各种路由协议(Routing protocol)的实现层。

- 数据链路层(Link)
这一层主要关注的是将包裹从当前交换机传输到下一个交换机，网络层将数据报交给数据链路层，数据链路层将数据报打包成帧(Frame)再交给下一个交换机，下一个交换机的数据链路层再将帧打开提取出其中的数据报交给其网络层，找到传输方向后将数据报再次交给数据链路层继续传输。

这一层提供的服务取决于连接中数据链路层协议的实现方式。例如一些数据链路层协议会提供针对下一个连接的数据的可靠传输、错误检查等等，这和传输控制协议所提供的端到端的各种服务是有区别的。

- 物理层(Physical)
与数据链路层负责将包裹传递给下一个连接相对应，物理层主要负责将比特传输给下一个连接。这一层的协议完全依赖于底层的传输介质，例如以太网有多种物理层的协议，分别对应于双绞线、同轴电缆、光纤等等。在不同的介质中，比特以不同的物理形态被传输。

与开放式系统互联(Open System Interconnection Model, OSI)模型的联系

开放式系统互联模型形成于上世纪七十年代，早于上面所讨论的五层模型，其中包含七层协议栈：

- 应用层(Application)
- 表示层(Presentation)
- 会话层(Session)
- 传输层(Transport)
- 网络层(Network)
- 数据链路层(Data Link)
- 物理层(Physical)
其中的应用层、传输层、网络层、数据链路层和物理层的定义与五层模型中的大同小异，基本不需要做额外区分，而多出来的表示层和会话层则有所不同。表示层负责提供“翻译”数据的功能，例如数据压缩、加密以及基本的格式规定，都是表示层负责的；会话层则负责设置和维护两台主机之间的连接，保持通信的同步。

为什么如今五层模型被使用地更多，而七层模型被抛弃了呢，主要是因为实际上互联网缺少七层模型当中的表示层和会话层。但这不代表它们是多余的，相反，对于许多应用来说，数据格式和连接的稳定性是非常重要的，但互联网没有提供这些服务，这些功能依靠应用的开发者实现。实际上这也是互联网的特性之一，后面我们会越来越多的看到，互联网通过将复杂功能的实现安排在终端部分，维持了自身结构的简单和高效。

## 13）封装(Encapsulation)
从上面对五层模型的介绍，我们可以看到数据在不同的层级有不同的名字，这不仅是为了做出区分，更有实际的含义。事实上，每经过一个网络层级的处理，数据包裹都会因为封装变得更大。
五层模型中，除去物理层之外，每一层都会在数据包裹中添加这一层的协议相关信息，我们称为信头(Header)。将信头添加到数据包裹上的过程就被称为封装，原来的也即来自上一层的数据包裹称为负载(Payload)。从上图中我们可以看到，数据包裹传输的过程也就是不断封装，拆开，再封装的过程。

## 14）网上的弊端
互联网以其便捷、高效闻名，但其中的阴暗面也广为人所知。透过互联网的攻击是我们每天都在面对的货真价实的威胁，这种攻击是基于恶意软件(Malware)实现的。

恶意软件通常通过互联网传播，其利用软件的漏洞入侵宿主系统，从而达到破坏系统、损坏文件、窃取私密信息等等目的，小到弹窗广告，大到勒索病毒，都可以被归类为恶意软件。恶意软件根据其行为，又可以分为两类：

## 15）病毒(Viruses)
这种恶意软件需要宿主电脑的特定行动触发，非常典型的如邮件病毒，其通常附着在邮件的附件中，一经用户下载，它就会进入宿主电脑作恶，往往还会借用宿主电脑的资源发出数千封邮件进行进一步的传播。可以说其行为与生物界的病毒十分相像。

>区分于：蠕虫(Worms)
这是主动型的恶意软件，它通常经过联网应用的漏洞感染宿主电脑，感染后它会利用宿主电脑的网络扫描互联网上其他运行相同联网应用的电脑，进一步传播。

在恶意软件的制作者通过其感染了数千台电脑后，他便能够通过控制这些电脑组织成一个僵尸网络(Botnet)，利用这些资源搞一波更大的。这里就不得不提到拒绝服务攻击(Denial-of-Service attack, DoS)。如果说恶意软件主要针对的是个人电脑，则拒绝服务攻击则主要针对服务器，它的主要目的在于使服务器瘫痪，导致正常用户无法访问该服务器，其有以下三种实现方法：

- 漏洞攻击(Vulnerability attack)，原理和恶意软件类似，都是利用软件的漏洞使目标主机系统瘫痪、崩溃，无法处理网络请求。
- 带宽消耗(Bandwidth flooding)，向目标服务器发送大量数据包，超过其接入网承载能力上限，导致网络拥塞，正常用户的请求无法到达服务器。
- 资源消耗(Connection flooding)，与目标服务器建立大量TCP连接，将其端口资源（这个后面会讲到）消耗殆尽，正常用户无法与服务器建立TCP连接。

如果攻击者只有一台主机，而想要以第二或第三种实现方式实施攻击，是有难度的。首先服务器的带宽和资源十分充裕，一台个人电脑是无论如何也不可能将其消耗殆尽的，要想达到这样的效果，就需要带宽和资源更加充裕的主机来实施攻击，这非常昂贵；其次，来自单一源的攻击是很容易被目标服务器的安全程序发现的，攻击很有可能在还未成规模前就被防火墙掐灭了，成功率堪忧。

因此，分布式拒绝服务攻击(Distributed Denial-of-Service attack, DDoS)横空出世，解决了上述两个问题，攻击者可以利用前面提到的僵尸网络，通过多个节点达到同样的效果，而攻击者只需要在一台电脑上操控这一切，便可达到四两拨千斤之功效。


>数据嗅探器(Packet sniffer)
嗅探器的原理十分简单，将它布置在接入网的关键节点上，然后将经过这一节点的所有数据包都复制下来，然后本地保存或者通过互联网发送到使用者处。嗅探器不一定是恶意的，实际上很多网络数据包分析软件也是嗅探器的一种——我们使用它来监测自己电脑上的网络数据包。但嗅探器的恶意使用可以让不法分子获得包含敏感信息的数据包，若他们能将之解密的话，他们就可以获得其中的信息，这是十分危险的。