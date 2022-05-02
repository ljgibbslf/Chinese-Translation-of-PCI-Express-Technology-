# 第二部分    Part Two：Transaction Layer 事务层

## Chapter 5      TLP Element //TLP 元素

### 关于上一章

上一章描述了一个 Function 通过 BARs 请求地址空间（内存地址空间或 IO 地址空间）的目的和方法，还描述了软件如何配置 Bridge 的 Base/Limit 寄存器，将源端口的 TLP 路由至正确的目的端口。我们还讨论了 PCIe 中 TLP 路由的一般概念，包括基于地址的路由、基于 ID 的路由，以及隐式路由（implicit routing）。

### 关于本章

在 PCIe 设备之间，信息是以包的形式进行传输的，包主要分为三类：TLP（Transaction Layer Packet，事务层包）、DLLP（Data Link Layer Packet，数据链路层包）和 Ordered Set（命令集，它应用于物理层）。本章将讲述 TLP 的用法、格式和不同种类的定义，并将详细讲解 TLP 的相关字段含义。关于 DLLP 的内容将会在 Chapter 9 “DLLP Element” 中进行单独讲解。

### 关于下一章

下一章将讨论流量控制协议（Flow Control Protocol）的目的和详细操作。流量控制是用来确保发送方在接收方无法接收 TLP 时，不再继续发送 TLP。流量控制避免了接收缓存的溢出，也消除了原本 PCI 工作方式中的一些低效行为，比如断开（disconnect）、重试（retry）和等待态（wait-state）。

### 5.1 基于数据包协议的介绍（Introduction to Packet-Based Protocol）

#### 5.1.1     概括（General）

不同于并行总线，PCIe 这样的串行总线不使用总线上的控制信号来表示某时刻链路上正在发生什么。相反地，PCIe 链路上的发送方发出的比特流必须要有一个预期的大小，还要有一个可供接收方辨认的格式，这样接收方才能理解比特流的内容。此外，PCIe 在传输数据包时并不使用任何直接握手机制（immediate handshake）。

除了逻辑空闲符号（Logical Idle symbol）和 Ordered Set 的物理层包外，在活跃的 PCIe 链路上传输的信息的基本组块被称为 Packet（包），包是由符号组成的。链路上交换的两类主要的数据包为高层的 TLP（Transaction Layer Packet，事务层包），和低层的用于链路维护的包称为 DLLP（Data Link Layer Packet，数据链路层包）。这些包和它们的传输流如图 5‑1 所示。物理层的 Ordered Set 也是一种包，但是它并不像 TLP 和 DLLP 一样会被封装上包起始符号和包结束符号（也就是前面章节所讲的组帧符号），并且 Ordered Set 也并没有像 TLP 和 DLLP 一样的字节条带化过程，相反地，Ordered Set 会在链路的每个通道（lane）上都复制一份，而不是像字节条带化一样把信息按字节分配到各个通道上。

![image-20220502163039798](img/5%20TLP%20%E5%85%83%E7%B4%A0/image-20220502163039798.png)

图 5‑1 TLP 和 DLLP 包

#### 5.1.2     使用基于数据包协议的动机

使用基于包的协议（Packet-Based Protocol）有三个明显的优点，特别是对于数据完整性（data integrity）来说：

##### 5.1.2.1   精心定义的数据包格式



像 PCI 这种早期的总线，它们允许总线上不确定数据量大小的传输，这使得只要传输没有结束就无法识别出数据荷载（payload）的边界。此外，任何一个设备都可以在此次传输完成前终止这个传输，这使得发送方很难去计算并传输一个覆盖了整个数据荷载的校验和或者 CRC。相反地，PCI 使用了一种简单的奇偶校验的方案，并在每个数据阶段（data phase）进行奇偶校验。（这里如果没理解请参阅 Chapter 1 内的 PCI 相关内容）

相比之下，PCIe 包具有一个已知的大小和格式。位于包的开头的部分为 Header，它用来指示这个包的类型，并包含了必需字段（required field）和可选字段（optional field）。除了地址字段以外，Header 中的其他字段长度都是固定的，地址字段可以为 32bit 也可以为 64bit。一旦一次传输开始，接收方不能暂停或者提前终止它。这种结构化的格式使得我们可以在 TLP 中加入一些信息来更有利于进行可靠的传输，例如加入组帧符号（framing symbol）、CRC、以及一个包的序列号（Sequence Number）。

##### 5.1.2.2   使用组帧符号定义包的边界

在 PCIe Gen1 和 Gen2 操作模式中使用的是 8b/10b 编码，因此在这 Gen1 和 Gen2 中每个 TLP 和 DLLP 在发送前都会使用起始符号（Start）和结束符号（End）这两种控制符号来进行组帧，这样就可以给接收方清晰地定义出包的边界。这是在 PCI 和 PCI-X 上的重大改进，在 PCI 和 PCI-X 中使用一个单独的 FRAME# 信号来指示一个事务的开始和结束，如果 FRAME# 出现了毛刺，或者其他的控制信号出现了毛刺，那么都有可能造成目标设备对总线行为的误解。一个 PCIe 接收方必须在最后的链路活动开始或结束之前，就正确完成一个完整 10bit 符号的译码，这样接收方能更容易识别不期望出现或者无法识别的符号，并且将其作为一个错误来处理。

对于 PCIe Gen3 使用的 128b/130b 编码来说，控制字符不再被使用，并且也没有组帧符号了。对于更多的关于 Gen3 编码与早期版本的差异的内容，请参阅 Chapter 12“Physical Layer-Logical（Gen3）”。

##### 5.1.2.3   使用 CRC 保障整个数据包的正确传输

在 PCI 体系结构中，会在地址阶段（address phase）和数据阶段（data phase）中使用奇偶校验边带（side-band）信号，但是在 PCIe 中则不同。PCIe 中使用带内（in-band）的 CRC 值来验证整个数据包是否进行了无错误的传输。同时 TLP 还会被发送方的数据链路层添加上一个序列号（Sequence Number），这使得当这个序列号的数据包传输出错时可以很简单的定位到它，并进行自动的重传。发送方会在自己的 Retry Buffer 内保存每个 TLP 的一个副本，直到接收方确认了这个 TLP 成功无错传输后才会将副本清除。这种 TLP 的确认机制被称为 Ack/Nak 协议（更多内容请参阅 Chapter 10 “Ack/Nak Protocol” ），它用来形成基础的链路级 TLP 错误检测和纠正机制。这种 Ack/Nak 协议提供的错误恢复机制使得我们可以在问题发生的地方或者链路上及时的解决问题，但是这也要求有一个本地的硬件解决方案来支持这种协议。

### 5.2 TLP 细节（TLP Details）

在 PCIe 中，高层次事务起源于发送方的 Device Core，终止于接收方的 Device Core。事务层会处理这些请求，其中，发送端的事务层组装 TLP，接收端的事务层解析 TLP。在这个过程中，每个设备的数据链路层和物理层也会参与包的组装。

#### 5.2.1     TLP 的组包和拆包

如图 5‑2 所示的是链路的发送端组装 TLP 和接收端拆解 TLP 的一般流程。现在让我们讲一讲从一个包的生成，到它被传送到接收端的 Device Core 的各个步骤。下面列出了 TLP 组包和拆包的关键阶段，列出的编号与图 5‑2 中的编号相对应。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image212.jpg)

图 5‑2 PCIe TLP 的组包与拆包

##### 发送方

1.    设备 A 的 Device Core 向它的 PCIe 接口发送一个请求（具体 Device Core 是如何给 PCIe 接口发送请求的，这并不是 PCIe 协议或者本书的讨论范畴）。这个请求中包括：

- 目标地址或者 ID（也就是路由信息）
- 源端信息，例如 Requester ID 和 Tag
- 事务类型/数据包类型（需要执行的命令，例如一个内存读取 MRd）
- 数据荷载大小 payload size 和数据荷载内容（如果 TLP 需要带数据）

- 流量类型（Traffic Class，用于分配数据包的优先级）

- 请求的自身属性（No Snoop 无窥探、Relaxed Ordering 宽松排序，等等）

2.    基于这个请求，事务层将会组建 TLP Header，并在其后附上数据荷载（如果有），以及如果启用并支持可选项的话也可以再附上 ECRC（End-to-End CRC）。随后 TLP 就会被放入一个虚拟通道 Buffer。这个虚拟通道会根据事务排序规则来管理 TLP 的顺序，并在向下转发 TLP 到数据链路层之前，确认接收方有足够的 Buffer 来接收一个 TLP。

3.    当 TLP 到达数据链路层，它会被分配一个序列号（Sequence Number），并基于 TLP 的内容和序列号来计算出一个 LCRC（Link CRC）来附加在原 TLP 后。然后会将经过这些处理过程之后的 TLP 保存一个副本，这个副本会保存在数据链路层的重传 Buffer（Replay Buffer，也可称为 Retry Buffer）中，这是为了应对传输出错的情况。与此同时，这个 TLP 也会被向下转发至物理层。

4.    物理层将会进行一系列的操作来准备对这个数据包进行串行传输，包括字节条带化（Byte Striping）、扰码（Scrambling）、编码（Encoding）以及并串转换（Serializing）。对于 Gen1 和 Gen2 的设备，当进行 8b/10b 编码时，会将 STP 和 END 这两个控制字符分别加在 TLP 的首端和尾端。最后，这个数据包通过链路进行传输。在 Gen3 操作模式中，STP 令牌（STP token）会被添加在 TLP 的首端，但是并不会在尾端加上 END，而是在 STP 令牌中包含 TLP 大小的信息来判断 TLP 的尾部位置。

 

##### 接收方

5.    在接收方（本例中是 Device B），为了发送包所做的一切准备现在都必须撤销。物理层将对比特流进行串并转换（deserialize）、对串并转换后的符号进行解码，然后再进行字节反条带化（un-stripes）。控制字符将被移除，因为它们仅在物理层有意义，然后这个数据包就会被向上转发至数据链路层。

6.    数据链路层将会计算 CRC（具体一点是 LCRC）并与 TLP 中的 CRC 进行比较。如果 CRC 比较结果相同，那么就再检查序列号（Sequence Number）。如果都没有出现错误，那就把 CRC 和序列号都从 TLP 剥除，并随后将 TLP 向上转发给接收方事务层，与此同时要通过返回给发送方一个 Ack DLLP 来通知发送方这个 TLP 被成功接收。相反地，如果前面的过程中检查出了错误，那么就要返回给发送方一个 Nak DLLP，这样发送方将会使用它的重传 Buffer 来对 TLP 进行重传。

7.    在事务层，TLP 被进行解码，并将 TLP 内的信息传递给 Device Core 来进行相应的操作。如果当前接收设备就是数据包的最终目的地，那么它可以检查 ECRC 错误，并在发现任何 ECRC 错误时报告给 Device Core。

#### 5.2.2     TLP 结构

一个事务层包 TLP 中每个字段域的基本用法在表 5‑1 中进行了定义。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image214.jpg)

表 5‑1 TLP Header 的 Type 字段定义了事务不同种类

下面对表 5‑1 中的内容进行复述。

- Header

- 协议层次：事务层

- 该组件用法：大小为 3 或 4DW（12 或 16Bytes）。Header 的格式会随类型而变化，但是 Header 也定义了一些参数，包括：

	- 事务类型（Transaction Type）

	- 目标地址、ID 等

	- 传输数据量大小（如果有数据）、字节使能（Byte Enable）

	- 属性（Attribute）

	- 流量类型（Traffic Class）

- Data

- 协议层次：事务层

- 该组件用法：可选的 1-1024DW 大小的数据荷载，具体的大小由字节使能或者字节对齐的开始和结束地址来进行描述。需要注意指定的长度不能为 0，但是一个 0 长度的读取可以通过指定长度为 1DW 然后将字节使能全部置为 0 来进行近似处理（在某些情况下会使用）。来自 Completer 的结果数据虽然是未定义的种类，但是 Requester 并不使用它，因此就和指定 0 长度的目的等效了。

- Digest/ECRC

- 协议层次：事务层

- 该组件用法：可选的功能。当需要使用时，ECRC 的大小永远为 1DW。

#### 5.2.3     通用 TLP Header 格式（Generic TLP Header Format）

##### 5.2.3.1   概括（General）

如图 5‑3 中，展示了一个 4DW 的通用 TLP Header 的格式和内容。在本节内，会对几乎所有事务的 TLP Header 中的公共字段进行总结，并会在稍后讨论与特定事务类型相关 Header 格式差异。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image216.jpg)

图 5‑3 通用 TLP Header 的各字段域

##### 5.2.3.2   通用 Header 各字段摘要

表 5‑2 中对通用 TLP Header 中的每个字段的大小和用途进行了总结。需要注意的是，在图 5‑3 中被标注为“R”的字段是保留字段（reserve），应该被置为 0。



| Header 中的字段名                                             | Header 中的位置                          | 字段作用                                                     |
| ------------------------------------------------------------ | --------------------------------------- | ------------------------------------------------------------ |
| Fmt[2:0]  格式  (Format)                                     | Byte  0  Bit  7:5                       | 这些 bit 的编码信息是关于 Header 的大小，以及这个 TLP 中是否存在数据荷载部分： <br> 000b：3DW Header，无数据荷载 <br> 001b：4DW Header，无数据荷载 <br> 010b：3DW Header，有数据荷载 <br> 011b：4DW Header，有数据荷载 <br> 100b：1DW，属于 Prefix TLP  不难发现，除了 Prefix TLP 外，只要 Fmt[0]=0 那么就是 3DW Header，反之则为 4DW Header。若 Fmt[1]=0 那么就无数据荷载，反之则有数据荷载。  对于低于 4GB 的地址，必须使用 3DW Header。协议规定，如果使用 4DW Header 但是地址小于 4GB，也就是说将 64bit 地址的高 32bit 置为 0，那么这种情况下接收方的行为是未进行定义的（undefined）。 |
| Type[4:0]  类型                                              | Byte  0  Bit  4:0                       | 这些 bit 编码的信息是 TLP 的不同事务类型。Type 字段用来跟 Fmt[1:0]字段一起指定了事务类型、Header 大小、以及是否存在数据荷载。更多详细信息请参阅“Generic Header  Field Details”一节。 |
| TC[2:0]  流量类型  (Traffic  Class)                          | Byte  1  Bit  6:4                       | 这些 bit 表示将应用于这个 TLP 和与之完成相关（如果需要完成包）的流量类型： <br> 000b：Traffic Class  0（默认） <br> 001b：Traffic Class  1  …… <br> 111b：Traffic Class  7  TC 0 是默认类型，而 TC 1-7 是用来提供差异化的服务。更多信息请参阅“Traffic Class”一节。 |
| Attr[2]  属性  (Attribute)                                   | Byte  1  Bit  2                         | 这个第三位的 Attribute 位（它共有 3 位）用于表示这个 TLP 是否使用基于 ID 的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering”一节。 |
| TH  TLP 处理提示  (TLP  Processing Hints)                     | Byte  1  Bit  0                         | 它用于表示何时 TLP 中会包含 TLP 提示（TLP Hints），以便让系统了解如何更好的处理这个 TLP。更多内容请参阅“TPH（TLP Processing Hints）”。 |
| TD  TLP 摘要  (TLP  Digest)                                   | Byte  2  Bit  7                         | 如果 TD=1，那么这个 TLP 中将包括可选的 4Byte TLP Digest 字段，也就是 ECRC 值。  它有一些规则： <br> 所有的接收者都必须要通过这个 bit 来检查是否存在 Digest 字段。 <br> 如果一个 TLP 的 TD=1，但是它又没有 Digest，那么它将被当做畸形 TLP（Malformed TLP）处理。 <br> 如果接收设备支持 ECRC 校验，且此 TLP 内 TD=1，那么这个接收设备必须进行校验。 <br> 如果一个作为 TLP 最终目的地的设备并不支持 ECRC 校验（因为这是可选功能），那么它必须要忽略 Digest 字段。  更多内容请参阅“CRC”和“ECRC Generating and Checking”这两节。 |
| EP  受污染的数据  (Poisoned Data)                            | Byte 2  Bit 6                           | 如果 EP=1，那么就认为所有伴随此数据的数据都是无效的，尽管相关事务依然允许正常的完成。关于 Poisoned 数据包的更多内容，请参阅“Data Poisoning”一节。 |
| Attr[1:0]  属性  (Attributes)                                | Byte  2  Bit  5:4                       | Bit 5 = Relax Ordering（宽松排序）：当它被置为 1 时，这个 TLP 会启用 PCI-X 的宽松排序。如果它为 0，则是用 PCI 的严格的 PCI 排序（strict PCI ordering）。  Bit 4 = No Snoop（无窥探）：当置为 1 时，Requester 的意思是这个 TLP 不会存在 host cache 一致性的问题（host cache coherency issues），因此系统硬件可以通过跳过普通处理器对这个请求的 cache 窥探，以此来节省时间。而当这一位被置为 0 时，需要进行 PCI-type 的 cache 窥探保护。 |
| AT[1:0]  地址类型  (Address  Type)                           | Byte  2  Bit  3:2                       | 对于 Memory 和 Atomic 请求来说，这个字段用于支持虚拟化系统（virtualized system）的地址转换。这个地址转换协议由一个单独的规范进行描述，被称为 Address  Translation Services，可以看到该字段的编码为： <br> 00 = 默认/未转换（Default/Untranslated） <br> 01 = 转换请求（Translation Request） <br> 10 = 已转换（Translated） <br> 11 = 保留 reserve |
| Length[9:0]  长度                                            | Byte  2  Bit  1:0     Byte  3  Bit  7:0 | TLP 数据荷载传输量的大小，单位为 DW，编码方式为： <br> 00 0000 0001b = 1DW <br> 00 0000 0010b = 2DW  …… <br> 11 1111 1111b = 1023DW <br> 00 0000 0000 = 1024DW |
| Last  DW BE[3:0]  末尾 DW 的字节使能  (Last  DW Byte Enable)   | Byte  7  Bit  7:4                       | 这个字段中的 4 个高有效的 bit，与数据荷载中最后一个 DW 中的 4 个 Byte 一一对应。 <br> Bit 7 = 1：末尾 DW 的 Byte 3 是有效的；否则为无效的。 <br> Bit 6 = 1：末尾 DW 的 Byte 2 是有效的；否则为无效的。 <br> Bit 5 = 1：末尾 DW 的 Byte 1 是有效的；否则为无效的。 <br> Bit 4 = 1：末尾 DW 的 Byte 0 是有效的；否则为无效的。 |
| 1st  DW BE[3:0]  第一个 DW 的字节使能  (First  DW Byte Enable) | Byte  7  Bit  3:0                       | 这个字段中的 4 个高有效的 bit，与数据荷载中第一个 DW 中的 4 个 Byte 一一对应。 <br> Bit 3 = 1：末尾 DW 的 Byte 3 是有效的；否则为无效的。 <br> Bit 2 = 1：末尾 DW 的 Byte 2 是有效的；否则为无效的。 <br> Bit 1 = 1：末尾 DW 的 Byte 1 是有效的；否则为无效的。 <br> Bit 0 = 1：末尾 DW 的 Byte 0 是有效的；否则为无效的。 |

表 5‑2 通用 TLP Header 的各字段摘要

#### 5.2.4     通用 TLP Header 详细说明（Generic TLP Header Details）

在接下来的几个小节中，我们将对图 5‑3 中展示的 TLP Header 的每一个字段都进行细节描述。

##### 5.2.4.1   Header 的 Type/Format 字段编码含义

表 5‑3 中总结了 TLP Header 中 Type 字段和 Fmt 字段（Format）的编码含义。

| TLP 事务种类                                                  | Fmt[2:0]                                | Type[4:0]                |
| ------------------------------------------------------------ | --------------------------------------- | ------------------------ |
| Memory  Read 请求  （MRd）                                    | 000=3DW，无数据  001=4DW，无数据        | 0 0000                   |
| Memory  Read Locked 请求  （MRdLk）                           | 000=3DW，无数据  001=4DW，无数据        | 0  0001                  |
| Memory  Write 请求  （MWr）                                   | 010=3DW，有数据  011=4DW，有数据        | 0  0000                  |
| IO  Read 请求  （IORd）                                       | 000=3DW，无数据  （IO 请求为 3DW Header） | 0  0010                  |
| IO  Write 请求  （IOWr）                                      | 010=3DW，有数据  （IO 请求为 3DW Header） | 0  0010                  |
| 配置 Type 0 Read 请求  （CfgRd0）                              | 000=3DW，无数据                         | 0  0100                  |
| 配置 Type 0 Write 请求  （CfgWr0）                             | 010=3DW，有数据                         | 0  0100                  |
| 配置 Type 1 Read 请求  （CfgRd1）                              | 000=3DW，无数据                         | 0  0101                  |
| 配置 Type 1 Write 请求  （CfgWr1）                             | 010=3DW，有数据                         | 0  0101                  |
| Message 请求  （Msg）                                         | 001=4DW，无数据                         | 1  0 rrr*  （见表 4‑10） |
| Message 带数据的请求  （MsgD）                                | 011=4DW，有数据                         | 1  0 rrr*  （见表 4‑10） |
| 完成包 Completion  （Cpl）                                    | 000=3DW，无数据                         | 0  1010                  |
| 完成包带数据  （CplD）                                       | 010=3DW，有数据                         | 0  1010                  |
| 锁定的完成包  （CplLk）                                      | 000=3DW，无数据                         | 0  1011                  |
| 锁定的完成包带数据  （CplDLk）                               | 010=3DW，有数据                         | 0  1011                  |
| 获取和增加原子操作请求  （Fetch and Add AtomicOP Request）   | 010=3DW，有数据  011=4DW，有数据        | 0  1100                  |
| 无条件的交换原子操作请求  （Unconditional Swap AtomicOP Request） | 010=3DW，有数据  011=4DW，有数据        | 0  1101                  |
| 对比和交换原子操作请求  （Compare and Swap AtomicOP Request） | 010=3DW，有数据  011=4DW，有数据        | 0  1110                  |
| 本地 TLP 前缀  （Local TLP Prefix）                            | 100=TLP  Prefix                         | 0L3L2L1L0                |
| 端到端 TLP 前缀  （End-to-End TLP Prefix）                     | 100=TLP  Prefix                         | 1E3E2E1E0                |

表 5‑3 TLP Header 的 Type 字段和 Fmt 字段编码含义

##### 5.2.4.2   Digest/ECRC 字段

TLP 的 Digest 位表示是否存在 ECRC（End-to-End CRC）。如果当前支持这个可选的特性，并且软件也启用了它，那么设备将会给自己产生的所有 TLP 都计算并且添加上一个 ECRC。需要注意，使用 ECRC 要求设备含有可选的 Advanced Error Reporting 寄存器，这是因为它的能力（Capability）和控制寄存器就位于 Advanced Error Reporting 寄存器中。

###### l ECRC 的生成和检查

ECRC 覆盖了所有在跨 Fabric 转发时不需要更改的字段。然而，当一个数据包在拓扑结构中移动时，有两个 bit 是可以合法的进行改变的：

- Type 字段的 Bit 0：在一个配置事务通过一个 Bridge 时，这个配置事务有可能从 Type 1 变成 Type 0，因为这个 Bridge 的次级总线有可能就是配置事务的目标总线。在这个情况下，Type 地段的 bit 0 的值就会从 1 变成 0。
	
- EP（Error/Poisoned）bit：当一个 TLP 在网络结构中移动时，如果与这个数据包相关的数据被认为是损坏的，那么就有可能引发这个 bit 值的改变。这是一种可选的特性，称为错误转发（error forwarding）。

###### l 谁来检查 ECRC

ECRC 的预定目标是 TLP 的最终接收者。对 LCRC（Link CRC）的校验是用来验证在当前给定链路上的传输没有出错，但是会在 Switch/Bridge 的出口端口（egress port）重新计算一个 LCRC，然后再转发给下一级链路，也就是说同一个 LCRC 的作用范围不会跨 Switch/Bridge，那么这将会使得 Switch/Bridge 这些路由元件内部发生的错误被掩盖。为了解决这个问题，就在 TLP 在 Requester 到 Completer 的整个传输过程中都加上一个不会改变的 ECRC（也就是这个过程中 ECRC 都不会被其他设备重新计算和替换）。当目标设备检查 ECRC 时，任何在整个传输过程中发生的错误都会极容易被发现。

关于 ECRC 校验中 Switch 的作用，PCIe 协议做了两个声明：

- 如果一个 Switch 支持 ECRC 校验，那么它需要对 TLP 目的地为 Switch 自身时，对这个 TLP 中的 ECRC 进行校验。如果 TLP 的目的地并不是 Switch 自身，那么 Switch 必须在保持 ECRC 不变的前提下对其进行转发，把 ECRC 作为 TLP 不可分割的一部分。“On all other TLPs a Switch must preserve the ECRC (forward it untouched) as an integral part of the TLP.”
	
- 注意，Switch 也可以对通过它的 TLP 进行 ECRC 检查。由 Switch 发现的 ECRC 错误的报告方法和其他设备一样，但是这不会改变 TLP 通过 Switch 的继续传输。“Note that a Switch may perform ECRC checking on TLPs passing through the Switch. ECRC Errors detected by the Switch are reported in the same way any other device would report them, but do not alter the TLPs passage through the Switch.”

##### 5.2.4.3   使用字节使能（Using Byte Enables）

###### l 整体说明

类似 PCI 一样，对于有时传输大小或者起始/结束地址不是 DW 对齐时，PCIe 也需要一种机制来根据需要协调其 DW 对齐地址。为了达到这个目的，PCIe 利用了 2 个字节使能（Byte Enable）字段，如图 5‑3 中和表 5‑2 中所介绍，分别是首 DW 字节使能（First DW Byte Enable）和尾 DW 字节使能（Last DW Byte Enable），它们使得 Requester 可以对第一个 DW 和最后一个 DW 真正有效传输的字节进行限定。

###### l 字节使能规则

1.    字节使能地各个 bit 是高有效的。如果某个字节使能 bit 值为 0，那么就表示数据荷载中相应的字节不应该被 Completer 使用，因为这个字节是无效的。若某个字节使能 bit 值为 1，那么就说明相应的字节应该被 Completer 使用。

2.    如果有效的数据全都在一个单独的 DW 中，那么尾 DW 字节使能（Last DW Byte Enable）必须为 0000b。

3.    如果 Header 中的 Length 字段表示了这次传输数据量大于 1DW，那么首 DW 字节使能（First DW Byte Enable）至少要有 1 个 bit 是有效的。

4.    如果 Header 中的 Length 字段表示了这次传输数据量大于等于 3DW，那么首 DW 字节使能和尾 DW 字节使能都必须是相连的 bit 为 1。在这种情况中，字节使能仅能用来提供有效起始地址和结束地址与 DW 对齐地址的字节偏移量。

5.    如果传输数据量大小为 1DW，那么才能允许只有首 DW 字节使能内是不连续为 1 的情况。

6.    如果传输数据量大小为 1-2DW，那么才能允许首 DW 字节使能和第二 DW 字节使能内都是不连续为 1 的情况。

7.    如果一个写请求，其 Length 字段表示其数据传输长度为 1DW，但是它的字节使能均为无效，这种情况对 Completer 不会有影响。

8.    如果一个读请求，其 Length 字段表示其需要的数据传输长度为 1DW，但是没有字节使能有效，那么 Completer 会返回一个 1DW 数据荷载，这种数据荷载是未定义数据（undefined data）。这种情况可能被用作一种刷新机制（Flush mechanism），它利用事务排序规则，在这个 undefine data 的读请求完成包返回之前，强制之前发出的所有 posted write 输出到内存中。

###### l 字节使能示例

如图 5‑4 展示了字节使能字段是如何使用的。注意，数据传输长度必须从第一个 DW 中的任何有效字节延伸至最后一个 DW 中的任何有效字节。因为数据传输量大于 2DW，字节使能就只能用来标识这次传输中起始地址的位置（2d）和结束地址的位置（34d）。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image218.jpg)

图 5‑4 首 DW 字节使能和尾 DW 字节使能字段的使用

##### 5.2.4.4   事务描述符字段（Transaction Descriptor Fields）

当事务在 Requester 和 Completer 之间移动时，必须要能够对各个事务进行唯一的标识，因为在任何时刻的 Requester 中都有可能有许多的拆分事务在排队。为了更好的进行标识，PCIe 协议在 TLP Header 中定义了多个重要字段，来组成一个唯一的事务描述符（Transaction Descriptor），如图 5‑5 所示。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image220.jpg)

图 5‑5 事务描述符字段

虽然事务描述符在 TLP Header 中并不是位置全连续的字段，但是这些不连续字段合起来可以一起描述事务的关键属性，包括：

###### l 事务 ID（Transaction ID）

事务 ID 由 Requester ID（Requester 的 BDF）和 TLP 的 Tag 字段共同组成。

###### l 流量类型（Traffic Class）

流量类型（Traffic Class，TC）是由 Requester 的 Device Core 来添加进 TLP 的，并且在拓扑结构中从 Requester 传输到 Completer 的过程中 TC 都不发生改变。在拓扑结构的每个链路中，TC 都映射到其中一个虚拟通道（Virtual Channel）。

###### l 事务属性（Transaction Attributes）

用于表示基于 ID 的排序（ID-based Ordering）、宽松排序（Relaxed Ordering）以及无窥探（No Snoop）的 bit 也存在于请求包中，被发送给 Completer。

##### 5.2.4.5   带有数据荷载的 TLP 的一些额外规则

当一个 TLP 带有数据荷载时，就要遵从如下规则：

1.    Length 字段仅表示数据荷载的长度。

2.    数据荷载的首字节（紧挨着 Header 的字节）的地址永远是最低位的。

3.    Length 字段永远表示的是整数 DW 的传输长度。对于不满整数 DW 的情况是使用首 DW 字节使能和尾 DW 字节使能来进行表示的。

4.    在协议中有声明，当一个 Completer 在响应一个单独的 Memory 请求时如果返回了多个 TLP，那么中间的（也就是非首尾的）TLP 的数据荷载的地址必须结束于 RC 的 64byte 或 128byte 自然对齐的边界上。具体是那种边界是由一个可配置位称为 RCB（读完成边界，Read Completion Boundary）来控制的。所有的其他设备遵循 PCI-X 协议，并会将这种情况的 TLP 截断在 128byte 自然对齐边界上。

5.    当发送 Message 请求时，Length 字段是保留字段，除非 Message 是带有数据荷载的，也就是 MsgD。

6.    TLP 的数据荷载一定不能大于设备控制寄存器（Device Control Register）中的最大数据荷载字段（Max_Payload_Size）。因为只有写事务会带有数据荷载，因此这种限制并不应用于读请求。接收方需要在被写入时检查是否存在 Max_Payload_Size 的违例，如果存在则认为这是一个畸形 TLP（Malformed TLP）。

7.    接收方还需要检查 Length 字段和 TLP 实际传输的数据荷载长度。如果二者不相同则认为这是一个畸形 TLP（Malformed TLP）。

8.    请求包中的起始地址和传输长度的组合信息，一定不能造成跨 4KB 边界的 Memory 访问。虽然这种检查是可选的，但是一旦发现了跨 4KB 边界访问的情况就认为这是一个畸形 TLP（Malformed TLP）。

#### 5.2.5     具体的 TLP 格式：请求 TLP 和完成 TLP

在本节中，将会描述用来构成具体的一些事物类型的 TLP 3DW Header 和 4DW Header。许多通用的字段就如前文所述，因此我们把重点放在那些需要根据具体事务类型进行具体处理的字段上。接下来的几个小节是关于 TLP Header 的详细描述，相关的 TLP 类型为：1）IO Request，2）Memory Requests，3）Configuration Requests，4）Completions，以及 5）Message Request。

##### 5.2.5.1   IO 请求（IO Request）

虽然协议中不鼓励使用 IO 事务，但是对于传统遗留设备还是允许的，并且对于软件来说需要通过 IO 事务来兼容那些存在系统 IO 映射而不是内存映射的设备。虽然 IO 事务从技术上讲是可以访问 32bit IO 范围的，但是实际上许多系统（和 CPU）都会将 IO 访问限制在低 16bit（64KB）范围内。图 5‑6 展示了系统 IO 映射以及 16bit 和 32bit 地址边界。自身不是传统遗留设备的设备是不允许访问 BAR 中的 IO 地址空间的。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image222.jpg)

图 5‑6 系统 IO 映射

###### l IO Request Header 的格式

如图 5‑7 所示是一个 3DW IO Request Header，其中的每个字段都会在接下来的内容中进行描述。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image224.jpg)

图 5‑7 3DW IO Request Header 格式

 

###### l IO Request Header 各字段

一个 IO Request Header 中的每个字段的位置和作用都在表 5‑4 中进行了描述。

| 字段名称                                  | 位于 Header 中的 Byte/Bit                                       | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                | Byte  0 Bit 7:5                                              | IO 请求的包格式字段：  000b=IO  Read（3DW，无数据）  010b=IO  Write（3DW，有数据） |
| Type[4:0]  类型                           | Byte  0 Bit 4:0                                              | 0 0010b=IO 请求                                               |
| TC[2:0]  流量类型  （Traffic Class）      | Byte  1 Bit 6:4                                              | IO 请求的 TC 永远为 0，确保这些包永远不会干扰高优先级的包。      |
| Attr[2]  属性  （Attribute）              | Byte  1 Bit 2                                                | 由于 IO 请求不使用基于 ID 的排序（ID-based Ordering），因此这个字段为保留字段。 |
| TH  TLP 处理提示  （TLP Processing Hints） | Byte  1 Bit 0                                                | 由于 IO 请求不使用 TLP 处理提示（TLP Processing Hints），因此这个字段为保留字段。 |
| TD  TLP 摘要  （TLP Digest）               | Byte  2 Bit 7                                                | 表示 TLP 的末尾是否存在 Digest 字段，也就是 ECRC。                |
| EP  受污染的数据  （Poisoned Data）       | Byte  2 Bit 6                                                | 表示 TLP 的数据荷载（如果有）是否被污染（poisoned）。          |
| Attr[1:0]  属性  （Attribute）            | Byte  2 Bit 5:4                                              | 由于 IO 请求不使用宽松排序（Relaxed  Ordering）和无窥探（No Snoop），因此这个字段为 0。 |
| AT[1:0]  地址类型  （Address Type）       | Byte  2 Bit 3:2                                              | 由于 IO 请求不使用地址类型字段，因此这个字段必须为 0。          |
| Length[9:0]  长度                         | Byte  2 Bit 1:0  Byte  3 Bit 7:0                             | 该字段用来表示以 DW 为单位的数据荷载大小。对于 IO 请求来说，这个字段为 1，因为 IO 请求不能传输超过 4Byte。由首 DW 字节使能来限定使用哪个 Byte。 |
| Requester  ID[15:0]  Requester  ID        | Byte  4 Bit 7:0  Byte  5 Bit 7:0                             | 用于表示相应完成包的“返回地址”。  Byte 4,7:0=Bus  Number  Byte 5,7:3=Device  Number  Byte 5,2:0=Function  Number |
| Tag[7:0]  Tag                             | Byte  6 Bit 7:0                                              | 这个字段用来标识来自 Requester 的具体每个请求。每个被发出的请求包都会被分配一个独一无二的 Tag 值。默认情况下，只使用 Tag[4:0]，但是扩展 Tag（Extended Tag）和 Phantom Function 可选项可以将 Tag 的使用扩展为 Tag[10:0]，这样就允许多达 2048 个未完成的请求同时进行。 |
| Last  DW BE[3:0]  尾 DW 字节使能            | Byte  7 Bit 7:4                                              | 因为 IO 请求数据大小只能是 1DW 以内，所以这个字段必须为 0000b。   |
| 1st  DW BE[3:0]  首 DW 字节使能             | Byte  7 Bit 3:0                                              | 这个字段用于限定第一个 DW 数据内的有效字节。对于 IO 请求来说，所有的 bit 组合都是允许的（包括 0000b）。 |
| Address[31:2]  地址                       | Byte  8 Bit 7:0  Byte  9 Bit 7:0  Byte  10 Bit 7:0  Byte  11 Bit 7:2 | 该字段是 32bit 起始地址的高 30bit。而最低 2bit 是保留位（00b），这使得 32bit 起始地址一定是 DW 对齐的。 |

表 5‑4 IO Request Header 各字段

##### 5.2.5.2   Memory 请求（Memory Request）

PCIe 的 Memory 事务包含两种类型：第一种是读请求及其相对应的完成包，第二种是写请求。系统内存映射如所示，展示了 3DW 与 4DW 的 Memory 请求包。需要记住协议规范中多次重申的一点，Memory 传输绝对不允许跨 4KB 地址边界。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image226.jpg)

图 5‑8 3DW 和 4DW 的 Memory Request Header 格式

###### l Memory Request Header 各字段

一个 4DW 的 Memory Request Header 中的每个字段的位置和作用都在表 5‑5 中进行了描述。注意，3DW 和 4DW Header 的区别仅仅是起始地址字段的位置和字段大小不同。

| 字段名称                                  | 位于 Header 中的 Byte/Bit                                       | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                | Byte  0 Bit 7:5                                              | IO 请求的包格式字段：  000b=Memory  Read（3DW 无数据）  010b=Memory  Write（3DW 有数据）  001b=Memory  Read（4DW 无数据）  011b=Memory  Write（4DW 有数据）  1xxb=TLP 前缀已经被添加到包的开头。更多内容请参阅“TPH（TLP Processing Hints）”一节 |
| Type[4:0]  类型                           | Byte  0 Bit 4:0                                              | 0 0000b=  Memory Read 或 Write  0 0001b=  Memory Read Locked  Type 字段与 Fmt[1:0]共同指定事务类型、Header 大小以及是否有数据荷载。 |
| TC[2:0]  流量类型  （Traffic Class）      | Byte  1 Bit 6:4                                              | 这个字段的编码信息是请求包及其完成包的流量类型。  000b=TC  0（默认）  ……  111b=TC  7  更多内容请参阅“Traffic Class（TC）”一节。 |
| Attr[2]  属性  （Attribute）              | Byte  1 Bit 2                                                | 表示这个 TLP 是否使用基于 ID 的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering（IDO）”一节。 |
| TH  TLP 处理提示  （TLP Processing Hints） | Byte  1 Bit 0                                                | 指示是否包含 TLP 提示信息。更多内容请参阅“TPH（TLP Processing  Hints）”一节。 |
| TD  TLP 摘要  （TLP Digest）               | Byte  2 Bit 7                                                | 如果为 1，则 TLP 中存在 TLP Digest 字段，即存在 ECRC。  一些规则：  所有的接收者都要使用这个 TD bit 检查是否存在 Digest 字段。  l TD=1 但是不含有 Digest 字段的 TLP 被当做畸形（Malformed）  l 如果 TD=1，且接收者也启用了 ECRC 检查，那么就必须对 ECRC 进行检查。  l 如果接收者并不支持可选的 ECRC 校验，那么它必须要忽略 TLP 中的 Digest 字段。 |
| EP  受污染的数据  （Poisoned Data）       | Byte  2 Bit 6                                                | 如果为 1，那么伴随这个 TLP 的数据荷载就都被认为是有错误的，尽管这个事务还是可以照常完成。 |
| Attr[1:0]  属性  （Attribute）            | Byte  2 Bit 5:4                                              | l Bit 5 =宽松排序（Relaxed Ordering）  当 Bit 5=1，这个 TLP 就使用 PCI-X 的宽松排序。否则，使用严格的 PCI 排序。  l Bit 6=无窥探（No Snoop）  当 Bit 6=1，系统软件就不需要进行关于这个 TLP 的处理器 Cache 一致性窥探。 |
| AT[1:0]  地址类型  （Address Type）       | Byte  2 Bit 3:2                                              | 这个字段支持虚拟系统（virtualized systems）的地址翻译。地址翻译协议是由一个单独的协议规范来描述的，称为地址翻译服务（Address Translation Services），AT 中的编码信息为：  00=默认/未翻译  01=翻译请求  10=已翻译  11=保留位 |
| Length[9:0]  长度                         | Byte  2 Bit 1:0  Byte  3 Bit 7:0                             | TLP 数据荷载的长度，单位为 DW。最大为 1024DW（4KB），编码如下：  00  0000 0001b=1DW  00  0000 0010b=2DW  ……  11  1111 1111b=1023DW  00  0000 0000b=1024DW |
| Requester  ID[15:0]  Requester  ID        | Byte  4 Bit 7:0  Byte  5 Bit 7:0                             | 用于表示这个请求的相应完成包的“返回地址”。  Byte 4,7:0=Bus  Number  Byte 5,7:3=Device  Number  Byte 5,2:0=Function  Number |
| Tag[7:0]  Tag                             | Byte  6 Bit 7:0                                              | 这个字段用来标识来自 Requester 的具体每个请求。每个被发出的请求包都会被分配一个独一无二的 Tag 值。默认情况下，只使用 Tag[4:0]，如果 Control Register 中的 Extended Tag 位被置为 1，那么可以将 Tag 的使用扩展为 Tag[7:0]，这样就允许最多使用 256 个 Tag。 |
| Last  DW BE[3:0]  尾 DW 字节使能            | Byte  7 Bit 7:4                                              | 这个字段用于限定最后一个 DW 数据内的有效字节                   |
| 1st  DW BE[3:0]  首 DW 字节使能             | Byte  7 Bit 3:0                                              | 这个字段用于限定第一个 DW 数据内的有效字节。                   |
| Address[63:32]  高 32bit 地址               | Byte  8 Bit 7:0  Byte  9 Bit 7:0  Byte  10 Bit 7:0  Byte  11 Bit 7:2 | 该字段是 Memory 传输的 64bit 起始地址的高 32bit。                 |
| Address[31:2]  低 32bit 地址                | Byte  12 Bit 7:0  Byte  13 Bit 7:0  Byte  14 Bit 7:0  Byte  15 Bit 7:2 | 该字段是 Memory 传输的 64bit 起始地址的低 32bit。而最低 2bit 是保留位始终为 0，这样就强制起始地址是 DW 对齐的。 |

表 5‑5 4DW Memory Request Header 各字段

###### l Memory Request 注意事项

Memory 请求的特性包括：

1.    Memory 数据传输不允许跨 4KB 边界。

2.    所有的内存映射写（Memory-Mapped Write）都是 Posted 的，以此来提升性能。

3.    使用 32bit 或者 64bit 地址方式。

4.    数据荷载大小在 0-1024DW 之间（0-4KB）。

5.    QoS（Quality of Service）特性可以被使用，最多可以有 8 个流量类型。

6.    No Snoop 属性可以在事务访问主存时，用来减轻系统对窥探处理器 Cache 的需求。

7.    Relaxed Ordering 属性可以使得数据包传输路径上的设备对这个 TLP 使用宽松排序规则，以此来提升性能。

##### 5.2.5.3   Configuration 请求（Configuration Request）

PCIe 像 PCI 一样使用 Type 0 和 Type 1 配置请求，以此来保持向后兼容性。一个 Type 1 配置请求会向下行传播，直到一个 Bridge 的次级总线（Secondary Bus）正好是这个 Type 1 配置请求的目标总线，在这时这个 Bridge 就会把这个配置事务从 Type 1 转换为 Type 0。Bridge 是通过此前编程写入的总线号寄存器来得知何时应该对配置事务进行 Type 转换以及继续转发，其中总线号寄存器包括主总线号（Primary Bus Number）、次级总线号（Secondary Bus Number）以及从属总线号（Subordinate Bus Number）。关于这里的更多内容，请参阅“传统 PCI 机制（Legacy PCI Mechanism）”一节。

如图 5‑9 中，一个 Type 1 配置请求正在按照规则向下行移动，它在 Switch 左侧的下行端口的 Bridge 被转换成 Type 0，这种转换是通过改变 TLP Header 中的 Type 字段 Bit 0 来完成的（Type 从 0 0101b 变为 0 0100b）。注意，不同于 PCI，PCIe 中下行的一条链路上只能由一个设备，因此链路上不需要有 IDSEL 或者其他指示信号来告诉设备需要声明自己占有 Type 0 配置请求，也就是说设备只要从上行收到了 Type 0 配置请求，就知道这个配置请求的目标设备就必然是自己。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image228.jpg)

图 5‑9 3DW 配置请求以及 Header 格式

###### l Configuration Request Header 各字段的定义

图 5‑9 中展示的 Configuration Request Header 中的每个字段的位置和作用都在表 5‑6 中进行了描述。

| 字段名称                                                     | 位于 Header 中的 Byte/Bit           | 功能                                                         |
| ------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                                   | Byte  0 Bit 7:5                  | 配置请求始终为 3DW Header：  000b=Configuration  Read（无数据）  010b=  Configuration Write（有数据） |
| Type[4:0]  类型                                              | Byte  0 Bit 4:0                  | 0 0100b=  Type 0 配置请求  0 0101b=  Type 1 配置请求           |
| TC[2:0]  流量类型  （Traffic Class）                         | Byte  1 Bit 6:4                  | 配置请求的 TC 必须为 0，以确保这些配置包永远不会影响到高优先级的包。 |
| Attr[2]  属性  （Attribute）                                 | Byte  1 Bit 2                    | 配置请求的这个字段为保留字段，必须为 0。                      |
| TH  TLP 处理提示  （TLP Processing Hints）                    | Byte  1 Bit 0                    | 配置请求的这个字段为保留字段，必须为 0。                      |
| TD  TLP 摘要  （TLP Digest）                                  | Byte  2 Bit 7                    | 用于指示 TLP 的末尾是否有 1DW 的 Digest 字段。                     |
| EP  受污染的数据  （Poisoned Data）                          | Byte  2 Bit 6                    | 用于指示数据荷载是被污染的。                                 |
| Attr[1:0]  属性  （Attribute）                               | Byte  2 Bit 5:4                  | 配置请求的 Relaxed Ordering 和 No Snoop 字段都为 0。              |
| AT[1:0]  地址类型  （Address Type）                          | Byte  2 Bit 3:2                  | 配置请求的 Address Type 字段为保留字段，必须为 0。              |
| Length[9:0]  长度                                            | Byte  2 Bit 1:0  Byte  3 Bit 7:0 | 配置请求的数据荷载大小字段永远为 1，4bit 字节使能的所有组合都是允许的。 |
| Requester  ID[15:0]  Requester  ID                           | Byte  4 Bit 7:0  Byte  5 Bit 7:0 | 用于表示这个请求的相应完成包的“返回地址”。  Byte 4,7:0=Bus  Number  Byte 5,7:3=Device  Number  Byte 5,2:0=Function  Number |
| Tag[7:0]  Tag                                                | Byte  6 Bit 7:0                  | 这个字段用来标识来自 Requester 的具体每个请求。每个被发出的请求包都会被分配一个独一无二的 Tag 值。默认情况下，只使用 Tag[4:0]（也就是同时可以有 32 个未完成的事务在进行），如果 Control Register 中的 Extended Tag 位被置为 1，那么可以将 Tag 的使用扩展为 Tag[7:0]，这样就允许最多使用 256 个 Tag。 |
| Last  DW BE[3:0]  尾 DW 字节使能                               | Byte  7 Bit 7:4                  | 这个字段用于限定最后一个 DW 数据内的有效字节。由于配置请求的数据荷载只能为 1DW，因此这个字段必须为 0 |
| 1st  DW BE[3:0]  首 DW 字节使能                                | Byte  7 Bit 3:0                  | 这个字段用于限定第一个 DW 数据内的有效字节。对于配置请求来说，这个字段的所有组合都是允许的（包括全 0）。 |
| Completer  ID[15:0]  Completer  ID                           | Byte  8 Bit 7:0  Byte  9 Bit 7:0 | 用于指示这个配置请求要访问的 Completer。  Byte 8,7:0=Bus Number  Byte 9,7:3=Device Number  Byte 9,2:0=Function Number |
| Ext  Register Number[3:0]  扩展寄存器号  （Extended Register Number） | Byte  10 Bit 3:0                 | 这个字段提供了在访问扩展配置空间（Extended Config Space）时 DW 空间的高 4bit，它与 Register Number 共同组合成 10bit 地址，用于访问 1024DW（也就是 4096Byte）的空间。对于 PCI-Compatible 配置空间，这个字段必须为 0。 |
| Register  Number[3:0]  寄存器号  （Register Number）         | Byte  11 Bit 7:0                 | 用于指示寄存器号，是配置 DW 空间的低 8bit。最低的 2bit 始终为 0，以强制进行 DW 对齐的访问。 |

表 5‑6 配置请求 Header 各字段

###### l Configuration Request 注意事项

配置请求的 Header 总为 3DW 格式，路由信息为 Bus Number、Device Number 和 Function Number。所有的设备都会在接收到一个 Type 0 配置写请求时从请求包中捕获出它们自己的 Bus Number 和 Device Number。之所以这样，是因为这些设备之后会用自己捕获到的 Type 0 配置写中的 Bus Number 和 Device Number 来作为 Requester ID，用来在以后发起请求时使用。

##### 5.2.5.4   完成包（Completion）

Completion（完成包）是用来响应 Non-Posted 请求的，除非有错误阻止了发送完成包。例如，Memory、IO、Configuration Read 请求通常就是由带有数据的完成包来响应。另一方面，IO 或者 Configuration 写请求通常也由无数据的完成包来响应，这种情况下的完成包仅仅是用来报告事务的状态。

在完成包中的许多字段的值，都与它相关的请求包中的字段相同，包括 TC（Traffic Class，流量类型）、Attr（Attribute，属性）以及 Requester ID（请求者 ID，用来将完成包路由回到请求者去）。图 5‑10 中展示了一个响应 Non-Posted 请求的完成包，并且还展示了这个完成包的 3DW Header。在正常操作中，Completer ID 并没有什么额外的用处，但若是在系统调试期间能够知道完成包从哪里来，那么这对错误的诊断就有较大的帮助。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image230.jpg)

图 5‑10 3DW 完成包 Header 格式

###### l Completion Header 各字段的定义

图 5‑10 中展示的 Completion Header 中的每个字段的位置和作用都在表 5‑7 中进行了描述。

| 字段名称                                                    | 位于 Header 中的 Byte/Bit           | 功能                                                         |
| ----------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                                  | Byte  0 Bit 7:5                  | 完成包始终为 3DW Header：  000b=Cpl，无数据  010b=CplD，有数据 |
| Type[4:0]  类型                                             | Byte  0 Bit 4:0                  | 0 1010b=完成包                                               |
| TC[2:0]  流量类型  （Traffic Class）                        | Byte  1 Bit 6:4                  | 完成包的该字段必须与相应的请求包相同。                       |
| Attr[2]  属性  （Attribute）                                | Byte  1 Bit 2                    | 表示这个 TLP 是否使用基于 ID 的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering（IDO）”一节。 |
| TH  TLP 处理提示  （TLP Processing Hints）                   | Byte  1 Bit 0                    | 完成包中，此字段为保留字段                                   |
| TD  TLP 摘要  （TLP Digest）                                 | Byte  2 Bit 7                    | 用于指示 TLP 的末尾是否有 1DW 的 Digest 字段，如果为 1 则说明存在。  |
| EP  受污染的数据  （Poisoned Data）                         | Byte  2 Bit 6                    | 如果为 1，则表示数据荷载是被污染的。                          |
| Attr[1:0]  属性  （Attribute）                              | Byte  2 Bit 5:4                  | 完成包的该字段必须与相应的请求包相同。                       |
| AT[1:0]  地址类型  （Address Type）                         | Byte  2 Bit 3:2                  | 完成包的 Address Type 字段为保留字段，必须为 0。                |
| Length[9:0]  长度                                           | Byte  2 Bit 1:0  Byte  3 Bit 7:0 | 用于表示完成包数据荷载的大小，单位为 DW。                     |
| Completer  ID[15:0]  Completer  ID                          | Byte  4 Bit 7:0  Byte  5 Bit 7:0 | 用于表示 Completer 是谁，用于支持调试。  Byte 4,7:0=Compl  Bus Number  Byte 5,7:3=Compl  Device Number  Byte 5,2:0=Compl  Function Number |
| Compl.  Status[2:0]  完成包状态  （Completion Status Code） | Byte  6 Bit 7:5                  | 这个字段用于表示完成包的状态。  000b=Successful  Completion（SC）成功完成  001b=Unsupported  Request（UR）不支持的请求  010b=Config  Req Retry Status（CRS）配置请求重试状态  100b=Completer  Abort（CA）完成者终止  除了上述 bit 组合之外的都是保留码。更多内容请参阅“Summary of Completion Status Codes”一节。 |
| BCM  字节数已修改  （Byte Count Modified）                  | Byte  6 Bit 4                    | 这个字段仅由 PCI-X 完成包使用，用来表示 Byte Count 字段仅报告第一个数据荷载的大小，而不是总数据荷载的大小。更多内容请参阅“Using The Byte Count Modified Bit”一节。 |
| Byte  Count[11:0]  字节数                                   | Byte  6 Bit 3:0  Byte  7 Bit 7:0 | Byte Count 字段是用来服务读请求的，它也是来源于原始请求中的 Length 字段。一些特别情况下，会出现使用多完成包返回数据荷载，请参阅“Data  Returned For Read Request”一节 |
| Requester  ID[15:0]  Requester  ID                          | Byte  8 Bit 7:0  Byte  9 Bit 7:0 | 该字段是从原始请求中复制来的，用于作为路由信息来帮助完成包返回 Requester。  Byte 4,7:0=Req  Bus Number  Byte 5,7:3=Req  Device Number  Byte 5,2:0=Req  Function Number |
| Tag[7:0]  Tag                                               | Byte  10 Bit 7:0                 | 完成包中的 Tag 必须与相应请求中的 Tag 一样。Requester 收到这个完成包时是通过 Tag 来将完成包与原始请求关联起来的。 |
| Lower  Address[6:0]  低位地址                               | Byte  11 Bit 6:0                 | 这个字段是一个读请求所返回的第一组数据的低 7bit 地址。它是由请求 Length 和 Byte Enable 共同计算出来的，它可以展示出在到达下一个读完成包边界之前还能传输多少字节数据，以此来协助 Buffer 管理。更多内容请参阅“Calculation Lower Address Field”一节。 |

表 5‑7 完成包 Header 格式

###### l 完成包状态码总结（Summary of Completion Status Codes）

- 000b（SC）Successful Completion 成功完成：请求被正确服务。
	
- 001b（UR）Unsupported Request 不支持的请求：对于 Completer 来说，此请求是非法的或者是无法被识别的。这是一种出错的情况，至于 Completer 如何进行响应则要根据 Completer 对应的 PCIe 协议版本来。在 PCIe 1.1 之前，这种情况被视为一种不可纠正的错误，但是在 1.1 和之后的版本中，它被作为一种 Advisory Non-Fatal Error（报告的非致命错误）。更多详细内容请参阅“Unsupported Request（UR） Status”一节。
	
- 010b（CRS）Configuration Request Retry Status 配置请求重试状态：Completer 临时的无法服务一个配置请求，因此这个请求应该稍后再次尝试。
	
- 100b（CA）Completer Abort 完成者终止：Completer 应该已经对请求事务进行了服务，但是因为某些原因导致了服务失败。这是一种不可纠正的错误。

###### l 计算低位地址字段（Calculating the Lower Address Field）

Completer 设置这个字段是用来反映返回给 Requester 的数据荷载中，第一个 enabled 的字节的字节对齐地址（byte-aligned address）。硬件会通过原始请求中的 DW 起始地址以及首 DW 字节使能字段（1st DW Byte Enable）中的字节使能情况，来计算出这个地址的值。

对于 MRd（Memory Read）请求，这个地址就是从 DW 起始地址开始的偏移量：

- 如果首 DW 字节使能字段为 1111b，也就是第一个 DW 中的所有字节都是有效的，那么偏移量就是 0。此时这个字段就与 DW 对齐的起始地址相同。

- 如果首 DW 字节使能字段为 1110b，也就是第一个 DW 中的高 3 个字节都是有效的，那么偏移量就是 1。此时这个字段就等于 DW 对齐的起始地址+1。
	
- 如果首 DW 字节使能字段为 1100b，也就是第一个 DW 中的高 2 个字节都是有效的，那么偏移量就是 2。此时这个字段就等于 DW 对齐的起始地址+2。
	
- 如果首 DW 字节使能字段为 1000b，也就是第一个 DW 中的仅有最高字节是有效的，那么偏移量就是 3。此时这个字段就等于 DW 对齐的起始地址+3。

 




一旦上述的计算完成，计算结果的低 7bit 就会被放入完成包 Header 的 Lower Address 字段，当读完成包小于整个数据荷载因而需要停止在第一个 RCB（Read Completion Boundary）时，这个字段可以有助于更好的处理这种情况。若想将一个事务拆分成多个部分，则必须要在 RCB 上进行拆分，而到达第一个 RCB 时所传输的字节数量是根据起始地址而定的。

对于 AtomicOp（原子操作）完成包，Lower Address 字段是保留位。对于所有其他完成包类型（非读请求、非原子操作），这个字段必须为 0。

###### l 使用字节数修改位（Using The Byte Count Modified Bit）

这个 bit 仅由 PCI-X Completer 进行设置，但是如果使用了 PCIe 到 PCI-X 的 Bridge，那么这些 PCI-X Completer 们就也可以出现在 PCIe 拓扑结构中。使用这个 bit 的规则包括：

1.    当一个读请求要拆分成多个完成包来返回数据时，Byte Count Modified 这一位只能由 PCI-X Completer 进行置位。

2.    仅有一系列完成包中的第一个可以将这个 bit 置为 1，这表示第一个完成包内包含的 Byte Count 字段只反映这个完成包（第一个完成包）的数据荷载，而不是整个事务的数据荷载（通常情况下是表示整个事务的总数据荷载）。这样 Requester 就知道，即使 Byte Count 看起来似乎表示这是这个请求的最后一个完成包，但是实际上这个完成包之后还会有完成包，以此来满足原始请求对数据量的需求。

3.    对于随后的一系列完成包来说，BCM 位必须置为 0，且 Byte Count 字段要用来反映剩下的所有数据荷载额字节数，就像它通常的含义一样。

4.    当设备接收到 BCM 位为 1 的完成包时，它必须要正确的处理这种情况。

5.    Completer 要在带有数据荷载的完成包中设置 Lower Address 字段，以此来反映返回的第一个有效字节的地址。

###### l 读请求数据返回（Data Returned For Read Requests）:

1.    一个读请求可能需要多个完成包来返回数据，但是最终传输的数据总量必须等于原始请求中要求的大小，否则可能导致完成包超时错误（Completion Timeout error）。

2.    一个完成包只能用于服务一个请求。

3.    IO 和 Configuration 读请求所请求的数据量永远是 1DW，并且只能由一个单独的完成包来返回数据。

4.    状态码不是 SC（Successful Completion）的完成包将会终止当前事务。

5.    在使用多个完成包来响应读请求时，必须注意读完成包边界（RCB，Read Completion Boundary）。对于 RC 来说，RCB 会是 64Bytes 或者 128Bytes，这是因为 RC 可以更改在其端口间移动的数据包的大小，具体的 RCB 的值可以在配置寄存器中看到。

6.    Bridge 和 EP 可以用一个 bit 来实现软件对 RCB 大小的选择，64bytes 或者 128bytes。

7.    完全在一个对齐 RCB 边界内的完成包们必须一次完成传输，这是因为这些 RCB 边界内的完成包的传输是不会到达 RCB 的，而 RCB 才是允许提前停止传输的位置，这样就必须对一个 RCB 边界内的所有完成包一次完成传输而不能中途停止。

8.    对于一个单独的读请求来说，若使用多个完成包来返回数据，那么这些数据必须以地址递增的顺序来返回。

###### l 接收者对完成包的处理规则（Receiver Completion Handling Rules）:

1.    如果一个已经被接收的完成包并没有匹配上任何一个待完成的请求，那么它就是一个 Unexpected Completion（意外完成包），将被当做是一种错误来处理。

2.    如果完成包状态不是 SC 或者 CRS，那么就将被当做是错误来处理，并且会清空与之相关联的 Buffer 空间。

3.    当 RC 在进行配置事务时接收到一个 CRS 状态的完成包，那么这个配置请求就被终止了。随后要进行的事情是根据具体实现而不同的，但是如果 RC 支持这种 CRS 状态的完成包处理，那么具体的操作将由 RC 控制寄存器中的 CRS Software Visibility 位来进行定义设置。

—  如果 CRS Software Visibility 没有启用，那么 RC 将会重新发出配置请求，直到重复了一定的次数（具体实现中指定）之后就会放弃重发的操作，并认为请求目标存在问题。

—  如果 CRS Software Visibility 是启用的，那么用来支持它的软件都会首先读取 Vendor ID 字段的两个字节，如果硬件接收到了这个读请求的 CRS 完成包，它将给返回的 Vendor ID 将会是 0001h。这个值是 PCI-SIG 规定的保留值，它并不对应任何有效的 Vendor，并会通知软件发生了这样的事件。这使得软件可以在等待目标 Function 准备就绪的这段时间里（通常在复位后需要 1s 时间），去执行其他的任务，而不是停滞不前。任何其他的配置读或者写（除了最开始的配置读 Vendor ID）都会被 RC 自动重试重发，直到达到设计的重复次数。

4.    如果一个请求并不是配置请求，但是它的完成包出现了 CRS 状态，那么这个完成包将被认为是一个畸形 TLP（Malformed TLP）。

5.    当完成包状态码为保留组合时，并不是定义出的那几种，就把它当做 UR（Unsupported Request）一样处理。

6.    如果接收到的一个读完成包或者一个原子操作完成包的状态码不是 SC，那么这个完成包就不会携带数据，并且 Requester 必须要考虑终止这个完成包对应的请求。至于 Requester 具体如何处理这种情况则根据具体实现而定。

7.    在使用多个完成包来响应一个读请求的情况下，任何一个完成包的状态只要不是 SC，那么就会终止这个事务。设备如何处理出错之前接收到的数据根据具体实现而定。

8.    为了保持对 PCI 的兼容性，RC 需要在一个配置事务被 UR 结束时，合成一个全 1 的读取值。这就类同于枚举软件尝试从不存在的设备中读取数据时的出现的 PCI Master Abort 一样。

##### 5.2.5.5   消息请求（Message Requests）

Message 请求取代了 PCI/PCI-X 中使用的许多中断（interrupt）、错误（error）和电源管理（power management）的边带信号。所有的 Message 请求使用的都是 4DW Header 格式，但是并不是每种 Message 都使用了 Header 中的每一个字段。对于 Header 中的 Byte8-15 来说，在一些种类的 Message 中就没有其定义，在这种情况下它就是保留字段。Message 的处理方式更像是 Posted 的 MWr 事务，但是 Message 的路由方式可以是基于地址的、基于 ID 的，在一些情况下还可以是隐式路由。TLP Header 中的路由子域（routing subfield），也就是 Byte 0 bit 2:0，它用来表示使用的是哪种路由方法，并且相应的在 Header 中定义了哪些附加的字段，例如使用基于 ID 路由的 Header 中就会有用于路由的 BDF。通用的 Message 请求 Header 格式如图 5‑11 所示。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image232.jpg)

图 5‑11 4DW 的 Message 请求 Header 格式

 

 

 

###### l Message 请求 Header 各字段（Message Request Header Fields）

| 字段名称                                  | 位于 Header 中的 Byte/Bit                                       | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                | Byte  0 Bit 7:5                                              | Message 始终为 4DW Header：  001b=4DW，无数据  <br>011b=4DW，有数据 |
| Type[4:0]  类型                           | Byte  0 Bit 4:0                                              | TLP 包的 Type 字段。 <br> Bit 4:3：  10b=Msg <br> Bit 2:0：（Message 路由子域）<br>000b=到 RC 的隐式路由。<br>001b=使用地址路由  <br/>010b=使用 ID 路由  <br/>011b=来自 RC 的隐式路由广播 <br/>100b=本地；终结于首级接收者  <br/>101b=收集并路由至 RC  其他值=保留值，作为本地处理 |
| TC[2:0]  流量类型  （Traffic Class）      | Byte  1 Bit 6:4                                              | Message 的 TC 始终为 0，以确保它们不会影响到高优先级的数据包。   |
| Attr[2]  属性  （Attribute）              | Byte  1 Bit 2                                                | 表示这个 TLP 是否使用基于 ID 的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering（IDO）”一节。 |
| TH  TLP 处理提示  （TLP Processing Hints） | Byte  1 Bit 0                                                | Message 中，除非另有注明，此字段为保留字段。                  |
| TD  TLP 摘要  （TLP Digest）               | Byte  2 Bit 7                                                | 如果为 1，则说明 TLP 的末尾存在 1DW 的 Digest 字段（在 LCRC 和 END 字符之前）。 |
| EP  受污染的数据  （Poisoned Data）       | Byte  2 Bit 6                                                | 如果为 1，则表示数据荷载是被污染的。                          |
| Attr[1:0]  属性  （Attribute）            | Byte  2 Bit 5:4                                              | Message 中，除非另有注明，此字段为保留字段。                  |
| AT[1:0]  地址类型  （Address Type）       | Byte  2 Bit 3:2                                              | Message 的 Address Type 字段为保留字段，必须为 0，但是并不要求甚至不鼓励接收方对这个字段进行检查。 |
| Length[9:0]  长度                         | Byte  2 Bit 1:0  Byte  3 Bit 7:0                             | 用于表示完成包数据荷载的大小，单位为 DW。对于 Message 来说，这个字段始终为 0（无数据）或 1（有 1DW 数据）。 |
| Requester  ID[15:0]  Requester  ID        | Byte  4 Bit 7:0  Byte  5 Bit 7:0                             | 用于表示发送 Message 的 Requester 是谁。  Byte 4,7:0=Requester  Bus Number  Byte 5,7:3=Requester  Device Number  Byte 5,2:0=Requester  Function Number |
| Tag[7:0]  Tag                             | Byte  6 Bit 7:0                                              | 由于所有的 Message 请求都是 Posted 的，不需要 Requester 接收完成包，因此不需要给 Message 分配 Tag。这个字段应该为 0。 |
| Message  Code[7:0]  Message 标识码         | Byte  7 Bit 7:0                                              | 这个字段包含的标识码表示发送的 Message 是什么种类。 <br> 0000 0000b=解锁 Message（Unlock Message） <br> 0001 0000b=延迟容忍报告（Lat. Tolerance Reporting） <br> 0001 0010b=优化的 Buffer 刷新/填满（Optimized Buffer Flush/Fill） <br> 0001 xxxxb=电源管理 Message（Power Mgt. Message） <br> 0010 0xxxb=INTx Message <br> 0011 00xxb=错误 Message（Error Message） <br> 0100 xxxxb=忽略 Message（Ignored Message） <br> 0101 0000b=设置插槽功耗 Message（Set Slot Power Message） <br> 0111 111xb=厂商定义的 Message（Vendor Defined Message） |
| Address[63:32]  高 32bit 地址               | Byte  8 Bit 7:0  Byte  9 Bit 7:0  Byte  10 Bit 7:0  Byte  11 Bit 7:0 | 如果 Message 选用了地址路由（见上面介绍的 Type[4:0]字段），那么这个字段的内容就是 64bit 起始地址的高 32bit。如果使用是 ID 路由，那么 Byte8-9 就组成目标 ID。  否则，这个字段就不使用。 |
| Address[31:0]  低 32bit 地址                | Byte  12 Bit 7:0  Byte  13 Bit 7:0  Byte  14 Bit 7:0  Byte  15 Bit 7:0 | 如果 Message 选用了地址路由（见上面介绍的 Type[4:0]字段），那么这个字段的内容就是 64bit 起始地址的低 32bit（最低的 2bit 必须为 0，以保持 DW 对齐）。  否则，这个字段就不使用。 |

表 5‑8 Message 请求 Header 各字段

###### l Message 注意事项（Using The Byte Count Modified Bit）

接下来几个小节的列表会列出 9 种 Message 组（如表 5‑8 中给出的）的各自使用的更具体的 Message Code。这 9 种 Message 组为：

1.    INTx 中断信号（INTx Interrupt Signaling）

2.    电源管理（Power Management）

3.    错误信号（Error Signaling）

4.    支持锁定事务（Locked Transaction Support）

5.    支持插槽功耗限制（Slot Power Limit Support）

6.    厂商定义的 Message（Vendor-Defined Message）

7.    忽略 Message（Ignored Message，与 PCIe 1.1 中的热插拔支持相关）

8.    延迟容忍报告（Latency Tolerance Reporting，LTR）

9.    优化的 Buffer 刷新和填满（Optimized Buffer Flush and Fill，OBFF）

###### l INTx 中断 Message（INTx Interrupt Messages）

许多设备都适用 PCI 2.3 的 MSI（Message Signaled Interrupt）方法来传输中断，但是对于老的设备来说可能会不支持 MSI。对于这些情况，PCIe 定义了一个“虚拟线 virtual wire”作为替代方案，设备通过发送 Message 来模拟 PCI 中断引脚（INTA-INTD）的拉起与释放。中断设备发出第一个 Message 来通知上行设备自己拉起了一个中断。一旦这个中断被响应处理（Serviced）了，那么中断设备就发出第二个 Message 来表示这个“虚拟中断线”已经释放。更多关于这种协议的内容，请参阅“Virtual INTx Signaling”一节。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image234.jpg)

表 5‑9 INTx 中断信号 Message 的代码

关于使用 INTx Message 的一些规则：

1.    INTx Message 没有数据荷载，因此 Length 字段为保留字段。

2.    它们只会由上行端口（Upstream Port）发出。对接收到的数据包进行这方面的规则检查是可选项，但是如果进行了检查，那么发现违例的数据包就会被当做畸形 TLP（Malformed TLP）。

3.    INTx Message 需要使用默认的流量类型 TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形 TLP（Malformed TLP）。

4.    链路两端的组件必须跟踪四种虚拟中断（virtual interrupt）的状态。如果上行端口的一个中断的逻辑状态发生了改变，那么它必须发出相应的 INTx Message。

5.    当命令寄存器（Command Register）中的 Interrupt Disable 位被置为 1 时，INTx 信号是被禁用的（就像物理中断线的类似情况一样）。

6.    如果在 Interrupt Disable 位为 1 时，设备的任何一个虚拟 INTx 信号仍为激活状态，那么上行端口必须发出对应的释放 INTx Message（Deassert_INTx Message）。

7.    Switch 的各个下行端口必须独立的跟踪四种 INTx 信号状态，并组合成上行端口的状态。

8.    RC 的各个下行端口必须独立的跟踪下行端口的 INTx 信号状态，并将它们转换成系统中断，具体的转换方式根据具体实现而定。

9.    INTx Message 使用“Local-Terminate at Receiver 在接收者本地结束”的路由类型来使得一个 Switch 在必要时对指定的中断引脚进行重映射（参阅“Mapping and Collapsing INTx Messages”一节）。因此，INTx Message 中的 Requester ID 有可能是最后一个传送者所指定的。

###### l 电源管理 Message（Power Management Messages）

PCIe 对 PCI 的电源管理是兼容的，并且还加入了基于硬件的链路电源管理。Message 是用来传达一些关于电源管理的信息，但是如果想了解完整的 PCIe 电源管理协议是如何工作的，那么请参阅 Chapter 16 “Power Management”一章。表 5‑10 总结了四种电源管理 Message 类型。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image236.jpg)

表 5‑10 电源管理 Message 标识码

关于使用电源管理 Message 的一些规则：

1.    电源管理 Message 没有数据荷载，因此 Length 字段为保留字段。

2.    电源管理 Message 需要使用默认的流量类型 TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形 TLP（Malformed TLP）。

3.    当一个下行端口收到了一个链路对端电源管理请求，请求要将链路电源状态更改为 L1，但是下行端口拒绝这种更改，那么它就要发出 PM_Active_State_Nak 这种电源管理 Message。

4.    当一个组件请求电源管理事件（Power Management Event）时，它的上行端口需要发出 PM_PME，这个 Message 将被隐式路由至 RC。

5.    PM_Turn_Off 会向下发送给所有的 EP（由 RC 发出，进行隐式路由的广播）。

6.    PME_TO_Ack 是由 EP 的上行端口发出的。对于拥有多个下行端口的 Switch 来说，只有当所有的下行端口都收到了这个 Message，才会将其转发至上行（也就是汇聚收集，并路由转发给 RC）。

###### l 错误 Message（Error Messages）

当启用了错误 Message 的组件检测到了错误，那么就会将错误 Message 向上发送（隐式路由至 RC）。为了协助软件，让软件知道应该如何处理这个错误，在 Error Message 中使用 Header 中的 Requester ID 字段来表示请求者。表 5‑11 总结了三种 Error Message 类型。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image238.jpg)

表 5‑11 错误 Message 标识码

关于使用 Error Message 的一些规则：

1.    Error Message 没有数据荷载，因此 Length 字段为保留字段。

2.    Error Message 需要使用默认的流量类型 TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形 TLP（Malformed TLP）。

3.    RC 会将 Error Message 转换成系统指定的事件。

###### l 支持锁定事务（Locked Transaction Support）

解锁 Message（Unlock Message）被应用于 PCI 定义的锁定事务协议中（Locked Transaction Protocol），并且在 PCIe 中依然对传统 PCI 遗留设备可用。这种协议起始于一个 MRd Lock 请求。当这个请求在被送达到目标设备的过程中，沿途的端口们都会发现它，这些端口会实现一个原子“读-修改-写”协议（read-modify-write protocol），也就是说它们将会锁定 VC0（Virtual Channel 0）让其他的请求不能使用，直到收到 Unlock Message 才会对 VC0 解锁。这种 Unlock Message 会被发送至 Locked 事务的目标设备，以此来释放传输路径中的所有被锁定的端口，并用于最终完成锁定事务的一系列行为。表 5‑12 总结了 Unlock Message 的标识码。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image240.jpg)

表 5‑12 解锁 Message 标识码

关于使用 Unlock Message 的一些规则：

1.    Unlock Message 没有数据荷载，因此 Length 字段为保留字段。

2.    Unlock Message 需要使用默认的流量类型 TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形 TLP（Malformed TLP）。

###### l 设置插槽功率限制 Message（Set Slot Power Limit Message）

这种 Message 是由下行端口发送给插在插槽上的设备的。这里面的功率限制信息会储存在 EP 的设备能力寄存器中（Device Capabilities Register）。表 5‑12 总结了 Unlock Message 的标识码。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image242.jpg)

表 5‑13 插槽功率限制 Message 标识码

关于使用 Set Slot Power Limit Message 的一些规则：

1.    Set Slot Power Limit Message 需要使用默认的流量类型 TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形 TLP（Malformed TLP）。

2.    Set Slot Power Limit Message 的数据荷载为 1DW，因此 Length 字段的值为 1。而在 32bit 数据荷载中仅有低 10bit 用于进行插槽功率放大或减小；剩下的高位必须都置为 0。

3.    当数据链路层转换为 DL-Up 状态时，该消息会自动发送。或者是放数据链路层已经报告了 DL-Up 状态，然后发生了一个配置写来写入 Slot Capabilities Register 时，该消息也会自动发送。

4.    如果插槽中的板卡消耗的功率已经低于了指定的功率限制，那么可以忽略这种 Message。

###### l 厂商定义的 Message 0 和 1（Vendor-Defined Message 0 and 1）

Vendor-Defined Message 是用于进行 PCIe Message 能力的扩展，这种扩展既可以通过协议本身，也可以通过厂商来指定的扩展。这种 Message 的 Header 格式如图 5‑12 所示，Message 标识码如表 5‑12。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image244.jpg)

图 5‑12 厂商定义的 Message Header 格式

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image246.jpg)

表 5‑14 厂商定义的 Message 标识码

关于使用Vendor-Defined Message 的一些规则：

1.    Vendor-Defined Message 0 和1 可以有也可以没有数据荷载。

2.    Message 用Vendor ID 来进行区分。

3.    Attr[2]和Attr[1:0]并不是保留位。

4.    如果接收者无法认出这种Message：

	- Type 1 Message 可以被安静的忽略丢弃掉。

	- Type 0 Message 要被作为Unsupported Request（UR）这种错误情况来处理。

###### l 被忽略的Message（Ignored Message）

如果直接列出一整个目录的需要被忽略的Message，而不去讲忽略的前因后果，那可能会听起来有一些奇怪。这些Message 其实是原来的热插拔信号Message（Hot Plug Signaling Message），它们用于支持设备，这些设备上具有热插拔指示器，并且只需要按下这个插卡设备上的按钮而不是系统板上的按钮。这种Message 类型是由PCIe 1.0a 版本所定义的，但是这个选项在PCIe 1.1 版本中就已经不再支持了，因此这里所讲的这些细节内容仅作为参考。正如名字要表达的那样，强烈建议发送方不要发送这些Message，并且也强烈建议接收方就算收到它们也要忽视掉。如果无论如何仍要使用这些Message，那么它们必须要符合PCIe 1.0a 的协议细节。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image248.jpg)

表 5‑15 热插拔Message 标识码

关于使用Hot Plug Message 的一些规则：

1.    Hot Plug Message 由下行端口发送给插槽中的板卡。

2.    Attention_Button Message 是由插槽中的设备向上行发送的。

###### l 延迟容忍报告Message（Latency Tolerance Reporting Message）

LTR Message 是用来报告一个设备可接受的读写服务延迟，这是一个可选项。更多关于这种电源管理技术的内容，请参阅“LTR（Latency Tolerance Reporting）”一节。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image250.jpg)

图 5‑13 LTR Message Header 格式

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image252.jpg)

表 5‑16 LTR Message 标识码

关于使用LTR Message 的一些规则：

1.    LTR Message 没有数据荷载，因此Length 字段为保留字段。

2.    LTR Message 需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

###### l 优化的Buffer 刷新和填充Message（Optimized Buffer Flush and Fill）

OBFF Message 用来向EP 报告平台电源情况，以此来促进更高效的系统电源管理。更多关于这种技术的内容，请参阅“OBBF（Optimized Buffer Flush and Fill）”一节。

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image254.jpg)

图 5‑14 OBBF Message Header 格式

![img](img/5%20TLP%20%E5%85%83%E7%B4%A0/clip_image256.jpg)

表 5‑17 OBBF Message 标识码

关于使用OBFF Message 的一些规则：

1.    OBFF Message 没有数据荷载，因此Length 字段为保留字段。

2.    OBFF Message 需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

3.    Header 中的Requester ID 必须为传送中的端口的ID。


------

原文：  Mindshare

译者：  Michael ZZY

校对：  

欢迎参与 《Mindshare PCI Express Technology 3.0 一书的中文翻译计划》

https://gitee.com/ljgibbs/chinese-translation-of-pci-express-technology