# 第二部分    Part Two：Transaction Layer事务层

## Chapter 5      TLP Element //TLP元素

### 关于上一章

上一章描述了一个Function通过BARs请求地址空间（内存地址空间或IO地址空间）的目的和方法，还描述了软件需要怎样配置Bridge的Base/Limit寄存器来将源端口的TLP路由至正确的目的端口。我们还讨论了PCIe中TLP路由的一般概念，包括基于地址的路由、基于ID的路由，以及隐式路由。

### 关于本章

在PCIe设备之间，信息是以包的形式进行传输的。有三类主要的包：TLP（Transaction Layer Packet，事务层包）、DLLP（Data Link Layer Packet，数据链路层包）和Ordered Set（命令集，它应用于物理层）。本章将讲述TLP的用法、格式和不同种类的定义，并将详细的讲解TLP的相关字段含义。关于DLLP的内容将会在Chapter 9 “DLLP Element”中进行单独讲解。

### 关于下一章

下一章将讨论流量控制协议（Flow Control Protocol）的目的和详细操作。流量控制是用来确保发送方在接收方无法接收TLP时，不会再继续发送TLP。这避免了接收缓存的溢出，也消除了原本PCI工作方式中的一些低效行为，比如断开（disconnect）、重试（retry）和等待态（wait-state）。

### 5.1 基于包的协议的介绍（Introduction to Packet-Based Protocol）

#### 5.1.1     整体说明（General）

不同于并行总线，PCIe这样的串行总线不使用总线上的控制信号来表示某时刻链路上正在发生什么。相反地，PCIe链路上的发送方发出的比特流必须要有一个预期的大小，还要有一个可供接收方辨认的格式，这样接收方才能理解比特流的内容。此外，PCIe在传输数据包时并不使用任何直接握手机制（immediate handshake）。

除了逻辑空闲符号（Logical Idle symbol）和物理层包也就是Ordered Set之外，在活跃的PCIe链路上移动传输的信息的基本组块被称为Packet（包），包是由符号组成的。链路上交换的两类主要的数据包为高层的TLP（Transaction Layer Packet，事务层包），和低层的用于链路维护的包称为DLLP（Data Link Layer Packet，数据链路层包）。这些包和它们的传输流如图 5‑1所示。物理层的Ordered Set也是一种包，但是它并不像TLP和DLLP一样会被封装上包起始符号和包结束符号（也就是前面章节所讲的组帧符号），并且Ordered Set也并没有像TLP和DLLP一样的字节条带化过程，相反地，Ordered Set会在链路的每个通道（lane）上都复制一份，而不是像字节条带化一样把信息按字节分配到各个通道上。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image210.jpg)

图 5‑1 TLP和DLLP包

#### 5.1.2     使用基于包的协议的原因

使用基于包的协议（Packet-Based Protocol）有三个明显的优点，特别是对于数据完整性（data integrity）来说：

##### 5.1.2.1   包格式是精心定义的



像PCI这种早期的总线，它们允许总线上传输不确定的数据量大小，这使得只要传输没有结束就无法识别出数据荷载（payload）的边界。此外，任何一个设备都可以在此次传输完成前终止这个传输，这使得发送方很难去计算并传输一个覆盖了整个数据荷载的校验和或者CRC。相反地，PCI使用了一种简单的奇偶校验的方案，并在每个数据阶段（data phase）进行奇偶校验。（这里如果没理解请参阅Chapter 1内的PCI相关内容）

相比之下，PCIe包具有一个已知的大小和格式。位于包的开头的部分为Header，它用来指示这个包的类型，并包含了必需字段（required field）和可选字段（optional field）。除了地址字段以外，Header中的其他字段长度都是固定的，地址字段可以为32bit也可以为64bit。一旦一次传输开始，接收方不能暂停或者提前终止它。这种结构化的格式使得我们可以在TLP中加入一些信息来更有利于进行可靠的传输，例如加入组帧符号（framing symbol）、CRC、以及一个包的序列号（Sequence Number）。

##### 5.1.2.2   使用组帧符号定义包的边界

在PCIe Gen1和Gen2操作模式中使用的是8b/10b编码，因此在这Gen1和Gen2中每个TLP和DLLP在发送前都会使用起始符号（Start）和结束符号（End）这两种控制符号来进行组帧，这样就可以给接收方清晰地定义出包的边界。这是在PCI和PCI-X上的重大改进，在PCI和PCI-X中还是使用一个单独的FRAME#信号来指示一个事务的开始和结束，如果FRAME#出现了毛刺，或者其他的控制信号出现了毛刺，那么都有可能造成目标设备对总线行为的误解。一个PCIe接收方必须要能在最后的链路活动开始或结束之前，就正确的完成一个完整10bit符号的译码，这样子那些不期望出现或者无法识别的符号就能更容易的被识别，并且被作为一个错误来处理。

对于PCIe Gen3使用的128b/130b编码来说，控制字符不再被使用，并且也没有组帧符号了。对于更多的关于Gen3编码与早期版本的差异的内容，请参阅Chapter 12“Physical Layer-Logical（Gen3）”。

##### 5.1.2.3   使用CRC保障整个数据包的正确传输

在PCI体系结构中，会在地址阶段（address phase）和数据阶段（data phase）中使用奇偶校验边带（side-band）信号，但是在PCIe中则不同。PCIe中使用带内（in-band）的CRC值来验证整个数据包是否进行了无错误的传输。同时TLP还会被发送方的数据链路层添加上一个序列号（Sequence Number），这使得当这个序列号的数据包传输出错时可以很简单的定位到它，并进行自动的重传。发送方会在自己的Retry Buffer内保存每个TLP的一个副本，直到接收方确认了这个TLP成功无错传输后才会将副本清除。这种TLP的确认机制被称为Ack/Nak协议（更多内容请参阅Chapter 10“Ack/Nak Protocol”），它用来形成基础的链路级TLP错误检测和纠正机制。这种Ack/Nak协议提供的错误恢复机制使得我们可以在问题发生的地方或者链路上及时的解决问题，但是这也要求有一个本地的硬件解决方案来支持这种协议。

### 5.2 TLP细节（TLP Details）

在PCIe中，高层次事务起源于发送方的Device Core，终止于接收方的Device Core。事务层会处理这些请求，其中，发送端的事务层组装TLP，接收端的事务层解析TLP。在这个过程中，每个设备的数据链路层和物理层也会对包的组装有一定的贡献。

#### 5.2.1     TLP的组包和拆包

如图 5‑2所示的是链路的发送端组装TLP和接收端拆解TLP的一般流程。现在让我们讲一讲从一个包的生成，到它被传送到接收端的Device Core的各个步骤。下面列出了TLP组包和拆包的关键阶段，列出的编号与图 5‑2中的编号相对应。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image212.jpg)

图 5‑2 PCIe TLP的组包与拆包

n 发送方

\1.    设备A的Device Core向它的PCIe接口发送一个请求（具体Device Core是如何给PCIe接口发送请求的，这并不是PCIe协议或者本书的讨论范畴）。这个请求中包括：

—  目标地址或者ID（也就是路由信息）

—  源端信息，例如Requester ID和Tag

—  事务类型/包类型（需要执行的命令，例如一个MRd）

—  数据荷载大小payload size和数据荷载内容（如果TLP需要带数据）

—  流量类型（Traffic Class，用于分配包的优先级）

—  请求的自身属性（No Snoop无窥探、Relaxed Ordering宽松排序，等等）

\2.    基于这个请求，事务层将会组建TLP Header，并在其后附上数据荷载（如果有），如果需要启用并支持可选项的话也可以再附上ECRC（End-to-End CRC）。随后TLP就会被放入一个虚拟通道Buffer。这个虚拟通道会根据事务排序规则来管理TLP的顺序，并且也要在TLP被向下转发到数据链路层之前，确认接收方有足够的Buffer来接收一个TLP。

\3.    当TLP到达数据链路层，它会被分配一个序列号（Sequence Number），并基于TLP的内容和序列号来计算出一个LCRC（Link CRC）来附加在原TLP后。然后会将经过这些处理过程之后的TLP保存一个副本，这个副本会保存在数据链路层的重传Buffer（Replay Buffer，也可称为Retry Buffer）中，这是为了应对传输出错的情况。与此同时，这个TLP也会被向下转发至物理层。

\4.    物理层将会进行一系列的操作来准备对这个数据包进行串行传输，包括字节条带化（Byte Striping）、扰码（Scrambling）、编码（Encoding）以及并串转换（Serializing）。对于Gen1和Gen2的设备，当进行8b/10b编码时，会将STP和END这两个控制字符分别加在TLP的首端和尾端。最后，这个数据包通过链路进行传输。在Gen3操作模式中，STP令牌（STP token）会被添加在TLP的首端，但是并不会在尾端加上END，而是在STP令牌中包含TLP大小的信息来判断TLP的尾部位置。

 

n 接收方

\5.    在接收方（本例中是Device B），为了发送包所做的一切准备现在都必须撤销。物理层将对比特流进行串并转换（deserialize）、对串并转换后的符号进行解码，然后再进行字节反条带化（un-stripes）。控制字符将被移除，因为它们仅在物理层有意义，然后这个数据包就会被向上转发至数据链路层。

\6.    数据链路层将会计算CRC（具体一点是LCRC）并与TLP中的CRC进行比较。如果CRC比较结果相同，那么就再检查序列号（Sequence Number）。如果都没有出现错误，那就把CRC和序列号都从TLP剥除，并随后将TLP向上转发给接收方事务层，与此同时要通过返回给发送方一个Ack DLLP来通知发送方这个TLP被成功接收。相反地，如果前面的过程中检查出了错误，那么就要返回给发送方一个Nak DLLP，这样发送方将会使用它的重传Buffer来对TLP进行重传。

\7.    在事务层，TLP被进行译码，并将TLP内的信息传递给Device Core来进行相应的操作。如果当前接收设备就是数据包的最终目的地，那么它可以检查ECRC错误，并在发现任何ECRC错误时报告给Device Core。

#### 5.2.2     TLP结构

一个事务层包TLP中各个字段域的基本用法在表 5‑1中进行了定义。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image214.jpg)

表 5‑1 TLP Header的Type字段定义了事务不同种类

下面对表 5‑1中的内容进行复述。

n Header

n 协议层次：事务层

n 该组件用法：大小为3或4DW（12或16Bytes）。Header的格式会随类型而变化，但是Header也定义了一些参数，包括：

o 事务类型（Transaction Type）

o 目标地址、ID等

o 传输数据量大小（如果有数据）、字节使能（Byte Enable）

o 属性（Attribute）

o 流量类型（Traffic Class）

n Data

n 协议层次：事务层

n 该组件用法：可选的1-1024DW大小的数据荷载，具体的大小由字节使能或者字节对齐的开始和结束地址来进行描述。需要注意指定的长度不能为0，但是一个0长度的读取（在某些情况下会使用）可以通过指定长度为1DW然后将字节使能全部置为0来进行近似处理。来自Completer的这样子的数据虽然是未定义的种类，但是Requester并不使用它，因此就和指定0长度的目的等效了。

n Digest/ECRC

n 协议层次：事务层

n 该组件用法：可选的功能。当需要使用时，ECRC的大小永远为1DW。

#### 5.2.3     通用TLP Header格式（Generic TLP Header Format）

##### 5.2.3.1   整体说明（General）

如图 5‑3中，展示了一个4DW的通用TLP Header的格式和内容。在本节内，会对几乎所有事务的TLP Header中的公共字段进行总结，并会在稍后讨论与特定事务类型相关Header格式差异。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image216.jpg)

图 5‑3通用TLP Header的各字段域

##### 5.2.3.2   通用Header各字段总结

表 5‑2中对通用TLP Header中的每个字段的大小和用途进行了总结。需要注意的是，在图 5‑3中被标注为“R”的字段是保留字段（reserve），应该被置为0。



| Header中的字段名                                             | Header中的位置                          | 字段作用                                                     |
| ------------------------------------------------------------ | --------------------------------------- | ------------------------------------------------------------ |
| Fmt[2:0]  格式  (Format)                                     | Byte  0  Bit  7:5                       | 这几个bit的编码信息是关于Header的大小，以及这个TLP中是否存在数据荷载部分：  n 000b：3DW Header，无数据荷载  n 001b：4DW Header，无数据荷载  n 010b：3DW Header，有数据荷载  n 011b：4DW Header，有数据荷载  n 100b：1DW，属于Prefix TLP  不难发现，除了Prefix TLP外，只要Fmt[0]=0那么就是3DW Header，反之则为4DW Header。若Fmt[1]=0那么就无数据荷载，反之则有数据荷载。  对于低于4GB的地址，必须使用3DW Header。协议规定，如果使用4DW Header但是地址小于4GB，也就是说将64bit地址的高32bit置为0，那么这种情况下接收方的行为是未进行定义的（undefined）。 |
| Type[4:0]  类型                                              | Byte  0  Bit  4:0                       | 这些bit编码的信息是TLP的不同事务类型。Type字段用来跟Fmt[1:0]字段一起指定了事务类型、Header大小、以及是否存在数据荷载。更多详细信息请参阅“Generic Header  Field Details”一节。 |
| TC[2:0]  流量类型  (Traffic  Class)                          | Byte  1  Bit  6:4                       | 这些bit表示将应用于这个TLP以及其完成包（如果需要完成包）的流量类型：  n 000b：Traffic Class  0（默认）  n 001b：Traffic Class  1  ……  n 111b：Traffic Class  7  TC 0是默认类型，而TC 1-7是用来提供差异化的服务。更多信息请参阅“Traffic Class”一节。 |
| Attr[2]  属性  (Attribute)                                   | Byte  1  Bit  2                         | 这个第三位的Attribute位（它共有3位）用于表示这个TLP是否使用基于ID的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering”一节。 |
| TH  TLP处理提示  (TLP  Processing Hints)                     | Byte  1  Bit  0                         | 它用于表示何时TLP中会包含TLP提示（TLP Hints），以便让系统了解如何更好的处理这个TLP。更多内容请参阅“TPH（TLP Processing Hints）”。 |
| TD  TLP摘要  (TLP  Digest)                                   | Byte  2  Bit  7                         | 如果TD=1，那么这个TLP中将包括可选的4Byte TLP Digest字段，也就是ECRC值。  它有一些规则：  n 所有的接收者都必须要通过这个bit来检查是否存在Digest字段。  n 如果一个TLP的TD=1，但是它又没有Digest，那么它将被当做畸形TLP（Malformed TLP）处理。  n 如果接收设备支持ECRC校验，且此TLP内TD=1，那么这个接收设备必须进行校验。  n 如果一个作为TLP最终目的地的设备并不支持ECRC校验（因为这是可选功能），那么它必须要忽略Digest字段。  更多内容请参阅“CRC”和“ECRC Generating and Checking”这两节。 |
| EP  受污染的数据  (Poisoned Data)                            | Byte 2  Bit 6                           | 如果EP=1，那么就认为所有伴随此数据的数据都是无效的，尽管相关事务依然允许正常的完成。关于Poisoned数据包的更多内容，请参阅“Data Poisoning”一节。 |
| Attr[1:0]  属性  (Attributes)                                | Byte  2  Bit  5:4                       | Bit 5 = Relax Ordering（宽松排序）：当它被置为1时，这个TLP会启用PCI-X的宽松排序。如果它为0，则是用PCI的严格的PCI排序（strict PCI ordering）。  Bit 4 = No Snoop（无窥探）：当置为1时，Requester的意思是这个TLP不会存在host cache一致性的问题（host cache coherency issues），因此系统硬件可以通过跳过普通处理器对这个请求的cache窥探，以此来节省时间。而当这一位被置为0时，需要进行PCI-type的cache窥探保护。 |
| AT[1:0]  地址类型  (Address  Type)                           | Byte  2  Bit  3:2                       | 对于Memory和Atomic请求来说，这个字段用于支持虚拟化系统（virtualized system）的地址翻译。这个地址翻译协议由一个单独的规范进行描述，被称为Address  Translation Services，可以看到该字段的编码为：  n 00 = 默认/未翻译（Default/Untranslated）  n 01 = 翻译请求（Translation Request）  n 10 = 已翻译（Translated）  n 11 = 保留reserve |
| Length[9:0]  长度                                            | Byte  2  Bit  1:0     Byte  3  Bit  7:0 | TLP数据荷载传输量的大小，单位为DW，编码方式为：  n 00 0000 0001b = 1DW  n 00 0000 0010b = 2DW  ……  n 11 1111 1111b = 1023DW  n 00 0000 0000 = 1024DW |
| Last  DW BE[3:0]  末尾DW的字节使能  (Last  DW Byte Enable)   | Byte  7  Bit  7:4                       | 这个字段中的4个高有效的bit，与数据荷载中最后一个DW中的4个Byte一一对应。  n Bit 7 = 1：末尾DW的Byte 3是有效的；否则为无效的。  n Bit 6 = 1：末尾DW的Byte 2是有效的；否则为无效的。  n Bit 5 = 1：末尾DW的Byte 1是有效的；否则为无效的。  n Bit 4 = 1：末尾DW的Byte 0是有效的；否则为无效的。 |
| 1st  DW BE[3:0]  第一个DW的字节使能  (First  DW Byte Enable) | Byte  7  Bit  3:0                       | 这个字段中的4个高有效的bit，与数据荷载中第一个DW中的4个Byte一一对应。  n Bit 3 = 1：末尾DW的Byte 3是有效的；否则为无效的。  n Bit 2 = 1：末尾DW的Byte 2是有效的；否则为无效的。  n Bit 1 = 1：末尾DW的Byte 1是有效的；否则为无效的。  n Bit 0 = 1：末尾DW的Byte 0是有效的；否则为无效的。 |

表 5‑2通用TLP Header的各字段总结

#### 5.2.4     通用TLP Header详细说明（Generic TLP Header Details）

在接下来的几个小节中，我们将对图 5‑3中展示的TLP Header的每一个字段都进行细节描述。

##### 5.2.4.1   Header的Type/Format字段编码含义

表 5‑3中总结了TLP Header中Type字段和Fmt字段（Format）的编码含义。

| TLP事务种类                                                  | Fmt[2:0]                                | Type[4:0]                |
| ------------------------------------------------------------ | --------------------------------------- | ------------------------ |
| Memory  Read请求  （MRd）                                    | 000=3DW，无数据  001=4DW，无数据        | 0 0000                   |
| Memory  Read Locked请求  （MRdLk）                           | 000=3DW，无数据  001=4DW，无数据        | 0  0001                  |
| Memory  Write请求  （MWr）                                   | 010=3DW，有数据  011=4DW，有数据        | 0  0000                  |
| IO  Read请求  （IORd）                                       | 000=3DW，无数据  （IO请求为3DW Header） | 0  0010                  |
| IO  Write请求  （IOWr）                                      | 010=3DW，有数据  （IO请求为3DW Header） | 0  0010                  |
| 配置Type 0 Read请求  （CfgRd0）                              | 000=3DW，无数据                         | 0  0100                  |
| 配置Type 0 Write请求  （CfgWr0）                             | 010=3DW，有数据                         | 0  0100                  |
| 配置Type 1 Read请求  （CfgRd1）                              | 000=3DW，无数据                         | 0  0101                  |
| 配置Type 1 Write请求  （CfgWr1）                             | 010=3DW，有数据                         | 0  0101                  |
| Message请求  （Msg）                                         | 001=4DW，无数据                         | 1  0 rrr*  （见表 4‑10） |
| Message带数据的请求  （MsgD）                                | 011=4DW，有数据                         | 1  0 rrr*  （见表 4‑10） |
| 完成包Completion  （Cpl）                                    | 000=3DW，无数据                         | 0  1010                  |
| 完成包带数据  （CplD）                                       | 010=3DW，有数据                         | 0  1010                  |
| 锁定的完成包  （CplLk）                                      | 000=3DW，无数据                         | 0  1011                  |
| 锁定的完成包带数据  （CplDLk）                               | 010=3DW，有数据                         | 0  1011                  |
| 获取和增加原子操作请求  （Fetch and Add AtomicOP Request）   | 010=3DW，有数据  011=4DW，有数据        | 0  1100                  |
| 无条件的交换原子操作请求  （Unconditional Swap AtomicOP Request） | 010=3DW，有数据  011=4DW，有数据        | 0  1101                  |
| 对比和交换原子操作请求  （Compare and Swap AtomicOP Request） | 010=3DW，有数据  011=4DW，有数据        | 0  1110                  |
| 本地TLP前缀  （Local TLP Prefix）                            | 100=TLP  Prefix                         | 0L3L2L1L0                |
| 端到端TLP前缀  （End-to-End TLP Prefix）                     | 100=TLP  Prefix                         | 1E3E2E1E0                |

表 5‑3 TLP Header的Type字段和Fmt字段编码含义

##### 5.2.4.2   Digest/ECRC字段

TLP的Digest位表示是否存在ECRC（End-to-End CRC）。如果当前支持这个可选的特性，并且软件也启用了它，那么设备将会给自己产生的所有TLP都计算并且添加上一个ECRC。需要注意，使用ECRC要求设备含有可选的Advanced Error Reporting寄存器，这是因为它的能力（Capability）和控制寄存器就位于Advanced Error Reporting寄存器中。

###### l ECRC的生成和检查

ECRC覆盖了所有在跨Fabric转发时不需要更改的字段。然而，当一个数据包在拓扑结构中移动时，有两个bit是可以合法的进行改变的：

o Type字段的Bit 0：在一个配置事务通过一个Bridge时，这个配置事务有可能从Type 1变成Type 0，因为这个Bridge的次级总线有可能就是配置事务的目标总线。在这个情况下，Type地段的bit 0的值就会从1变成0。

o EP（Error/Poisoned）bit：当一个TLP在网络结构中移动时，如果与这个数据包相关的数据被认为是损坏的，那么就有可能引发这个bit值的改变。这是一种可选的特性，称为错误转发（error forwarding）。

###### l 谁来检查ECRC

ECRC的预定目标是TLP的最终接收者。对LCRC（Link CRC）的校验是用来验证在当前给定链路上的传输没有出错，但是会在Switch/Bridge的出口端口（egress port）重新计算一个LCRC，然后再转发给下一级链路，也就是说同一个LCRC的作用范围不会跨Switch/Bridge，那么这将会使得Switch/Bridge这些路由元件内部发生的错误被掩盖。为了解决这个问题，就在TLP在Requester到Completer的整个传输过程中都加上一个不会改变的ECRC（也就是这个过程中ECRC都不会被其他设备重新计算和替换）。当目标设备检查ECRC时，任何在整个传输过程中发生的错误都会极容易被发现。

关于ECRC校验中Switch的作用，PCIe协议做了两个声明：

o 如果一个Switch支持ECRC校验，那么它需要对TLP目的地为Switch自身时，对这个TLP中的ECRC进行校验。如果TLP的目的地并不是Switch自身，那么Switch必须在保持ECRC不变的前提下对其进行转发，把ECRC作为TLP不可分割的一部分。“On all other TLPs a Switch must preserve the ECRC (forward it untouched) as an integral part of the TLP.”

o 注意，Switch也可以对通过它的TLP进行ECRC检查。由Switch发现的ECRC错误的报告方法和其他设备一样，但是这不会改变TLP通过Switch的继续传输。“Note that a Switch may perform ECRC checking on TLPs passing through the Switch. ECRC Errors detected by the Switch are reported in the same way any other device would report them, but do not alter the TLPs passage through the Switch.”

##### 5.2.4.3   使用字节使能（Using Byte Enables）

###### l 整体说明

类似PCI一样，对于有时传输大小或者起始/结束地址不是DW对齐时，PCIe也需要一种机制来根据需要协调其DW对齐地址。为了达到这个目的，PCIe利用了2个字节使能（Byte Enable）字段，如图 5‑3中和表 5‑2中所介绍，分别是首DW字节使能（First DW Byte Enable）和尾DW字节使能（Last DW Byte Enable），它们使得Requester可以对第一个DW和最后一个DW真正有效传输的字节进行限定。

###### l 字节使能规则

\1.    字节使能地各个bit是高有效的。如果某个字节使能bit值为0，那么就表示数据荷载中相应的字节不应该被Completer使用，因为这个字节是无效的。若某个字节使能bit值为1，那么就说明相应的字节应该被Completer使用。

\2.    如果有效的数据全都在一个单独的DW中，那么尾DW字节使能（Last DW Byte Enable）必须为0000b。

\3.    如果Header中的Length字段表示了这次传输数据量大于1DW，那么首DW字节使能（First DW Byte Enable）至少要有1个bit是有效的。

\4.    如果Header中的Length字段表示了这次传输数据量大于等于3DW，那么首DW字节使能和尾DW字节使能都必须是相连的bit为1。在这种情况中，字节使能仅能用来提供有效起始地址和结束地址与DW对齐地址的字节偏移量。

\5.    如果传输数据量大小为1DW，那么才能允许只有首DW字节使能内是不连续为1的情况。

\6.    如果传输数据量大小为1-2DW，那么才能允许首DW字节使能和第二DW字节使能内都是不连续为1的情况。

\7.    如果一个写请求，其Length字段表示其数据传输长度为1DW，但是它的字节使能均为无效，这种情况对Completer不会有影响。

\8.    如果一个读请求，其Length字段表示其需要的数据传输长度为1DW，但是没有字节使能有效，那么Completer会返回一个1DW数据荷载，这种数据荷载是未定义数据（undefined data）。这种情况可能被用作一种刷新机制（Flush mechanism），它利用事务排序规则，在这个undefine data的读请求完成包返回之前，强制之前发出的所有posted write输出到内存中。

###### l 字节使能示例

如图 5‑4展示了字节使能字段是如何使用的。注意，数据传输长度必须从第一个DW中的任何有效字节延伸至最后一个DW中的任何有效字节。因为数据传输量大于2DW，字节使能就只能用来标识这次传输中起始地址的位置（2d）和结束地址的位置（34d）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image218.jpg)

图 5‑4首DW字节使能和尾DW字节使能字段的使用

##### 5.2.4.4   事务描述符字段（Transaction Descriptor Fields）

当事务在Requester和Completer之间移动时，必须要能够对各个事务进行唯一的标识，因为在任何时刻的Requester中都有可能有许多的拆分事务在排队。为了更好的进行标识，PCIe协议在TLP Header中定义了多个重要字段，来组成一个唯一的事务描述符（Transaction Descriptor），如图 5‑5所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image220.jpg)

图 5‑5事务描述符字段

虽然事务描述符在TLP Header中并不是位置全连续的字段，但是这些不连续字段合起来可以一起描述事务的关键属性，包括：

###### l 事务ID（Transaction ID）

事务ID由Requester ID（Requester的BDF）和TLP的Tag字段共同组成。

###### l 流量类型（Traffic Class）

流量类型（Traffic Class，TC）是由Requester的Device Core来添加进TLP的，并且在拓扑结构中从Requester传输到Completer的过程中TC都不发生改变。在拓扑结构的每个链路中，TC都映射到其中一个虚拟通道（Virtual Channel）。

###### l 事务属性（Transaction Attributes）

用于表示基于ID的排序（ID-based Ordering）、宽松排序（Relaxed Ordering）以及无窥探（No Snoop）的bit也存在于请求包中，被发送给Completer。

##### 5.2.4.5   带有数据荷载的TLP的一些额外规则

当一个TLP带有数据荷载时，就要遵从如下规则：

\1.    Length字段仅表示数据荷载的长度。

\2.    数据荷载的首字节（紧挨着Header的字节）的地址永远是最低位的。

\3.    Length字段永远表示的是整数DW的传输长度。对于不满整数DW的情况是使用首DW字节使能和尾DW字节使能来进行表示的。

\4.    在协议中有声明，当一个Completer在响应一个单独的Memory请求时如果返回了多个TLP，那么中间的（也就是非首尾的）TLP的数据荷载的地址必须结束于RC的64byte或128byte自然对齐的边界上。具体是那种边界是由一个可配置位称为RCB（读完成边界，Read Completion Boundary）来控制的。所有的其他设备遵循PCI-X协议，并会将这种情况的TLP截断在128byte自然对齐边界上。

\5.    当发送Message请求时，Length字段是保留字段，除非Message是带有数据荷载的，也就是MsgD。

\6.    TLP的数据荷载一定不能大于设备控制寄存器（Device Control Register）中的最大数据荷载字段（Max_Payload_Size）。因为只有写事务会带有数据荷载，因此这种限制并不应用于读请求。接收方需要在被写入时检查是否存在Max_Payload_Size的违例，如果存在则认为这是一个畸形TLP（Malformed TLP）。

\7.    接收方还需要检查Length字段和TLP实际传输的数据荷载长度。如果二者不相同则认为这是一个畸形TLP（Malformed TLP）。

\8.    请求包中的起始地址和传输长度的组合信息，一定不能造成跨4KB边界的Memory访问。虽然这种检查是可选的，但是一旦发现了跨4KB边界访问的情况就认为这是一个畸形TLP（Malformed TLP）。

#### 5.2.5     具体的TLP格式：请求TLP和完成TLP

在本节中，将会描述用来构成具体的一些事物类型的TLP 3DW Header和4DW Header。许多通用的字段就如前文所述，因此我们把重点放在那些需要根据具体事务类型进行具体处理的字段上。接下来的几个小节是关于TLP Header的详细描述，相关的TLP类型为：1）IO Request，2）Memory Requests，3）Configuration Requests，4）Completions，以及5）Message Request。

##### 5.2.5.1   IO请求（IO Request）

虽然协议中不鼓励使用IO事务，但是对于传统遗留设备还是允许的，并且对于软件来说需要通过IO事务来兼容那些存在系统IO映射而不是内存映射的设备。虽然IO事务从技术上讲是可以访问32bit IO范围的，但是实际上许多系统（和CPU）都会将IO访问限制在低16bit（64KB）范围内。图 5‑6展示了系统IO映射以及16bit和32bit地址边界。自身不是传统遗留设备的设备是不允许访问BAR中的IO地址空间的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image222.jpg)

图 5‑6 系统IO映射

###### l IO Request Header的格式

如图 5‑7所示是一个3DW IO Request Header，其中的每个字段都会在接下来的内容中进行描述。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image224.jpg)

图 5‑7 3DW IO Request Header格式

 

###### l IO Request Header各字段

一个IO Request Header中的每个字段的位置和作用都在表 5‑4中进行了描述。

| 字段名称                                  | 位于Header中的Byte/Bit                                       | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                | Byte  0 Bit 7:5                                              | IO请求的包格式字段：  000b=IO  Read（3DW，无数据）  010b=IO  Write（3DW，有数据） |
| Type[4:0]  类型                           | Byte  0 Bit 4:0                                              | 0 0010b=IO请求                                               |
| TC[2:0]  流量类型  （Traffic Class）      | Byte  1 Bit 6:4                                              | IO请求的TC永远为0，确保这些包永远不会干扰高优先级的包。      |
| Attr[2]  属性  （Attribute）              | Byte  1 Bit 2                                                | 由于IO请求不使用基于ID的排序（ID-based Ordering），因此这个字段为保留字段。 |
| TH  TLP处理提示  （TLP Processing Hints） | Byte  1 Bit 0                                                | 由于IO请求不使用TLP处理提示（TLP Processing Hints），因此这个字段为保留字段。 |
| TD  TLP摘要  （TLP Digest）               | Byte  2 Bit 7                                                | 表示TLP的末尾是否存在Digest字段，也就是ECRC。                |
| EP  受污染的数据  （Poisoned Data）       | Byte  2 Bit 6                                                | 表示TLP的数据荷载（如果有）是否被污染（poisoned）。          |
| Attr[1:0]  属性  （Attribute）            | Byte  2 Bit 5:4                                              | 由于IO请求不使用宽松排序（Relaxed  Ordering）和无窥探（No Snoop），因此这个字段为0。 |
| AT[1:0]  地址类型  （Address Type）       | Byte  2 Bit 3:2                                              | 由于IO请求不使用地址类型字段，因此这个字段必须为0。          |
| Length[9:0]  长度                         | Byte  2 Bit 1:0  Byte  3 Bit 7:0                             | 该字段用来表示以DW为单位的数据荷载大小。对于IO请求来说，这个字段为1，因为IO请求不能传输超过4Byte。由首DW字节使能来限定使用哪个Byte。 |
| Requester  ID[15:0]  Requester  ID        | Byte  4 Bit 7:0  Byte  5 Bit 7:0                             | 用于表示相应完成包的“返回地址”。  Byte 4,7:0=Bus  Number  Byte 5,7:3=Device  Number  Byte 5,2:0=Function  Number |
| Tag[7:0]  Tag                             | Byte  6 Bit 7:0                                              | 这个字段用来标识来自Requester的具体每个请求。每个被发出的请求包都会被分配一个独一无二的Tag值。默认情况下，只使用Tag[4:0]，但是扩展Tag（Extended Tag）和Phantom Function可选项可以将Tag的使用扩展为Tag[10:0]，这样就允许多达2048个未完成的请求同时进行。 |
| Last  DW BE[3:0]  尾DW字节使能            | Byte  7 Bit 7:4                                              | 因为IO请求数据大小只能是1DW以内，所以这个字段必须为0000b。   |
| 1st  DW BE[3:0]  首DW字节使能             | Byte  7 Bit 3:0                                              | 这个字段用于限定第一个DW数据内的有效字节。对于IO请求来说，所有的bit组合都是允许的（包括0000b）。 |
| Address[31:2]  地址                       | Byte  8 Bit 7:0  Byte  9 Bit 7:0  Byte  10 Bit 7:0  Byte  11 Bit 7:2 | 该字段是32bit起始地址的高30bit。而最低2bit是保留位（00b），这使得32bit起始地址一定是DW对齐的。 |

表 5‑4 IO Request Header各字段

##### 5.2.5.2   Memory请求（Memory Request）

PCIe的Memory事务包含两种类型：第一种是读请求及其相对应的完成包，第二种是写请求。系统内存映射如所示，展示了3DW与4DW的Memory请求包。需要记住协议规范中多次重申的一点，Memory传输绝对不允许跨4KB地址边界。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image226.jpg)

图 5‑8 3DW和4DW的Memory Request Header格式

###### l Memory Request Header各字段

一个4DW的Memory Request Header中的每个字段的位置和作用都在表 5‑5中进行了描述。注意，3DW和4DW Header的区别仅仅是起始地址字段的位置和字段大小不同。

| 字段名称                                  | 位于Header中的Byte/Bit                                       | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                | Byte  0 Bit 7:5                                              | IO请求的包格式字段：  000b=Memory  Read（3DW无数据）  010b=Memory  Write（3DW有数据）  001b=Memory  Read（4DW无数据）  011b=Memory  Write（4DW有数据）  1xxb=TLP前缀已经被添加到包的开头。更多内容请参阅“TPH（TLP Processing Hints）”一节 |
| Type[4:0]  类型                           | Byte  0 Bit 4:0                                              | 0 0000b=  Memory Read或Write  0 0001b=  Memory Read Locked  Type字段与Fmt[1:0]共同指定事务类型、Header大小以及是否有数据荷载。 |
| TC[2:0]  流量类型  （Traffic Class）      | Byte  1 Bit 6:4                                              | 这个字段的编码信息是请求包及其完成包的流量类型。  000b=TC  0（默认）  ……  111b=TC  7  更多内容请参阅“Traffic Class（TC）”一节。 |
| Attr[2]  属性  （Attribute）              | Byte  1 Bit 2                                                | 表示这个TLP是否使用基于ID的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering（IDO）”一节。 |
| TH  TLP处理提示  （TLP Processing Hints） | Byte  1 Bit 0                                                | 指示是否包含TLP提示信息。更多内容请参阅“TPH（TLP Processing  Hints）”一节。 |
| TD  TLP摘要  （TLP Digest）               | Byte  2 Bit 7                                                | 如果为1，则TLP中存在TLP Digest字段，即存在ECRC。  一些规则：  所有的接收者都要使用这个TD bit检查是否存在Digest字段。  l TD=1但是不含有Digest字段的TLP被当做畸形（Malformed）  l 如果TD=1，且接收者也启用了ECRC检查，那么就必须对ECRC进行检查。  l 如果接收者并不支持可选的ECRC校验，那么它必须要忽略TLP中的Digest字段。 |
| EP  受污染的数据  （Poisoned Data）       | Byte  2 Bit 6                                                | 如果为1，那么伴随这个TLP的数据荷载就都被认为是有错误的，尽管这个事务还是可以照常完成。 |
| Attr[1:0]  属性  （Attribute）            | Byte  2 Bit 5:4                                              | l Bit 5 =宽松排序（Relaxed Ordering）  当Bit 5=1，这个TLP就使用PCI-X的宽松排序。否则，使用严格的PCI排序。  l Bit 6=无窥探（No Snoop）  当Bit 6=1，系统软件就不需要进行关于这个TLP的处理器Cache一致性窥探。 |
| AT[1:0]  地址类型  （Address Type）       | Byte  2 Bit 3:2                                              | 这个字段支持虚拟系统（virtualized systems）的地址翻译。地址翻译协议是由一个单独的协议规范来描述的，称为地址翻译服务（Address Translation Services），AT中的编码信息为：  00=默认/未翻译  01=翻译请求  10=已翻译  11=保留位 |
| Length[9:0]  长度                         | Byte  2 Bit 1:0  Byte  3 Bit 7:0                             | TLP数据荷载的长度，单位为DW。最大为1024DW（4KB），编码如下：  00  0000 0001b=1DW  00  0000 0010b=2DW  ……  11  1111 1111b=1023DW  00  0000 0000b=1024DW |
| Requester  ID[15:0]  Requester  ID        | Byte  4 Bit 7:0  Byte  5 Bit 7:0                             | 用于表示这个请求的相应完成包的“返回地址”。  Byte 4,7:0=Bus  Number  Byte 5,7:3=Device  Number  Byte 5,2:0=Function  Number |
| Tag[7:0]  Tag                             | Byte  6 Bit 7:0                                              | 这个字段用来标识来自Requester的具体每个请求。每个被发出的请求包都会被分配一个独一无二的Tag值。默认情况下，只使用Tag[4:0]，如果Control Register中的Extended Tag位被置为1，那么可以将Tag的使用扩展为Tag[7:0]，这样就允许最多使用256个Tag。 |
| Last  DW BE[3:0]  尾DW字节使能            | Byte  7 Bit 7:4                                              | 这个字段用于限定最后一个DW数据内的有效字节                   |
| 1st  DW BE[3:0]  首DW字节使能             | Byte  7 Bit 3:0                                              | 这个字段用于限定第一个DW数据内的有效字节。                   |
| Address[63:32]  高32bit地址               | Byte  8 Bit 7:0  Byte  9 Bit 7:0  Byte  10 Bit 7:0  Byte  11 Bit 7:2 | 该字段是Memory传输的64bit起始地址的高32bit。                 |
| Address[31:2]  低32bit地址                | Byte  12 Bit 7:0  Byte  13 Bit 7:0  Byte  14 Bit 7:0  Byte  15 Bit 7:2 | 该字段是Memory传输的64bit起始地址的低32bit。而最低2bit是保留位始终为0，这样就强制起始地址是DW对齐的。 |

表 5‑5 4DW Memory Request Header各字段

###### l Memory Request 注意事项

Memory请求的特性包括：

\1.    Memory数据传输不允许跨4KB边界。

\2.    所有的内存映射写（Memory-Mapped Write）都是Posted的，以此来提升性能。

\3.    使用32bit或者64bit地址方式。

\4.    数据荷载大小在0-1024DW之间（0-4KB）。

\5.    QoS（Quality of Service）特性可以被使用，最多可以有8个流量类型。

\6.    No Snoop属性可以在事务访问主存时，用来减轻系统对窥探处理器Cache的需求。

\7.    Relaxed Ordering属性可以使得数据包传输路径上的设备对这个TLP使用宽松排序规则，以此来提升性能。

##### 5.2.5.3   Configuration请求（Configuration Request）

PCIe像PCI一样使用Type 0和Type 1配置请求，以此来保持向后兼容性。一个Type 1配置请求会向下行传播，直到一个Bridge的次级总线（Secondary Bus）正好是这个Type 1配置请求的目标总线，在这时这个Bridge就会把这个配置事务从Type 1转换为Type 0。Bridge是通过此前编程写入的总线号寄存器来得知何时应该对配置事务进行Type转换以及继续转发，其中总线号寄存器包括主总线号（Primary Bus Number）、次级总线号（Secondary Bus Number）以及从属总线号（Subordinate Bus Number）。关于这里的更多内容，请参阅“传统PCI机制（Legacy PCI Mechanism）”一节。

如图 5‑9中，一个Type 1配置请求正在按照规则向下行移动，它在Switch左侧的下行端口的Bridge被转换成Type 0，这种转换是通过改变TLP Header中的Type字段Bit 0来完成的（Type从0 0101b变为0 0100b）。注意，不同于PCI，PCIe中下行的一条链路上只能由一个设备，因此链路上不需要有IDSEL或者其他指示信号来告诉设备需要声明自己占有Type 0配置请求，也就是说设备只要从上行收到了Type 0配置请求，就知道这个配置请求的目标设备就必然是自己。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image228.jpg)

图 5‑9 3DW配置请求以及Header格式

###### l Configuration Request Header各字段的定义

图 5‑9中展示的Configuration Request Header中的每个字段的位置和作用都在表 5‑6中进行了描述。

| 字段名称                                                     | 位于Header中的Byte/Bit           | 功能                                                         |
| ------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                                   | Byte  0 Bit 7:5                  | 配置请求始终为3DW Header：  000b=Configuration  Read（无数据）  010b=  Configuration Write（有数据） |
| Type[4:0]  类型                                              | Byte  0 Bit 4:0                  | 0 0100b=  Type 0配置请求  0 0101b=  Type 1配置请求           |
| TC[2:0]  流量类型  （Traffic Class）                         | Byte  1 Bit 6:4                  | 配置请求的TC必须为0，以确保这些配置包永远不会影响到高优先级的包。 |
| Attr[2]  属性  （Attribute）                                 | Byte  1 Bit 2                    | 配置请求的这个字段为保留字段，必须为0。                      |
| TH  TLP处理提示  （TLP Processing Hints）                    | Byte  1 Bit 0                    | 配置请求的这个字段为保留字段，必须为0。                      |
| TD  TLP摘要  （TLP Digest）                                  | Byte  2 Bit 7                    | 用于指示TLP的末尾是否有1DW的Digest字段。                     |
| EP  受污染的数据  （Poisoned Data）                          | Byte  2 Bit 6                    | 用于指示数据荷载是被污染的。                                 |
| Attr[1:0]  属性  （Attribute）                               | Byte  2 Bit 5:4                  | 配置请求的Relaxed Ordering和No Snoop字段都为0。              |
| AT[1:0]  地址类型  （Address Type）                          | Byte  2 Bit 3:2                  | 配置请求的Address Type字段为保留字段，必须为0。              |
| Length[9:0]  长度                                            | Byte  2 Bit 1:0  Byte  3 Bit 7:0 | 配置请求的数据荷载大小字段永远为1，4bit字节使能的所有组合都是允许的。 |
| Requester  ID[15:0]  Requester  ID                           | Byte  4 Bit 7:0  Byte  5 Bit 7:0 | 用于表示这个请求的相应完成包的“返回地址”。  Byte 4,7:0=Bus  Number  Byte 5,7:3=Device  Number  Byte 5,2:0=Function  Number |
| Tag[7:0]  Tag                                                | Byte  6 Bit 7:0                  | 这个字段用来标识来自Requester的具体每个请求。每个被发出的请求包都会被分配一个独一无二的Tag值。默认情况下，只使用Tag[4:0]（也就是同时可以有32个未完成的事务在进行），如果Control Register中的Extended Tag位被置为1，那么可以将Tag的使用扩展为Tag[7:0]，这样就允许最多使用256个Tag。 |
| Last  DW BE[3:0]  尾DW字节使能                               | Byte  7 Bit 7:4                  | 这个字段用于限定最后一个DW数据内的有效字节。由于配置请求的数据荷载只能为1DW，因此这个字段必须为0 |
| 1st  DW BE[3:0]  首DW字节使能                                | Byte  7 Bit 3:0                  | 这个字段用于限定第一个DW数据内的有效字节。对于配置请求来说，这个字段的所有组合都是允许的（包括全0）。 |
| Completer  ID[15:0]  Completer  ID                           | Byte  8 Bit 7:0  Byte  9 Bit 7:0 | 用于指示这个配置请求要访问的Completer。  Byte 8,7:0=Bus Number  Byte 9,7:3=Device Number  Byte 9,2:0=Function Number |
| Ext  Register Number[3:0]  扩展寄存器号  （Extended Register Number） | Byte  10 Bit 3:0                 | 这个字段提供了在访问扩展配置空间（Extended Config Space）时DW空间的高4bit，它与Register Number共同组合成10bit地址，用于访问1024DW（也就是4096Byte）的空间。对于PCI-Compatible配置空间，这个字段必须为0。 |
| Register  Number[3:0]  寄存器号  （Register Number）         | Byte  11 Bit 7:0                 | 用于指示寄存器号，是配置DW空间的低8bit。最低的2bit始终为0，以强制进行DW对齐的访问。 |

表 5‑6 配置请求Header各字段

###### l Configuration Request注意事项

配置请求的Header总为3DW格式，路由信息为Bus Number、Device Number和Function Number。所有的设备都会在接收到一个Type 0配置写请求时从请求包中捕获出它们自己的Bus Number和Device Number。之所以这样，是因为这些设备之后会用自己捕获到的Type 0配置写中的Bus Number和Device Number来作为Requester ID，用来在以后发起请求时使用。

##### 5.2.5.4   完成包（Completion）

Completion（完成包）是用来响应Non-Posted请求的，除非有错误阻止了发送完成包。例如，Memory、IO、Configuration Read请求通常就是由带有数据的完成包来响应。另一方面，IO或者Configuration写请求通常也由无数据的完成包来响应，这种情况下的完成包仅仅是用来报告事务的状态。

在完成包中的许多字段的值，都与它相关的请求包中的字段相同，包括TC（Traffic Class，流量类型）、Attr（Attribute，属性）以及Requester ID（请求者ID，用来将完成包路由回到请求者去）。图 5‑10中展示了一个响应Non-Posted请求的完成包，并且还展示了这个完成包的3DW Header。在正常操作中，Completer ID并没有什么额外的用处，但若是在系统调试期间能够知道完成包从哪里来，那么这对错误的诊断就有较大的帮助。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image230.jpg)

图 5‑10 3DW完成包Header格式

###### l Completion Header各字段的定义

图 5‑10中展示的Completion Header中的每个字段的位置和作用都在表 5‑7中进行了描述。

| 字段名称                                                    | 位于Header中的Byte/Bit           | 功能                                                         |
| ----------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                                  | Byte  0 Bit 7:5                  | 完成包始终为3DW Header：  000b=Cpl，无数据  010b=CplD，有数据 |
| Type[4:0]  类型                                             | Byte  0 Bit 4:0                  | 0 1010b=完成包                                               |
| TC[2:0]  流量类型  （Traffic Class）                        | Byte  1 Bit 6:4                  | 完成包的该字段必须与相应的请求包相同。                       |
| Attr[2]  属性  （Attribute）                                | Byte  1 Bit 2                    | 表示这个TLP是否使用基于ID的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering（IDO）”一节。 |
| TH  TLP处理提示  （TLP Processing Hints）                   | Byte  1 Bit 0                    | 完成包中，此字段为保留字段                                   |
| TD  TLP摘要  （TLP Digest）                                 | Byte  2 Bit 7                    | 用于指示TLP的末尾是否有1DW的Digest字段，如果为1则说明存在。  |
| EP  受污染的数据  （Poisoned Data）                         | Byte  2 Bit 6                    | 如果为1，则表示数据荷载是被污染的。                          |
| Attr[1:0]  属性  （Attribute）                              | Byte  2 Bit 5:4                  | 完成包的该字段必须与相应的请求包相同。                       |
| AT[1:0]  地址类型  （Address Type）                         | Byte  2 Bit 3:2                  | 完成包的Address Type字段为保留字段，必须为0。                |
| Length[9:0]  长度                                           | Byte  2 Bit 1:0  Byte  3 Bit 7:0 | 用于表示完成包数据荷载的大小，单位为DW。                     |
| Completer  ID[15:0]  Completer  ID                          | Byte  4 Bit 7:0  Byte  5 Bit 7:0 | 用于表示Completer是谁，用于支持调试。  Byte 4,7:0=Compl  Bus Number  Byte 5,7:3=Compl  Device Number  Byte 5,2:0=Compl  Function Number |
| Compl.  Status[2:0]  完成包状态  （Completion Status Code） | Byte  6 Bit 7:5                  | 这个字段用于表示完成包的状态。  000b=Successful  Completion（SC）成功完成  001b=Unsupported  Request（UR）不支持的请求  010b=Config  Req Retry Status（CRS）配置请求重试状态  100b=Completer  Abort（CA）完成者终止  除了上述bit组合之外的都是保留码。更多内容请参阅“Summary of Completion Status Codes”一节。 |
| BCM  字节数已修改  （Byte Count Modified）                  | Byte  6 Bit 4                    | 这个字段仅由PCI-X完成包使用，用来表示Byte Count字段仅报告第一个数据荷载的大小，而不是总数据荷载的大小。更多内容请参阅“Using The Byte Count Modified Bit”一节。 |
| Byte  Count[11:0]  字节数                                   | Byte  6 Bit 3:0  Byte  7 Bit 7:0 | Byte Count字段是用来服务读请求的，它也是来源于原始请求中的Length字段。一些特别情况下，会出现使用多完成包返回数据荷载，请参阅“Data  Returned For Read Request”一节 |
| Requester  ID[15:0]  Requester  ID                          | Byte  8 Bit 7:0  Byte  9 Bit 7:0 | 该字段是从原始请求中复制来的，用于作为路由信息来帮助完成包返回Requester。  Byte 4,7:0=Req  Bus Number  Byte 5,7:3=Req  Device Number  Byte 5,2:0=Req  Function Number |
| Tag[7:0]  Tag                                               | Byte  10 Bit 7:0                 | 完成包中的Tag必须与相应请求中的Tag一样。Requester收到这个完成包时是通过Tag来将完成包与原始请求关联起来的。 |
| Lower  Address[6:0]  低位地址                               | Byte  11 Bit 6:0                 | 这个字段是一个读请求所返回的第一组数据的低7bit地址。它是由请求Length和Byte Enable共同计算出来的，它可以展示出在到达下一个读完成包边界之前还能传输多少字节数据，以此来协助Buffer管理。更多内容请参阅“Calculation Lower Address Field”一节。 |

表 5‑7完成包Header格式

###### l 完成包状态码总结（Summary of Completion Status Codes）

o 000b（SC）Successful Completion成功完成：请求被正确服务。

o 001b（UR）Unsupported Request不支持的请求：对于Completer来说，此请求是非法的或者是无法被识别的。这是一种出错的情况，至于Completer如何进行响应则要根据Completer对应的PCIe协议版本来。在PCIe 1.1之前，这种情况被视为一种不可纠正的错误，但是在1.1和之后的版本中，它被作为一种Advisory Non-Fatal Error（报告的非致命错误）。更多详细内容请参阅“Unsupported Request（UR） Status”一节。

o 010b（CRS）Configuration Request Retry Status配置请求重试状态：Completer临时的无法服务一个配置请求，因此这个请求应该稍后再次尝试。

o 100b（CA）Completer Abort完成者终止：Completer应该已经对请求事务进行了服务，但是因为某些原因导致了服务失败。这是一种不可纠正的错误。

###### l 计算低位地址字段（Calculating the Lower Address Field）

Completer设置这个字段是用来反映返回给Requester的数据荷载中，第一个enabled的字节的字节对齐地址（byte-aligned address）。硬件会通过原始请求中的DW起始地址以及首DW字节使能字段（1st DW Byte Enable）中的字节使能情况，来计算出这个地址的值。

对于MRd（Memory Read）请求，这个地址就是从DW起始地址开始的偏移量：

o 如果首DW字节使能字段为1111b，也就是第一个DW中的所有字节都是有效的，那么偏移量就是0。此时这个字段就与DW对齐的起始地址相同。

o 如果首DW字节使能字段为1110b，也就是第一个DW中的高3个字节都是有效的，那么偏移量就是1。此时这个字段就等于DW对齐的起始地址+1。

o 如果首DW字节使能字段为1100b，也就是第一个DW中的高2个字节都是有效的，那么偏移量就是2。此时这个字段就等于DW对齐的起始地址+2。

o 如果首DW字节使能字段为1000b，也就是第一个DW中的仅有最高字节是有效的，那么偏移量就是3。此时这个字段就等于DW对齐的起始地址+3。

 

一旦上述的计算完成，计算结果的低7bit就会被放入完成包Header的Lower Address字段，当读完成包小于整个数据荷载因而需要停止在第一个RCB（Read Completion Boundary）时，这个字段可以有助于更好的处理这种情况。若想将一个事务拆分成多个部分，则必须要在RCB上进行拆分，而到达第一个RCB时所传输的字节数量是根据起始地址而定的。

对于AtomicOp（原子操作）完成包，Lower Address字段是保留位。对于所有其他完成包类型（非读请求、非原子操作），这个字段必须为0。

###### l 使用字节数修改位（Using The Byte Count Modified Bit）

这个bit仅由PCI-X Completer进行设置，但是如果使用了PCIe到PCI-X的Bridge，那么这些PCI-X Completer们就也可以出现在PCIe拓扑结构中。使用这个bit的规则包括：

\1.    当一个读请求要拆分成多个完成包来返回数据时，Byte Count Modified这一位只能由PCI-X Completer进行置位。

\2.    仅有一系列完成包中的第一个可以将这个bit置为1，这表示第一个完成包内包含的Byte Count字段只反映这个完成包（第一个完成包）的数据荷载，而不是整个事务的数据荷载（通常情况下是表示整个事务的总数据荷载）。这样Requester就知道，即使Byte Count看起来似乎表示这是这个请求的最后一个完成包，但是实际上这个完成包之后还会有完成包，以此来满足原始请求对数据量的需求。

\3.    对于随后的一系列完成包来说，BCM位必须置为0，且Byte Count字段要用来反映剩下的所有数据荷载额字节数，就像它通常的含义一样。

\4.    当设备接收到BCM位为1的完成包时，它必须要正确的处理这种情况。

\5.    Completer要在带有数据荷载的完成包中设置Lower Address字段，以此来反映返回的第一个有效字节的地址。

###### l 读请求数据返回（Data Returned For Read Requests）:

\1.    一个读请求可能需要多个完成包来返回数据，但是最终传输的数据总量必须等于原始请求中要求的大小，否则可能导致完成包超时错误（Completion Timeout error）。

\2.    一个完成包只能用于服务一个请求。

\3.    IO和Configuration读请求所请求的数据量永远是1DW，并且只能由一个单独的完成包来返回数据。

\4.    状态码不是SC（Successful Completion）的完成包将会终止当前事务。

\5.    在使用多个完成包来响应读请求时，必须注意读完成包边界（RCB，Read Completion Boundary）。对于RC来说，RCB会是64Bytes或者128Bytes，这是因为RC可以更改在其端口间移动的数据包的大小，具体的RCB的值可以在配置寄存器中看到。

\6.    Bridge和EP可以用一个bit来实现软件对RCB大小的选择，64bytes或者128bytes。

\7.    完全在一个对齐RCB边界内的完成包们必须一次完成传输，这是因为这些RCB边界内的完成包的传输是不会到达RCB的，而RCB才是允许提前停止传输的位置，这样就必须对一个RCB边界内的所有完成包一次完成传输而不能中途停止。

\8.    对于一个单独的读请求来说，若使用多个完成包来返回数据，那么这些数据必须以地址递增的顺序来返回。

###### l 接收者对完成包的处理规则（Receiver Completion Handling Rules）:

\1.    如果一个已经被接收的完成包并没有匹配上任何一个待完成的请求，那么它就是一个Unexpected Completion（意外完成包），将被当做是一种错误来处理。

\2.    如果完成包状态不是SC或者CRS，那么就将被当做是错误来处理，并且会清空与之相关联的Buffer空间。

\3.    当RC在进行配置事务时接收到一个CRS状态的完成包，那么这个配置请求就被终止了。随后要进行的事情是根据具体实现而不同的，但是如果RC支持这种CRS状态的完成包处理，那么具体的操作将由RC控制寄存器中的CRS Software Visibility位来进行定义设置。

—  如果CRS Software Visibility没有启用，那么RC将会重新发出配置请求，直到重复了一定的次数（具体实现中指定）之后就会放弃重发的操作，并认为请求目标存在问题。

—  如果CRS Software Visibility是启用的，那么用来支持它的软件都会首先读取Vendor ID字段的两个字节，如果硬件接收到了这个读请求的CRS完成包，它将给返回的Vendor ID将会是0001h。这个值是PCI-SIG规定的保留值，它并不对应任何有效的Vendor，并会通知软件发生了这样的事件。这使得软件可以在等待目标Function准备就绪的这段时间里（通常在复位后需要1s时间），去执行其他的任务，而不是停滞不前。任何其他的配置读或者写（除了最开始的配置读Vendor ID）都会被RC自动重试重发，直到达到设计的重复次数。

\4.    如果一个请求并不是配置请求，但是它的完成包出现了CRS状态，那么这个完成包将被认为是一个畸形TLP（Malformed TLP）。

\5.    当完成包状态码为保留组合时，并不是定义出的那几种，就把它当做UR（Unsupported Request）一样处理。

\6.    如果接收到的一个读完成包或者一个原子操作完成包的状态码不是SC，那么这个完成包就不会携带数据，并且Requester必须要考虑终止这个完成包对应的请求。至于Requester具体如何处理这种情况则根据具体实现而定。

\7.    在使用多个完成包来响应一个读请求的情况下，任何一个完成包的状态只要不是SC，那么就会终止这个事务。设备如何处理出错之前接收到的数据根据具体实现而定。

\8.    为了保持对PCI的兼容性，RC需要在一个配置事务被UR结束时，合成一个全1的读取值。这就类同于枚举软件尝试从不存在的设备中读取数据时的出现的PCI Master Abort一样。

##### 5.2.5.5   消息请求（Message Requests）

Message请求取代了PCI/PCI-X中使用的许多中断（interrupt）、错误（error）和电源管理（power management）的边带信号。所有的Message请求使用的都是4DW Header格式，但是并不是每种Message都使用了Header中的每一个字段。对于Header中的Byte8-15来说，在一些种类的Message中就没有其定义，在这种情况下它就是保留字段。Message的处理方式更像是Posted的MWr事务，但是Message的路由方式可以是基于地址的、基于ID的，在一些情况下还可以是隐式路由。TLP Header中的路由子域（routing subfield），也就是Byte 0 bit 2:0，它用来表示使用的是哪种路由方法，并且相应的在Header中定义了哪些附加的字段，例如使用基于ID路由的Header中就会有用于路由的BDF。通用的Message请求Header格式如图 5‑11所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image232.jpg)

图 5‑11 4DW的Message请求Header格式

 

 

 

###### l Message请求Header各字段（Message Request Header Fields）

| 字段名称                                  | 位于Header中的Byte/Bit                                       | 功能                                                         |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fmt[2:0]  格式  （Format）                | Byte  0 Bit 7:5                                              | Message始终为4DW Header：  001b=4DW，无数据  011b=4DW，有数据 |
| Type[4:0]  类型                           | Byte  0 Bit 4:0                                              | TLP包的Type字段。  o Bit 4:3：  10b=Msg  o Bit 2:0：（Message路由子域）  000b=到RC的隐式路由。  001b=使用地址路由  010b=使用ID路由  011b=来自RC的隐式路由广播  100b=本地；终结于首级接收者  101b=收集并路由至RC  其他值=保留值，作为本地处理 |
| TC[2:0]  流量类型  （Traffic Class）      | Byte  1 Bit 6:4                                              | Message的TC始终为0，以确保它们不会影响到高优先级的数据包。   |
| Attr[2]  属性  （Attribute）              | Byte  1 Bit 2                                                | 表示这个TLP是否使用基于ID的排序（ID-based Ordering）。更多内容请参阅“ID Based Ordering（IDO）”一节。 |
| TH  TLP处理提示  （TLP Processing Hints） | Byte  1 Bit 0                                                | Message中，除非另有注明，窦泽此字段为保留字段。              |
| TD  TLP摘要  （TLP Digest）               | Byte  2 Bit 7                                                | 如果为1，则说明TLP的末尾存在1DW的Digest字段（在LCRC和END字符之前）。 |
| EP  受污染的数据  （Poisoned Data）       | Byte  2 Bit 6                                                | 如果为1，则表示数据荷载是被污染的。                          |
| Attr[1:0]  属性  （Attribute）            | Byte  2 Bit 5:4                                              | Message中，除非另有注明，窦泽此字段为保留字段。              |
| AT[1:0]  地址类型  （Address Type）       | Byte  2 Bit 3:2                                              | Message的Address Type字段为保留字段，必须为0，但是并不要求甚至不鼓励接收方对这个字段进行检查。 |
| Length[9:0]  长度                         | Byte  2 Bit 1:0  Byte  3 Bit 7:0                             | 用于表示完成包数据荷载的大小，单位为DW。对于Message来说，这个字段始终为0（无数据）或1（有1DW数据）。 |
| Requester  ID[15:0]  Requester  ID        | Byte  4 Bit 7:0  Byte  5 Bit 7:0                             | 用于表示发送Message的Requester是谁。  Byte 4,7:0=Requester  Bus Number  Byte 5,7:3=Requester  Device Number  Byte 5,2:0=Requester  Function Number |
| Tag[7:0]  Tag                             | Byte  6 Bit 7:0                                              | 由于所有的Message请求都是Posted的，不需要Requester接收完成包，因此不需要给Message分配Tag。这个字段应该为0。 |
| Message  Code[7:0]  Message标识码         | Byte  7 Bit 7:0                                              | 这个字段包含的标识码表示发送的Message是什么种类。  o   0000 0000b=解锁Message（Unlock Message）  o   0001 0000b=延迟容忍报告（Lat. Tolerance Reporting）  o   0001 0010b=优化的Buffer刷新/填满（Optimized Buffer Flush/Fill）  o   0001 xxxxb=电源管理Message（Power Mgt. Message）  o   0010 0xxxb=INTx Message  o   0011 00xxb=错误Message（Error Message）  o   0100 xxxxb=忽略Message（Ignored Message）  o   0101 0000b=设置插槽功耗Message（Set Slot Power Message）  o   0111 111xb=厂商定义的Message（Vendor Defined Message） |
| Address[63:32]  高32bit地址               | Byte  8 Bit 7:0  Byte  9 Bit 7:0  Byte  10 Bit 7:0  Byte  11 Bit 7:0 | 如果Message选用了地址路由（见上面介绍的Type[4:0]字段），那么这个字段的内容就是64bit起始地址的高32bit。如果使用是ID路由，那么Byte8-9就组成目标ID。  否则，这个字段就不使用。 |
| Address[31:0]  低32bit地址                | Byte  12 Bit 7:0  Byte  13 Bit 7:0  Byte  14 Bit 7:0  Byte  15 Bit 7:0 | 如果Message选用了地址路由（见上面介绍的Type[4:0]字段），那么这个字段的内容就是64bit起始地址的低32bit（最低的2bit必须为0，以保持DW对齐）。  否则，这个字段就不使用。 |

表 5‑8 Message请求Header各字段

###### l Message注意事项（Using The Byte Count Modified Bit）

接下来几个小节的列表会列出9种Message组（如表 5‑8中给出的）的各自使用的更具体的Message Code。这9种Message组为：

\1.    INTx中断信号（INTx Interrupt Signaling）

\2.    电源管理（Power Management）

\3.    错误信号（Error Signaling）

\4.    支持锁定事务（Locked Transaction Support）

\5.    支持插槽功耗限制（Slot Power Limit Support）

\6.    厂商定义的Message（Vendor-Defined Message）

\7.    忽略Message（Ignored Message，与PCIe 1.1中的热插拔支持相关）

\8.    延迟容忍报告（Latency Tolerance Reporting，LTR）

\9.    优化的Buffer刷新和填满（Optimized Buffer Flush and Fill，OBFF）

###### l INTx中断Message（INTx Interrupt Messages）

许多设备都适用PCI 2.3的MSI（Message Signaled Interrupt）方法来传输中断，但是对于老的设备来说可能会不支持MSI。对于这些情况，PCIe定义了一个“虚拟线virtual wire”作为替代方案，设备通过发送Message来模拟PCI中断引脚（INTA-INTD）的拉起与释放。中断设备发出第一个Message来通知上行设备自己拉起了一个中断。一旦这个中断被服务了，那么中断设备就发出第二个Message来表示这个“虚拟中断线”已经释放。更多关于这种协议的内容，请参阅“Virtual INTx Signaling”一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image234.jpg)

表 5‑9 INTx中断信号Message的代码

关于使用INTx Message的一些规则：

\1.    INTx Message没有数据荷载，因此Length字段为保留字段。

\2.    它们只会由上行端口（Upstream Port）发出。对接收到的数据包进行这方面的规则检查是可选项，但是如果进行了检查，那么发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

\3.    INTx Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

\4.    链路两端的组件必须跟踪四种虚拟中断（virtual interrupt）的状态。如果上行端口的一个中断的逻辑状态发生了改变，那么它必须发出相应的INTx Message。

\5.    当命令寄存器（Command Register）中的Interrupt Disable位被置为1时，INTx信号是被禁用的（就像物理中断线的类似情况一样）。

\6.    如果在Interrupt Disable位为1时，设备的任何一个虚拟INTx信号仍为激活状态，那么上行端口必须发出对应的释放INTx Message（Deassert_INTx Message）。

\7.    Switch的各个下行端口必须独立的跟踪四种INTx信号状态，并组合成上行端口的状态。

\8.    RC的各个下行端口必须独立的跟踪下行端口的INTx信号状态，并将它们转换成系统中断，具体的转换方式根据具体实现而定。

\9.    INTx Message使用“Local-Terminate at Receiver在接收者本地结束”的路由类型来使得一个Switch在必要时对指定的中断引脚进行重映射（参阅“Mapping and Collapsing INTx Messages”一节）。因此，INTx Message中的Requester ID有可能是最后一个传送者所指定的。

###### l 电源管理Message（Power Management Messages）

PCIe对PCI的电源管理是兼容的，并且还加入了基于硬件的链路电源管理。Message是用来传达一些关于电源管理的信息，但是如果想了解完整的PCIe电源管理协议是如何工作的，那么请参阅Chapter 16 “Power Management”一章。表 5‑10总结了四种电源管理Message类型。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image236.jpg)

表 5‑10电源管理Message标识码

关于使用电源管理Message的一些规则：

\1.    电源管理Message没有数据荷载，因此Length字段为保留字段。

\2.    电源管理Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

\3.    当一个下行端口收到了一个链路对端电源管理请求，请求要将链路电源状态更改为L1，但是下行端口拒绝这种更改，那么它就要发出PM_Active_State_Nak这种电源管理Message。

\4.    当一个组件请求电源管理事件（Power Management Event）时，它的上行端口需要发出PM_PME，这个Message将被隐式路由至RC。

\5.    PM_Turn_Off会向下发送给所有的EP（由RC发出，进行隐式路由的广播）。

\6.    PME_TO_Ack是由EP的上行端口发出的。对于拥有多个下行端口的Switch来说，只有当所有的下行端口都收到了这个Message，才会将其转发至上行（也就是汇聚收集，并路由转发给RC）。

###### l 错误Message（Error Messages）

当启用了错误Message的组件检测到了错误，那么就会将错误Message向上发送（隐式路由至RC）。为了协助软件，让软件知道应该如何处理这个错误，在Error Message中使用Header中的Requester ID字段来表示请求者。表 5‑11总结了三种Error Message类型。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image238.jpg)

表 5‑11错误Message标识码

关于使用Error Message的一些规则：

\1.    Error Message没有数据荷载，因此Length字段为保留字段。

\2.    Error Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

\3.    RC会将Error Message转换成系统指定的事件。

###### l 支持锁定事务（Locked Transaction Support）

解锁Message（Unlock Message）被应用于PCI定义的锁定事务协议中（Locked Transaction Protocol），并且在PCIe中依然对传统PCI遗留设备可用。这种协议起始于一个MRd Lock请求。当这个请求在被送达到目标设备的过程中，沿途的端口们都会发现它，这些端口会实现一个原子“读-修改-写”协议（read-modify-write protocol），也就是说它们将会锁定VC0（Virtual Channel 0）让其他的请求不能使用，直到收到Unlock Message才会对VC0解锁。这种Unlock Message会被发送至Locked事务的目标设备，以此来释放传输路径中的所有被锁定的端口，并用于最终完成锁定事务的一系列行为。表 5‑12总结了Unlock Message的标识码。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image240.jpg)

表 5‑12解锁Message标识码

关于使用Unlock Message的一些规则：

\1.    Unlock Message没有数据荷载，因此Length字段为保留字段。

\2.    Unlock Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

###### l 设置插槽功率限制Message（Set Slot Power Limit Message）

这种Message是由下行端口发送给插在插槽上的设备的。这里面的功率限制信息会储存在EP的设备能力寄存器中（Device Capabilities Register）。表 5‑12总结了Unlock Message的标识码。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image242.jpg)

表 5‑13插槽功率限制Message标识码

关于使用Set Slot Power Limit Message的一些规则：

\1.    Set Slot Power Limit Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

\2.    Set Slot Power Limit Message的数据荷载为1DW，因此Length字段的值为1。而在32bit数据荷载中仅有低10bit用于进行插槽功率放大或减小；剩下的高位必须都置为0。

\3.    当数据链路层转换为DL-Up状态时，该消息会自动发送。或者是放数据链路层已经报告了DL-Up状态，然后发生了一个配置写来写入Slot Capabilities Register时，该消息也会自动发送。

\4.    如果插槽中的板卡消耗的功率已经低于了指定的功率限制，那么可以忽略这种Message。

###### l 厂商定义的Message 0和1（Vendor-Defined Message 0 and 1）

Vendor-Defined Message是用于进行PCIe Message能力的扩展，这种扩展既可以通过协议本身，也可以通过厂商来指定的扩展。这种Message的Header格式如图 5‑12所示，Message标识码如表 5‑12。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image244.jpg)

图 5‑12厂商定义的Message Header格式

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image246.jpg)

表 5‑14厂商定义的Message标识码

关于使用Vendor-Defined Message的一些规则：

\1.    Vendor-Defined Message 0和1可以有也可以没有数据荷载。

\2.    Message用Vendor ID来进行区分。

\3.    Attr[2]和Attr[1:0]并不是保留位。

\4.    如果接收者无法认出这种Message：

o Type 1 Message可以被安静的忽略丢弃掉。

o Type 0 Message要被作为Unsupported Request（UR）这种错误情况来处理。

###### l 被忽略的Message（Ignored Message）

如果直接列出一整个目录的需要被忽略的Message，而不去讲忽略的前因后果，那可能会听起来有一些奇怪。这些Message其实是原来的热插拔信号Message（Hot Plug Signaling Message），它们用于支持设备，这些设备上具有热插拔指示器，并且只需要按下这个插卡设备上的按钮而不是系统板上的按钮。这种Message类型是由PCIe 1.0a版本所定义的，但是这个选项在PCIe 1.1版本中就已经不再支持了，因此这里所讲的这些细节内容仅作为参考。正如名字要表达的那样，强烈建议发送方不要发送这些Message，并且也强烈建议接收方就算收到它们也要忽视掉。如果无论如何仍要使用这些Message，那么它们必须要符合PCIe 1.0a的协议细节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image248.jpg)

表 5‑15热插拔Message标识码

关于使用Hot Plug Message的一些规则：

\1.    Hot Plug Message由下行端口发送给插槽中的板卡。

\2.    Attention_Button Message是由插槽中的设备向上行发送的。

###### l 延迟容忍报告Message（Latency Tolerance Reporting Message）

LTR Message是用来报告一个设备可接受的读写服务延迟，这是一个可选项。更多关于这种电源管理技术的内容，请参阅“LTR（Latency Tolerance Reporting）”一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image250.jpg)

图 5‑13 LTR Message Header格式

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image252.jpg)

表 5‑16 LTR Message标识码

关于使用LTR Message的一些规则：

\1.    LTR Message没有数据荷载，因此Length字段为保留字段。

\2.    LTR Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

###### l 优化的Buffer刷新和填充Message（Optimized Buffer Flush and Fill）

OBFF Message用来向EP报告平台电源情况，以此来促进更搞笑的系统电源管理。更多关于这种技术的内容，请参阅“OBBF（Optimized Buffer Flush and Fill）”一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image254.jpg)

图 5‑14 OBBF Message Header格式

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image256.jpg)

表 5‑17 OBBF Message标识码

关于使用OBFF Message的一些规则：

\1.    OBFF Message没有数据荷载，因此Length字段为保留字段。

\2.    OBFF Message需要使用默认的流量类型TC 0。接收方必须检查这一条规则，发现违例的数据包就会被当做畸形TLP（Malformed TLP）。

\3.    Header中的Requester ID必须为传送中的端口的ID。