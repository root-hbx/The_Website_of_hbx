# Chapter2 应用层
互联网是一种服务，世界上数以亿计的程序仰仗这一服务运行。要让世界上运行在不同地区、不同型号、不同性能的机器上的不同程序相互通信，是一项浩大的工程。就像语言让不同的人相互交流那样，应用层的诸多协议也使得运行在不同主机上的程序得以交换信息，进而用户提供各种服务。本章正是要介绍应用层的基本概念以及常用的应用层协议，有了这些知识，我们就能对当今使用极其广泛的各类网络程序有一个基本的认识。

## 网络架构

假设我们想要开发一款联网应用，抛去商业方面的考虑，我们会对其开发细节做诸多规划，但在制定这些更加细节的计划之前，我们需要确定这个应用的网络架构，大致来说，是从目前的两种主流架构中做出选择：主从式架构(Client-server model)和点对点(Peer-to-peer, P2P)架构。

- 主从式

顾名思义，在这种架构中，有一个主要的服务器和诸多从属的客户端构成，组成类似星型的网络架构。典型的例子有视频网站，每个用户都直接向主服务器请求视频资源，服务器根据客户端的请求以相应的资源响应。在这种架构中，客户端之间是没有通信的，每个客户端只会和处于中心地位的服务器通信。

在今天的大型网络应用中，一个服务器显然不足以应付数量庞大的用户请求，加之以安全性的问题，目前绝大多数互联网公司都选择使用数据中心(data center)来充当主从式架构中的“服务器”角色。这个虚拟的服务器通常由成百上千个真实存在的服务器组成，跨国公司如谷歌通常还会在全世界各地搭建多个数据中心来提升用户体验。

- 点对点

点对点架构与主从式架构恰好相反，它将网络架构对服务器或者数据中心的依赖降至最小，将后者中通常由服务器承担的通信压力分配到各个用户终端上，也即P2P网络术语中的用户群(peers)。大名鼎鼎的比特彗星(BitTorrent)就是典型的P2P应用，许多视频会议应用也使用了P2P架构。

P2P作为去中心化的典型代表，其突出特性就是自我扩展性。例如磁链下载，当没人使用这个磁链时下载速度很慢，但是人越多下载速度越快（因为提供服务的用户群变大了），下载速度快能进一步吸引更多人使用这个磁链。

## 进程通信

应用程序在操作系统中是以进程(process)的形式存在的，来自互联网的数据首先要经过操作系统才能被进程获得，当应用程序使用各种互联网服务时，**它实际上是通过使用操作系统提供的服务来使用互联网服务的。**这些服务以**网路接口(Network socket)**的形式提供给开发者，即应用程序接口(Application Programming Interface)。网络接口以类似门的形式连接了互联网协议栈中的应用层和传输层：

![](https://pic3.zhimg.com/80/v2-4a7883a303caaba8ddec26abcff6b276_1440w.webp)

提供服务的另一层含义就是传输层以及传输层之下的层级功能对于应用开发者来说是给定的，开发者对于应用层的行为有完全的控制权，但是到了应用层以下则只能选择传输协议（TCP或UDP）以及调整传输层相关的参数。

就像每个主机在网络中都有一个IP地址对应，一个主机当中也有多个进程，也需要类似地址的信息来标记每一个进程，这就是端口号(port number)。因此要想和其他主机中的某个进程通信，需要两个信息：主机的IP地址和进程的端口号。端口号通常是固定分配给各个应用的。

一个联网应用在正常工作时可以分为两部分：一个**发起通信请求**的客户进程（如浏览器）和一个**接受通信请求**的服务进程（服务器中的程序）。进程在通信中扮演什么角色完全是由它的行为决定的，和它在什么主机上运行没有关系。例如在P2P文件传输中，一个进程既可以是客户进程，也可以是服务进程；它在下载文件时是作为客户进程，而它在提供文件块时是作为服务进程。为了强调这个定义的重要性，此处将原文贴上：

> In the context of a communication seesion between a pair of processes, the process that initiates the communication (that is, initially contact other process at the beginning of the seesion) is labeled as the client. The process that waits to be contacted to begin the session is the server.  

## 选择传输层协议

### 需求分析

前面提到，开发者在传输层需要选择传输协议。因此我们需要首先了解手头的这个应用需求几何，再根据需求来选择特定的传输协议（就像旅行时选择出行方式那样），总的来说，应用的需求可以概括为以下四种：

- 可靠的数据传输

一些应用要求数据的完全正确，在这些应用中数据的丢失或者错误会造成不可容忍的错误，因此要求传输协议通过某种手段来保证数据的完整和正确。有这种需求的应用有浏览器、文件传输程序、电子邮箱等等。注意，不能一厢情愿地认为所有联网应用都要求可靠的数据传输，实际上很多实时应用对于数据的完整性和正确性要求很低，例如实时聊天软件，在聊天时画面和声音的偶然失真是可以接受的，但是为修复这些失真而导致的延迟却往往是用户所不希望的。

- 稳定的吞吐量

网络的吞吐量并不是一成不变的，实际上从上一章的讨论可以看出，网络中的“交通状况”总是在波动变化着，加之主机中其他的联网进程，一个进程得到的吞吐量也总是在波动变化。这是部分应用所不希望看到的，它们对于吞吐量有基本的要求，若吞吐量低于某个数值，则它们无法正确工作，例如对于语言通话来说，假设它以固定的速率r bits/sec编码语音信息，那么为了保证数据的正确发送，提供给这个进程的吞吐量也至少要是r bits/sec。

- 低延迟

低延迟是许多实时应用的基本要求之一，还是以语音通话为例，用户希望通话的延迟能够低至无法察觉的程度，否则这会给通话的双方带来不自然的停顿，没有人希望自己说完之后要等待半分钟才得到回应。

- 安全

安全是互联网中老生常谈的问题。传输协议通常经过发送端加密，接收端解密的方式来保证安全，这样做可以保证即使包裹被中途拦截，拦截者也难以获得其中信息。除了加密这一传统的保密手段，传输协议还会使用诸如端点认证(end-point authentication) 这类技术来提升安全性。

![](https://pic1.zhimg.com/80/v2-52a9386d1a0e214dc136e93f517f8fb0_1440w.webp)

现实中，传输协议无外乎TCP和UDP两大种类，我们来看看TCP和UDP提供了哪些特性。

- TCP  
    TCP协议在分组交换网络中模仿电路交换网络，使用了TCP连接这一概念。若需要使用TCP协议进行传输，首先需要建立TCP连接，两个主机经过“三次握手”来交换建立连接所必需的信息。在通信完成之后，两个主机也需要关闭TCP连接。这些连接保证了TCP的**可靠数据传输**，任何丢失的或者出错的包裹都会被检测到并重新发送直到接收方顺利接收为止。在后面我们同样会看到TCP的拥堵控制(congestion-control)技术会最大化整个网络的传输能力（在大部分传输协议都是TCP的情况下），力图使每个TCP连接都能均分网络的传输能力。  
    总的来说，TCP可以提供完全的可靠数据传输。  
    
- UDP  
    

UDP是一个极其简单的传输协议，总的来说它只做了一件事情：把包裹传给下一层（以及一个效果很弱的错误检查机制）。因此UDP并不保证数据的可靠传输，包裹随时可能丢失，或者在传输过程中出现错误内容。UDP没有提供以上任何特性之一。

- SSL

由于TCP和UDP都没有提供任何安全保证，SSL(Secure Sockets Layer)应运而生。它本质上只是**TCP的加强版，**除去TCP提供的服务之外，加入了加密、数据完整性检验(data integrity)和端点验证功能。SSL的安全部分是在应用层实现的，因此想要使用SSL的开发者需要在程序中添加实现了SSL的第三方库。

经过上述介绍我们会发现当今互联网的传输协议缺少稳定吞吐量和低延迟的保证。这些功能在如今的联网应用中是在应用内部完成的，开发者尽其所能最大化利用已有的网络条件来给用户提供可靠的服务，当然在网络条件本身过于差劲（网络延迟高、带宽不足）时，技术也无力回天。下面列举各种应用使用的应用层协议和传输层协议：

![](https://pic4.zhimg.com/80/v2-d376ac1ba9faf505b238a0a3304acb23_1440w.webp)

## 应用层协议

上面我们讨论了将信息(message)通过网络接口传递给传输层，但信息不是凭空产生的，它的具体形式是由应用层协议规定的，总体来看，应用层协议回答了以下关于信息的四个问题：

- 这份信息的种类是什么？它是作为回复还是请求？
- 不同的数据是如何在信息中排布的？某一种数据在信息包裹中的哪个位置？
- 这些数据的含义是什么？它们遵循什么样的语法？
- 当进程收到这种信息时应该执行什么动作？又需要发送什么响应信息？

接下来，我们将具体介绍当今互联网最为著名也是使用最广泛的几个应用层协议，让我们带着这几个问题继续。

## 万维网和超文本传输协定

一提到互联网，我们首先映入脑海的很可能是万维网(World Wide Web, Web)，虽然它霸占了我们绝大多数的网络体验（移动端应用也是基于Web技术的），但万维网不等于网络，它只是互联网上的一个庞大的应用而已。而支撑这个庞然大物重要的应用层协议之一便是超文本传输协议(Hypertext Transfer Protocol, HTTP)。

### 网页

在开始介绍HTTP之前，我们需要先了解平时使用浏览器浏览的网页究竟是什么。**网页是由一组文件构成的**，这些文件可以是超文本标记语言(Hypertext Makeup Language, HTML)文件、图片、可交互程序、视频等等，大部分的网页都由一个基础的HTML文件加上被这个文件引用的其他文件构成。

基础HTML文件通过网址(Uniform Resource Locator, URL)引用其他文件，我们也通过网址来访问网站。网址可以视作万维网中的地址，浏览器使用这些地址来找到需要的网页，而网址实际上和ip地址有千丝万缕的关系，这一关系会在介绍域名系统(Domain Name Systeam, DNS)的部分详细介绍。下面展示了一个典型的网址：

> [http://www.someschool.edu/someDepartment/picture.gif](https://link.zhihu.com/?target=http%3A//www.someschool.edu/someDepartment/picture.gif)  

网址的结构很简单，它由服务器名称(hostname)和路径组成，服务器名称指定需要访问的服务器，而路径则指定这一文件在服务器文件系统中的位置。在上面这个网址中，`www.someschool.edu`是服务器名称，`someDepartment/picture.gif`是文件路径。

### HTTP总览

HTTP这一应用层协议是基于TCP的，也就是说任何想使用HTTP的应用都必须选择TCP作为传输层协议。我们还是以浏览器为例，当用户在浏览器中点击了一个超链接或者在网址栏中输入了网址点击访问时，浏览器所在的主机就会向网址中指定的服务器发出TCP连接请求，当主机和服务器建立了TCP连接后，主机接着向服务器请求网址中指定的文件路径对应的文件，服务器在收到文件路径之后在本地存储中找到这个文件并打包发送给主机，主机收到文件后便按照一定的逻辑将它显示在屏幕上，这就是我们访问一个网页的全过程。

HTTP协议中不要求客户端或者服务器任何一方保存关于通信的另一方的任何信息，例如一个主机如果在几秒内多次向服务器请求同一个文件，服务器并不会因为之前已经发送过相同的文件而做出不一样的响应，相反，服务器完全不记得之前给这个主机发送过相同的文件这件事，它只会和之前一样忠实地履行协议，将相同的文件多次发送出去。

### 持续\非持续连接

我们上面提到了HTTP是基于TCP的，并简单介绍了访问网页的过程，但是忽略了一个问题：**何时关闭TCP连接？**流行的TCP连接的管理方式有两种：传输一个文件就建立一个TCP连接；只建立一个TCP连接传输所有的文件。这两种方式就分别对应非持续连接(Non-persistent connections)和持续连接(Persistent connections)，HTTP同时支持两种连接管理方式，大部分浏览器默认使用后者，但也可以调整为使用非持续连接。

让我们一步一步观察网页访问过程来对比两种方式，假设我们需要访问下面这个网址，它对应的网页由一个基础HTML文件和五个图片文件组成：

> [http://www.someschool.edu/someDepartment/home.index](https://link.zhihu.com/?target=http%3A//www.someschool.edu/someDepartment/home.index)  

当使用非持续连接时：

1. 浏览器所在的主机找到服务器名称`www.someschool.edu`所对应的ip地址，浏览器进程通过端口80（预留给HTTP的默认端口）和主机建立TCP连接
2. 主机向服务器发送HTTP请求信息，其中包含文件路径`someDepartment/home.index`
3. 服务器通过接口收到请求信息，在本地存储中找到对应文件放入响应信息中交给接口
4. 服务器请求关闭TCP连接，等待确认主机已经顺利接收到响应信息，如果主机没有接收到，则连接还不能关闭
5. 主机接收到响应信息得到基础HTML文件，并发送信息告诉服务器可以关闭连接，连接关闭
6. 主机解析该HTML文件发现它还引用了五个图片文件，于是重复五遍上述步骤以此得到这五个图片文件

对于一个由六个文件组成的网页来说，主机需要和服务器建立六个TCP连接来获得全部文件。但上面的步骤不一定是完全按顺序进行的，浏览器可能并行地和服务器建立多个连接来提升访问速度。如今的大多数浏览器默认会同时维护五到十个TCP连接。

为了更方便地衡量访问网页所需的时间，我们将以时间为单位的延迟抽象成来回通信时间(Round-Trip Time, RTT)，一次来回通信时间所对应的时长就是送发送信号方发送信号到接收信号方发送的响应信号到达发送信号方所耗费的时间：

![](https://pic1.zhimg.com/80/v2-ff19890f06409a5f904894352a86a6c8_1440w.webp)

对于非持续连接来说，每次接受一个文件都需要建立一个TCP连接，其中包含“三次握手”，前两次握手便耗费了一次来回通信时间，在最后一次握手时，浏览器所在的主机发送的TCP回应信息可以同时包含HTTP请求信息，于是服务器接收到TCP回应信息成功建立TCP连接的同时也接收到了HTTP请求信息，于是发送对应的文件给主机。整个请求单个文件的过程耗费了两次来回通信时间：

![](https://pic2.zhimg.com/80/v2-078c2334fe76a0eb86f1f3a4a298af7d_1440w.webp)

非持续连接每一次建立TCP连接都会消耗额外的时间（每次建立连接都需要一次来回通信时间）以及服务器的资源（TCP缓冲区、端口等等），相比较而言，持续连接使用一个TCP连接来完成一个网页所有文件的传输，节省了时间和资源。持续连接会自动关闭持续一段时间闲置的TCP连接。

### HTTP信息格式

### 请求信息(HTTP request message)

首先看一个请求信息的例子：

```http
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agent: Mozilla/5.0
Accept-language: fr
```

这个请求信息只有五行，其中第一行是请求行(request line)，剩下四行都是请求头(header lines)。请求行在请求信息格式中只有一行，分为三个部分：请求方法、网址中的文件索引和使用的HTTP版本。接下来的Host就是服务器名称；Connection: close告诉服务器使用非持续连接，在发送完请求行中指定的文件后就关闭TCP连接；User-agent告诉服务器发送这个请求的浏览器是什么版本，上述例子中的Mozilla/5.0对应FireFox浏览器；Accept-language告诉服务器用户希望获得什么语言版本的文件，如果没有这个版本的文件，则服务器发送默认的语言版本，HTTP中还有很多本地化相关的请求头。下面是HTTP请求信息的格式：

![动图封面](https://pic2.zhimg.com/v2-2c27374b68828672fb994403cedbeda1_b.jpg)

可以看到，HTTP中所有的行都是以一个回车符和一个换行符结尾的，并且在头部信息的最后一行下面还有空白的一行。接下来的Entity body使用与否和请求方法有关，GET方法并不使用这个信息域。

HTTP中的请求方法只有GET, POST, HEAD, PUT和DELETE这五种，下面分别介绍这五种请求方法：

- GET

GET方法是最常用的请求方法，它用于向服务器请求请求行中文件索引对应的文件，在使用GET方法时，Entity body为空

- POST

POST方法和GET方法功能基本相同，但它用于**带输入的HTTP请求**，例如用户在搜索引擎中搜索某样东西时，输入的搜索关键字就存储在Entity body中。注意，带输入的HTTP请求并不是只能使用POST方法，我们一样可以使用GET方法来完成类似搜索的功能，这是通过在网址末添加搜索关键字中完成的。例如当我们访问谷歌时，网址为[https://www.google.com.hk](https://link.zhihu.com/?target=https%3A//www.google.com.hk)，然后我们在输入框中输入"HTTP"并进行搜索，此时访问的网址就会变成[https://www.google.com.hk/search?q=HTTP](https://link.zhihu.com/?target=https%3A//www.google.com.hk/search%3Fq%3DHTTP)（实际上的网址可能很长）

- HEAD

HEAD方法也是请求一个文件，但是它要求响应信息中只包含信息头，这一方法通常用于开发调试

- PUT

用于将文件放入特定的服务器中，文件内容在Entity body中

- DELETE

用于删除服务器中的特定文件

### 响应信息(HTTP respond message)

和上一节相同，我们先给出一个具体的响应信息作为例子：

```http
HTTP/1.1 200 OK
Connection: close
Date: Tue, 18 Aug 2015 15:44:04 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT
Content-Length: 6821
Content-Type: text/html

(data data data data data ......)
```

类似请求信息，响应信息的第一行被称为状态行(status line)，剩下的六行都是响应头(header line)，状态头分为三个部分：HTTP版本、状态码和与之对应的状态信息，例子中的状态码为200，状态信息为OK，意味着一切正常，浏览器可以顺利得到请求的文件。接下来的Connection: close告诉浏览器服务器将要关闭这个TCP连接；Date表明了这个响应信息被创建的时间，而不是响应信息中的文件被创建或者上一次修改的时间；Server和请求信息中的User-agent类似，告诉浏览器创建这个响应信息的服务器版本；Last-Modified是响应信息中的文件被创建或者上一次修改的时间；Content-Length是响应信息中的文件的大小，单位是byte；Content-Type表明相应信息中文件的类型，在这里是html文本文件；最后的(data ...)就代表响应信息中的文件。响应信息的结构如下所示：

![](https://pic3.zhimg.com/80/v2-2749858995d9625ff33ff24c247e36e2_1440w.webp)

下面介绍一些常见的状态码和状态信息组合：

- 200 OK: 请求成功
- 301 Moved Permanently: 请求的文件已经被永久移动到其他位置，服务器会将新位置放在响应信息中的Location响应头中，浏览器会据此自动发起第二次请求
- 400 Bad Request: 一个非常宽泛的错误码，表示服务器无法理解请求信息
- 404 Not Found: 服务器中无法找到请求的文件
- 505 HTTP Version Not Supported: 服务器不支持请求信息指定的HTTP版本

### Cookies

前面提到，Web服务器是不会保存客户端的信息的，这是出于性能的考虑才这么设计的。但是随着互联网发展，网页逐渐有了识别不同用户的需求，识别不同用户就可以进一步提供更好的个性化服务。Cookies技术就是识别用户的技术之一，它将cookie信息保存在客户端本地，在需要时发送给服务器来让服务器，服务器再通过其维护的数据库来识别不同用户，cookie信息可以看作服务器中数据库里的用户信息索引。

cookie提供多项有用的功能，它使Web服务器能够在客户端上保存用户在网站上的行为（比如添加购物车），跟踪用户的浏览活动（点击特定链接、登录）；它还使浏览器能够保存用户在表单中输入的信息，例如登陆用的用户名和密码等，方便用户使用。

下面我们来看一个使用cookie的具体例子。Susan是ebay的忠实用户，经常使用ebay进行网上购物，但她今天第一次使用Amazon。当她访问Amazon的请求到达服务器之后，Amazon的服务器会识别到这个主机在数据库中没有对应的用户信息，随后它会在数据库中新增一个条目，对应一个新的索引值。随后服务器将这个索引值放入响应信息中发给Susan的电脑，这个索引值可能是这样的：

```http
Set-cookie: 1678
```

Susan的浏览器在响应信息中发现这一行之后，就将这个值放入其维护的cookie文件中，对应Amazon这个网站。在Susan后续访问Amazon时，浏览器都会在其维护的cookie文件中寻找Amazon对应的cookie值并插入请求信息中，于是在浏览器发送的每个请求信息在请求头中都有这样一行（只要她没有清空cookie记录）：

```http
Cookie: 1678
```

而每次Susan在Amazon网站上的行为都可以被Amazon的服务器记录到1678对应的条目中（想想或许有点可怕），这样Susan就能从Amazon网站上获得更加个性化的服务，而Amazon也有办法从Susan那里赚更多钱了。当Susan在Amazon网站上注册了一个新的帐号之后，服务器中匿名的cookie条目就会和这个帐号的条目合并。上述cookie的运作过程图示如下：

![](https://pic4.zhimg.com/80/v2-7ecd15d4ea9cd5ad79cb1ff9f72d078b_1440w.webp)

### 网页缓存(Web Caching)

缓存就是用于提升访问速度的存储数据的硬件或者软件组件。网页缓存，又名代理服务器(Proxy Server)，指代替原Web服务器响应主机的请求信息的服务器，其通常由网络服务提供商(ISP)部署在距离主机更近的地方，可以提升附近这片区域访问网页的速度，至于网页缓存是如何做到这一点的，让我们看一个例子。假设我们想要访问`www.bing.com`这个网站：

1. 浏览器首先和网页缓存建立TCP连接，发出请求信息
2. 网页缓存在其存储中寻找这个请求信息对应的文件，如果找到了的话就发给浏览器
3. 如果没有找到，则网页缓存和原Web服务器建立TCP连接，接着请求浏览器所请求的文件，原Web服务器将文件放入响应信息中发回给网页缓存
4. 网页缓存接收到响应信息，将其中的文件存储在本地内存中，然后再给浏览器以响应信息的形式发送一份拷贝，浏览器最终接收到响应信息

![](https://pic2.zhimg.com/80/v2-e4f8b497ad48c6e08c5ba66ee99ab8a5_1440w.webp)

由于主机和网页缓存的通信不需要经过互联网，而是在接入网内完成的，因此速度要比直接访问原Web服务器要快得多，并且由于显著地减少了接入网和互联网通信的流量，也变相提升了接入网的网络连接质量。

我们来看一个具体的例子来感受网页缓存的优势，假设一所大学的网络由一个网络服务提供商提供，接入网中连接的速度为100Mbps，而接入网和互联网的连接速度为15Mbps：

![](https://pic4.zhimg.com/80/v2-7eaf925e173a7ac3d330193f793a2013_1440w.webp)

同时假设该接入网中只有Web相关的流量，忽略请求信息，接入网中平均每秒有15次网页请求，每次请求的响应信息大小平均为1Mbits；并且认为互联网的平均延迟为2s。那么接入网中连接的流量强度为： 

而在接入网到互联网的连接中的流量强度为： 

这可不得了，我们在上一章说过，当流量强度趋向于1时，通信延迟将趋向于无穷，我们需要升级接入网的规格来降低延迟，现在我们有两个选择：1）升级接入网到互联网的连接至100Mbps 2）在接入网中设置网页缓存。

第一个选择可以将接入网到互联网的连接的流量强度降至0.15，此时总的通信延迟大约为2s。

第二个选择相比而言更加经济，要估算这个情况下的延迟，我们需要知道缓存的命中率，在实践中这个值通常在0.2至0.7之间，我们往低了取，认为命中率为0.4，也就是说有60%的网页访问需要经过互联网，而剩下的只需要在接入网内完成。此时接入网到互联网的连接的流量强度降至0.6，而可以认为接入网内的通信延迟只有传输延迟，那么总的延迟就是： 

这个数值甚至比直接升级连接得到的延迟还要低很多。

### 条件性GET(The Conditional GET)

网页缓存甚好，但这一系统还有一个问题需要我们解决：存储在其中的文件可能过期。网页随时可能更新，当浏览器请求网页时，若网页缓存没有及时更新文件而把过期的文件传给了用户，那就是帮了倒忙，我们需要一个办法来更新网页缓存中的文件，这就是条件性GET. 

还是来看一个例子，假设我们用浏览器访问`www.exotiquecuisine.com/fruit/kiwi.gif`，浏览器向网页缓存请求这个文件，未命中，于是网页缓存接着向原Web服务器发出请求：

```http
GET /fruit/kiwi.gif HTTP/1.1
Host: www.exotiquecuisine.com
```

Web服务器发回响应信息：

```http
HTTP/1.1 200 OK
Date: Sat, 3 Oct 2015 15:39:29
Server: Apache/1.3.0 (Unix)
Last-Modified: Wed, 9 Sep 2015 09:23:24
Content-Type: image/gif

(data data data data data ...)
```

网页缓存将文件和最后修改时间Last-Modified一起存储在本地，然后再将拷贝放入响应信息发给浏览器。假设三周后浏览器又向网页缓存请求相同的文件，这时候网页缓存需要先确定本地存储中的文件是否过期，于是它向原Web服务器发送请求信息，其中请求方法为条件性GET：

```http
GET /fruit/kiwi.gif HTTP/1.1
Host: www.exotiquecuisine.com
If-modified-since: Wed, 9 Sep 2015 09:23:24
```

If-modified-since是条件性GET中必须的一行请求头，其中值和三周前网页缓存接收到的响应信息中的Last-Modified中的值相同。在使用条件性GET时，只有文件在这个时间之后被修改过，服务器才会发送文件，否则如果该文件并没有被更新，服务器只会返回一个响应头：

```http
HTTP/1.1 304 Not Modified
Date: Sat, 10 Oct 2015 15:39:29
Server: Apache/1.3.0 (Unix)

(empty entity body)
```

网页缓存根据状态码304 Not Modified得知本地存储中的文件是最新的，于是将其拷贝放入响应信息中发给浏览器。

## 电子邮箱(Electronic Mail)

上一节介绍的HTTP是近二十年才出现的新玩意，而这一节要介绍的电子邮箱则是互联网诞生早期就已经成形的元老级应用。和传统的邮政服务类似，电子邮箱是异步通信的，它主要由三个部分组成：用户代理(user agent)、邮箱服务器(mail server)和简单邮件传输协议(Simple Mail Transfer Protocol, SMTP)，和HTTP一样，SMTP这一协议是基于TCP的。

用户代理允许用户阅读、回复、转发、保存和撰写信息，微软的Outlook和各类网页端的邮箱都属于用户代理。邮箱服务器是执行SMTP的基础设施，它的存储区域分为单独属于各个用户的邮箱和共用的消息队列(message queue)，前者存放用户还未查看的邮件，后者存放等待被传输的用户发送出去的邮件。邮箱服务器之间使用SMTP进行通信，电子邮箱的整体结构如下：

![](https://pic1.zhimg.com/80/v2-21c47d5b564f0a8f46630ff3f62caea4_1440w.webp)

当用户A通过电子邮箱服务向用户B发送邮件时，A先通过用户代理将写好的邮件发送给A的邮箱服务器（邮箱服务器往往是共享的，此处所说“A的”并非暗示A拥有这个服务器），后者再通过SMTP和B的邮箱服务器进行通信。如果一切顺利，那么A的邮件很快就会出现在B的邮箱服务器中，等待B通过用户代理去查阅；如果A的邮箱服务器和B的邮箱服务器通信出现了错误，那么A的邮箱服务器会将这封邮件暂时存储在消息队列中，等待一段时间（比如30分钟）之后再尝试重新发送。若尝试一直失败，那么A的邮箱服务器会将这封邮件从消息队列中移除并给A发送一封邮件告诉他发送失败了。

### SMTP

前面提到，电子邮箱服务在互联网的早期就已经成形了，因此SMTP中存在诸多过时的规定。其中影响最大的可以说是对字符集的限制了：SMTP规定邮件的信息必须以7bit的ASCII编码形式传输，这在早期网络带宽有限，且多媒体尚不发达的条件下是合理的限制，但到了现在这个时代，7比特的限制就显得毫无必要甚至迂腐了。因为这个限制的存在，所有邮件中的多媒体内容在发送前都必须先转码成ASCII再进行发送，接收者在收到邮件之后又需要将编码成ASCII的内容解码回去。相比较而言HTTP就没有这种限制。

让我们更详细地看看A是如何将邮件发送给B的：

1. A打开了他的用户代理，撰写邮件并提供了B的邮箱地址，然后让用户代理发送这封邮件给B
2. A的用户代理将这封邮件发给了A的邮箱服务器，邮箱服务器将这封邮件放入消息队列中
3. A的邮箱服务器作为客户端（回忆服务器和客户端的定义）和B的邮箱服务器建立TCP连接
4. A的服务器将邮件通过TCP连接发送给B的邮箱服务器
5. B的邮箱服务器收到给B的邮件，将它放入B的邮箱中
6. B打开用户代理，看到了这封来自A的邮件

![](https://pic3.zhimg.com/80/v2-48efb14db4b696e1118846a9971c3c26_1440w.webp)

可以注意到在第四步，A的邮箱服务器**直接**和B的邮箱服务器建立了TCP连接，也就是说SMTP没有使用任何中间服务器，而是直接通过端到端的通信来完成邮件的传输。

SMTP传输信息的方式和人类交谈的方式很相似，通信的双方在TCP连接建立以后以一种类似“寒暄”的方式交换信息。在下面的例子中，C代表客户端，S代表服务器：

```text
S: 220 hamburger.edu
C: HELO creps.fr
S: 250 Hello creps.fr, pleased to meet you
C: MAIL FROM: <alice@creps.fr>
S: 250 alice@creps.fr ... Sender ok
C: RCPT TO: <bob@hamburger.edu>
S: 250 bob@hamburger.edu ... Recipient ok
C: DATA
S: 354 Enter mail, end with "." on a line by itself
C: Do you like ketchup?
C: How about pickles?
C: .
S: 250 Message accepted for delivery
C: QUIT
S: 221 hamburger.edu closing connection
```

上面这段通信中只传输了一封邮件，是从邮箱地址[alice@creps.fr](mailto:alice@creps.fr)发到邮箱地址[bob@hamburger.edu](mailto:bob@hamburger.edu)的，内容为：

> Do you like ketchup?  
> How about pickles?  

而正如服务器对客户端说的那样，邮件正文内容以"."作为结束。

客户端在上述通信中使用了HELO, MAIL FROM, RCPT TO, DATA, QUIT这五个命令，它们的含义不言而喻。而服务器的每条回复的格式都是“回复码 英文信息”，后半部分的英文信息是用于解释回复码的，因此服务器也可以只发送回复码，对机器来说其意义已经足够明确。

SMTP使用的是持续连接，这也就是说同一个发件人在这段时间内发送的所有邮件都会在这一次TCP连接中全部发送给收件方。在所有邮件发送完毕之后，客户端以QUIT命令结束连接。

从这个例子我们可以感受到SMTP完全以文本为中心的设计思路，这也解释了为什么多媒体内容必须要转换成7-bit ASCII格式才能进行传输，在SMTP中，所有内容都不做区分地被视作文本流。

一个典型的电子邮件有如下格式：

```text
From: alice@crepes.fr
To: bob@hamburger.edu
Subject: Searching for the meaning of life

(text text text text text ......)
```

正文部分都是ASCII格式的数据，在信息头和正文之间隔着空白的一行。信息头中必须包含From和To，Subject是可选的。

### 邮件访问协议(Mail Access Protocol)

SMTP是一个推送协议(push protocol)，也就是说这时客户端会将文件上传到服务器；相比较之下HTTP是一个拉取协议(pull protocol)，这时客户端会从服务器下载文件。这时候就带来了问题，回忆我们简略介绍过的邮件传输过程：我们可以用SMTP将邮件从发件人电脑发送到发件人的邮箱服务器，然后再用SMTP将邮件发送到收件人的邮箱服务器，但是收件人却无法使用SMTP从服务器得到邮件，因为SMTP不是一个拉取协议！

下面我们简单介绍用于读取邮件的三种协议。

### POP3(Post Office Protocol - Version3)

这是一个非常简单同时功能也很少的协议，在这个协议中，客户端通过端口110和邮箱服务器建立TCP连接，然后依次经历登录(authorization)、下载(transaction)和更新(update)三个阶段。协议规定服务器只有两种回复信息：+OK代表一切正常，-ERR泛指出现错误。

登陆阶段需要客户端提供用户名和密码，如果在命令行中使用telnet和一个POP3服务器通信，可能会收到这样的内容：

```text
$ telnet mailServer 110
+OK POP3 server ready
$ user bob
+OK
$ pass hungry
+OK user successfully logged in
```

如果敲错了命令，那么服务器就回复-ERR. 

接下来在下载阶段中，用户有两种模式选择：下载并删除(download and delete)和下载并保存(download and keep)，如果选择第一种模式，那么客户端会使用list, retr和dele这三个命令，用法如下：

```text
C: list
S: 1 498
S: 2 912
S: .
C: retr 1
S: (text text
S: ...... ...
S: ...... text)
S: .
C: dele 1
C: retr 2
S: (text 
S: ...... ......
S: ... ......
S: ...... text)
S: .
C: dele 2
C: quit
S: +OK POP3 server signing off
```

在客户端使用quit命令之后，服务器会将把被标记为删除的邮件从本地删除。可以看到这一模式会在将邮件保存在客户端之后将其从服务器中删除，如果想要在多个设备上阅读同一封邮件的话，我们需要用下载并保存模式。

### IMAP(Internet Mail Access Protcol)

POP3没有提供任何远程的邮件管理功能，用户只能将邮件下载到本地然后加以整理，而IMAP就弥补了POP3的这一缺陷，允许用户远程管理邮件，将服务器上不同的邮件归档以及查找邮件。IMAP还允许用户下载邮件的一部分（信息头以及各种多媒体附件）而不是像POP3那样默认下载整个邮件。

### HTTP

目前主流的邮件访问方式就是HTTP，在基于Web的电子邮箱中，无论是发件人还是收件人，都通过HTTP和各自的邮箱服务器通信，只有邮箱服务器之间的通信使用SMTP.

## 域名系统(Domain Name System, DNS)

前面我们提到过IP地址和网址中的服务器名称有关联，实际上服务器名称就是用于代替IP地址的，它作为Web服务器的别名存在，仅仅只是为了让网址更好记——[http://google.com](https://link.zhihu.com/?target=http%3A//google.com)总是比142.251.33.196这一串无意义的数字要好记得多，而将两者输入到浏览器的url栏中的效果是一样的。DNS就在其中发挥类似字典的作用，当浏览器遇到[http://google.com](https://link.zhihu.com/?target=http%3A//google.com)这样的网址时，它就“查阅”最近的DNS服务器得到对应的IP地址，接着才能访问对应的Web服务器。

### DNS服务

HTTP和SMTP的实现都有赖于DNS服务的正确运作，DNS是基于UDP运作的，占用端口53。

诸多协议都依赖于DNS服务，就像HTTP那样，DNS服务需要使用合理的服务器架构和缓存策略来尽可能地减少通信压力，后面将会谈到这些技术。DNS主要提供这些服务：

- 主机别名(Host aliasing)

一个主机只有一个IP地址，但是却可以有多个服务器名称。一般来说每个Web服务器都有一个较长的规范服务器名称(Canonical hostname)，然后还有其他较短服务器名称以类似助记符的形式存在，因此DNS不仅可以查询IP地址，也可以查询规范服务器名称

- 邮箱服务器别名(Mail server aliasing)

相信大家都遇到过“用户名被占用”的惨痛经历，而DNS服务让我们在电子邮箱上免于重名的痛苦，一个类似`bob@yahoo.com`的邮箱地址的实际地址可能是`bob@relay1.west-coast.yahoo.com`。而这一服务还允许邮箱服务器和Web服务器重名，某个公司的Web服务器和邮箱服务器的名称可能都是`enterprise.com`

- 负载均衡(Load distribution)

一个服务器可以有多个名称，同时一个名称也可以代表多个服务器，通过这个机制，DNS能够帮助降低服务器的通信压力。在一个服务器名称对应多个IP地址时，DNS服务器会返回一个IP地址的列表，其中排在前面的地址会被优先访问。如此，互联网公司就可以通过调整DNS服务器发送的列表的顺序来控制主机访问服务器的优先级，将通信压力平均分配给不同的服务器

### DNS服务器架构

DNS是如何构建一个分布式数据库系统的绝佳例子。一共有三个层级的DNS服务器：根域名服务器(root DNS servers)、一级域名服务器(top-levle domain DNS server, TLD DNS server)和权威域名服务器(authoritative DNS servers)，它们一起形成了树状的DNS服务器架构：

![](https://pic3.zhimg.com/80/v2-cd875816859ea09b278df1bf6a1a2a6e_1440w.webp)

- 根域名服务器：全世界一共有400多个根域名服务器，分别由13个组织管理，根域名服务器负责提供一级域名服务器的IP地址
- 一级域名服务器：一级域名服务器对应不同的一级域名，如com, org, net, edu, gov等，还有各个国家和地区的一级域名如uk, fr, ca等，一级域名服务器负责提供权威域名服务器的IP地址
- 权威域名服务器：这一层才是真正提供了翻译服务的部分，每个拥有可公开访问的服务器的组织都需要组建自己的权威域名服务器，将域名映射到服务器的IP地址上

不考虑缓存，在上述结构中一台电脑通过浏览器访问`www.yahoo.com`时，首先会访问根域名服务器得到com这个一级域名服务器的IP地址；接着访问com顶级域名服务器得到yahoo的权威域名服务器的IP地址；最后访问权威域名服务器才得到需要访问的Web服务器的IP地址。

在实践中还有一层服务器被称为本地DNS服务器，它们通常由网络服务提供商设置在距离用户最近的位置，当主机需要DNS服务时，首选便是本地DNS服务器，随后本地DNS服务器再作为代理人去从根域名服务器开始依次向下寻找到对应的权威域名服务器，才能得到需要的IP地址并最终发给主机，下面展示了一个繁琐的例子：

![](https://pic2.zhimg.com/80/v2-e1b36079bb16e4296763b11221537391_1440w.webp)

1. 主机访问网址`gaia.cs.umass.edu`，向本地DNS服务器发出DNS请求
2. 本地DNS服务器向根域名服务器发出DNS请求
3. 根域名服务器响应，本地DNS服务器得到edu一级域名服务器的IP地址
4. 本地DNS服务器向edu一级域名服务器发出DNS请求
5. edu一级域名服务器响应，本地DNS服务器得到麻省大学阿默斯特分校的权威域名服务器的IP地址
6. 本地DNS服务器向权威域名服务器发出DNS请求
7. 权威域名服务器响应，本地DNS服务器得到`gaia.cs.umass.edu`对应的IP地址
8. 本地DNS将IP地址发回给主机

并不是只有本地DNS能够作为代理人，其他服务器也可以作为代理人，下面展示了一个比较另类的例子：

![](https://pic1.zhimg.com/80/v2-63069ed0a47f9f7e321439ab73ee2490_1440w.webp)

一般来说，DNS服务的运行是遵循第一个例子的逻辑的。

在上面的例子中为了说明不同层级的功能，我们有意忽略了DNS缓存(DNS Caching)，简单来说，**DNS缓存就是令作为代理人的DNS服务器缓存它接收到的任何域名到IP地址的映射信息**。例如在上面第一个例子中，本地DNS服务器中可能就缓存了大量本校学生经常访问的域名的IP信息，主机只需要访问本地DNS服务器就可以直接得到IP地址。同时缓存信息存在时效性，映射信息随时可能失效，DNS服务器因此会给每个缓存信息设置时限（通常是两天），超过了时限之后直接丢弃该信息。

### DNS记录(DNS Records)和DNS信息(DNS Messages)

DNS信息是以资源记录(Resource Records, RRs)的形式存储在DNS服务器以及DNS信息当中的，一条资源记录有如下结构：

`resource records (Name, Value, Type, TTL)`

最后一条TTL代表time to live，它规定了这条记录将会在存储中存在多久，在下面的讨论中我们忽略这一项。Name和Value类似字典对，它们的意义取决于Type的值：

- Type=A，这时Name是域名，Value是这个域名对应的IP地址，例子(google.com, 142.251.33.196, A)
- Type=NS，这时Name是域名，Value是**知道如何获得这个域名对应的IP地址的服务器的域名**，这个服务器可以是一级域名服务器，也可以是权威域名服务器，由于这个类型的记录只有域名，因此还需要一个类型A的资源记录来记录这个域名对应的IP地址：例如edu一级域名服务器会有一条记录为(umass.edu, dns.umass.edu, NS)，与之对应的类型A资源记录为(dns.umass.edu, 128.119.40.111, A)
- Type=CNAME，这时Name是域名，Value是同一个服务器的规范服务器名称，例子(foo.com, relay1.bar.foo.comm, CNAME)
- Type=MX，这时Name是一个邮箱服务器域名，Value是同一个服务器的规范服务器名称，例子(foo.com, mail.bar.foo.com, MX)。A和MX是两个不同的资源记录类型，这就是一个组织的Web服务器和邮箱服务器可以拥有相同域名的原理

DNS信息的格式如下：

![](https://pic2.zhimg.com/80/v2-175f9c9c52903d2f108695b1d02bc619_1440w.webp)

其中Identification是独属于一对请求和响应信息的一串数字，在主机发出请求信息时生成，服务器再将请求信息中的Identification复制到响应信息中再发回给主机。这么做是因为DNS服务是基于UDP构建的，其没有提供区分不同连接的功能。

Flags顾名思义，我们只介绍其中比较重要的几个比特位。一个比特位表明这是请求信息(0)还是响应信息(1)；一个比特位指出这个响应信息是否是由权威域名服务器发出的；一个比特位指示主机是否希望这个服务器在它没有所需的资源记录时作为代理人；一个比特位告诉主机服务器是否能够作为代理人。

Questions自然是留给DNS请求信息的，其由两部分组成：需要查询的域名和请求的类型（也就是资源记录中的Type）；Answers对应响应信息，其中存放数个资源记录（之前讨论过返回数个IP地址的情况）；Authority记录其他可以响应同一个类型A请求的权威域名服务器；Additional information记录一些额外的信息，例如当请求为MX类型时，这个区域可能会有对应的IP地址，也即请求要求的规范邮箱服务器名称对应的类型A的资源记录

在详细介绍了DNS信息之后，我们可以再来看看我们究竟是如何访问网页的。从创建网站开始，一个组织或个人需要为他的网站建立数个权威域名服务器，接着向某个域名注册商(Domain Name Registrar)缴纳一定的费用并提供这些DNS服务器的域名和IP地址，这样他们就可以将对应的类型NS和类型A的资源记录（也许还有邮箱服务器的MX类型资源记录）计入他们的一级域名服务器中了。

接下来这个网站就可以被公众访问了，考虑DNS信息的类型，我们访问这个新建立的网站可能经过如下步骤：

1. 主机访问网址，向本地DNS服务器发出DNS请求
2. 本地DNS服务器向根域名服务器发出DNS请求
3. 根域名服务器响应，本地DNS服务器得到类型NS和类型A的资源记录，获得一级域名的IP地址
4. 本地DNS服务器向一级域名服务器发出DNS请求
5. edu一级域名服务器响应，本地DNS服务器得到类型NS和类型A的资源记录，获得权威域名服务器的IP地址
6. 本地DNS服务器向权威域名服务器发出DNS请求
7. 权威域名服务器响应，本地DNS服务器得到得到类型A的资源记录，获得对应的IP地址
8. 本地DNS将IP地址发回给主机

## 点对点文件传输(Peer-to-Peer File Distribution)

前面所讨论的协议都是基于主从式架构的，而这一节要讨论的文件传输协议则是基于点对点架构的。目前最流行的点对点文件传输协议是BitTorrent，目前有很多基于这个协议开发的点对点文件传输软件，它们之间的关系就和各种不同的浏览器之间的关系一样。

### 速度优势

我们已经知道点对点架构的可扩展性非常好，并且随着加入的主机增多，提供的服务性能反而提升，在这一点上主从式架构是相反的。我们通过一个具体的例子来量化这个问题，假设一个服务器要将一份大小为F bits的文件发送给网络中的N个主机，各个连接的速度如图所示，假设互联网足够快，且网络中只有传输这份文件的流量（即传输速度的瓶颈为接入网）：

![](https://pic4.zhimg.com/80/v2-b10ac4dc3b6169284a8f37767f2edc0b_1440w.webp)

我们来看看两种架构下，完成这个传输任务大致需要的时间：

假设主从式架构下需要的传输时间为   

- 在主从式架构下，服务器承担了所有的传输任务，也即服务器要上传NF bits数据，因此传输时间不会小于总的上传时间，   
    
- 同时每个主机都要接受F bits数据，它们的下载速度也是影响传输时间的因素之一，设  ，传输时间不会小于最慢的下载时间， 

因此我们可以得到传输时间的下确界，即  ，而通过合理安排传输我们是可以达到这个下确界的，因此认为  ，当N足够大时，传输时间就是  ，此时是N的线性函数

假设点对点架构下需要的传输时间为   

- 在点对点架构下，虽然主机也提供上传服务，但首先服务器至少需要上传一份完整的文件到网络中，因此传输时间不会小于上传一份文件的时间，   
    
- 和主从式架构时的情况一样，每个主机的下载速度也会影响传输时间， 
- 整体观察P2P网络，它的总上传能力为  ，而总共需要传输NF bits数据，因此传输时间不会小于  ， 

类似地，我们得到了传输时间的下确界，  ，通过合理安排传输我们可以达到这个下确界，认为  ，最后一项  虽然分子随着N增大而线性增大，但分母也随之增大，增长速度要低于线性增长

下面假设各个主机的上传能力相同，赋予合适的数值可以得到下面的图表：

![](https://pic4.zhimg.com/80/v2-fa4e4e4f27da171bcea2a2964d670573_1440w.webp)

可以看到主从式架构的传输时间的增长趋向于线性，而点对点架构的传输时间的增长比线性增长要慢得多，且随着N增大增长速度反而降低。

### BitTorrent

![](https://pic2.zhimg.com/80/v2-4cc7fa616fa75a5f61d1a8cbc86c79f9_1440w.webp)

BitTorrent网络由参与传输的诸多主机和一个作为追踪器(Tracker)的服务器组成，追踪器不参与文件传输，而是记录当前参与传输主机的IP信息。一个BitTorrent网络专门用于传输一个特定的文件，它被拆分成许多文件块(chunks)，它们大小相等（通常为256KB），文件块就是这个文件传输网络的基本传输单位。

当一个主机加入文件传输网络时，它会首先告知追踪器自己的存在，然后追踪器会随机选择网络中的一部分主机的IP信息制作成一张表提供给A，A会尽可能和表中所有的主机建立TCP连接（称这些主机为与A相邻的主机）。A会阶段性的向所有相邻的主机询问它们拥有的文件块，在收到所有相邻的主机拥有的文件块的列表之后，A就要请求其中它还未拥有的文件块；同时A还要上传自己已经有的文件块。

那么对A来说，就需要做出两个选择：请求哪一个文件块，以及向哪一个相邻的主机传输文件块，或者说响应哪一个相邻主机的请求。

A首先会选择未拥有的文件块中最稀有的，也就是说在A的邻居当中，拥有这个文件块的人数最少。随着时间流逝，A的邻居可能会退出网络，而新的主机又会试图和A建立连接，那么A的当务之急就是先获得最有可能之后就无法获得文件块。这一策略也有助于平衡不同文件块在整个网络当中的分布。

对于相邻主机对文件块的请求，BitTorrent提供的策略是“人人为我，我为人人”，A会给以高速率向它传输文件块的邻居更高的优先级。具体来说，每10秒钟A都会检查邻居向其传输文件的速率，然后选择其中速率最高的四个邻居并响应它们的文件块请求；同时在这四个邻居之外，A还会随机选择一个邻居响应它的文件块请求，这个随机选择可以让上传速度和下载速度相匹配的两个主机能够找到对方，提高整个网络的传输效率。并且随机选择还让新加入网络而没有存储任何文件块的主机能够获得文件块而不至于因为无法传输文件块而无法获得文件块，陷入死循环。

## 流媒体与内容分发网络

### 流媒体(Viedo Streaming)

在今天的互联网中，视频和直播占据了主导地位，它们的流量构成了互联网流量的绝大部分。这些视频是怎么被传输的？简单来说，视频是一系列图片组成的文件，频率通常为每秒24至30张照片，而照片本身又是由像素组成，每个像素都由一组数值表示其颜色。正如图片可以被压缩一样，如今的各种压缩算法可以将视频压缩至任意大小。

压缩会降低视频质量，这显然不是我们的主观愿望，这一操作的主要目的就是降低视频的码率(bit rate)。在客户端，视频是以恒定速率播放的，因此网络传输视频文件的速度必须跟上这个速度，否则会产生令人烦躁的加载，**所谓码率就是保证这个视频流畅播放的最低的文件传输速率**。通过压缩视频，我们可以使视频的码率和客户端的网络吞吐量相匹配，从而减少卡顿。正如我们在各类视频网站上都能够看到“清晰度”的选项，用户可以根据自己的网络情况选择合适的清晰度，服务器随后将本地预先压缩的文件发送给用户。

但用户可没法精确地知道自己的网速究竟适合哪个清晰度，这件事还是交给电脑来做更好，由此诞生了基于HTTP的动态自适应流(Dynamic Adptive Streaming over HTTP, DASH)为用户自动选择合适的清晰度。此时Web服务器中不同质量的视频版本对应不同的URL，并且为每个视频都维护了一张表(manifest file)，其上记录了清晰度和对应的URL. 在用户请求视频文件前，会首先向服务器请求这张表，然后再选择对应的URL，在之后的一系列请求中，客户端会不断计算当前可用带宽然后调整选择的视频版本。

### 内容分发网络(Content Distribution Network, CDN)

上面我们已经多次讨论过分布服务器的好处，而对于视频服务器也不例外。内容分发网络通过将服务器部署在世界各地来提升网络性能，每个服务器上都有一部分视频的拷贝，这样大部分用户就可以访问到距离更近、带宽更充裕、网络状况更优的服务器。

有两种部署服务器的策略，一种是将服务器部署在尽可能近（距离用户的连接数更少）的地方，也即部署到接入网网络服务提供商处，这是服务器所能部署的离用户最近的地方了，这种策略能够最大程度提升用户访问体验，但同时也需要在世界各地部署大量的服务器，建设以及维护开销十分可观；另一种是将服务器部署到距离用户相对近的地方，也即部署到互联网交换中心(IXPs)处，这个策略的好处是部署的服务器较少，开销也因此降低，但这是以相对差的用户体验作为代价的。

并不是每个服务器中都有所有视频资源的拷贝，内容分发网络的逻辑类似网页缓存，在用户向其请求文件时，若本地存储中有这个文件，则发送；若本地存储没有这个文件，则向中央服务器或其他服务器请求这个文件，然后将这个文件存储到本地，再将拷贝发送给用户。当本地存储装满时，移除被访问次数最少的视频拷贝。

和往常一样，让我们来看一个具体例子。假设一个用户要访问`http://video.netcinema.com/6Y7B23V`这个链接，而NetCinema这个公司通过第三方内容分发网络服务商KingCDN搭建了自己的视频内容分发网络：

1. 浏览器向本地DNS服务器请求这个网址对应的IP地址
2. 本地DNS服务器设法找到了NetCinema的权威域名服务器，后者发现这是个视频链接，将KingCDN中的一个主机域名`a1105.kingcdn.com`发给了本地DNS服务器
3. 本地DNS服务器访问`a1105.kingcdn.com`对应的主机，后者在NetCinema的视频内容分发网络内部选择了最适合响应这个用户的请求的服务器，然后将IP地址发给了本地DNS服务器
4. 本地DNS服务器将IP地址发给主机，主机和对应的服务器建立TCP连接，视频开始在浏览器上播放

以上步骤省略了选择服务器的策略，总的来说，大致有两种选择最优服务器的策略：

- 选择地理位置最近的服务器。这是一个相当简单的策略，服务器只需要查询请求所在IP位置（也就是本地DNS服务器的位置），然后在可用的服务器中选一个距离最近的即可。这在大多数时候都是奏效的，但是有几个问题，第一，地理距离小不代表传输经过的距离短，通信可能反而经过了更多连接；第二，有的用户可能会使用其他区域的本地DNS服务器，这时候本地DNS服务器所在的位置和用户的位置差了十万八千里；第三，这个策略无视了波动的网络状况，可能会导致选择了繁忙的服务器或带宽低的连接
- 另一种策略就是动态地测量每个本地DNS服务器的延迟和丢包率，这样内容分发服务器可以实时掌握各个区域的网络情况，并选择延迟和丢包率低的服务器提供服务。这种动态测量可以通过服务器定时向各个本地DNS服务器发送类似ping信息的数据包来实现，但很遗憾的是，许多本地DNS服务器都拒绝接受这种数据包。