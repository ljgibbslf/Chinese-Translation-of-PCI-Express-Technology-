 

 

 

 

 

 

 

 

PCIe技术

**——面向****1.x,2.x,3.0****的综合指南**

 

 

 



| 版本 | 修改日期   | 修改描述                                                     | 作者   |
| ---- | ---------- | ------------------------------------------------------------ | ------ |
| V0.1 | 2020-07-27 | 初版，终于开始了，拖了两个星期今天看并开始翻译第一章Big Picture中关于PCI的内容 | 张仲禹 |
| V0.2 | 2020-08-03 | PCI与PCI-X的讲述结束了，终于能开始PCIe了                     | 张仲禹 |
| V0.3 | 2020-08-16 | Chapter 2对PCIe体系结构的概述完成了                          | 张仲禹 |
| V0.4 | 2020-09-03 | 随着Chapter 4完成，Part 1终于完成了！                        | 张仲禹 |
| V0.5 | 2020-10-22 | 前几天找完工作后继续开始进行，今天完成了Chapter 6            | 张仲禹 |
| V0.6 | 2020-11-24 | Part 2完成！事务层的东西完成后，一个较为高层视角的PCIe架构基本上展现了出来。 | 张仲禹 |

 

 

 

 



目录

[第一部分    Part One：The Big Picture大局总览... 12](#_Toc57153850)

[Chapter 1  Background //背景... 12](#_Toc57153851)

[关于本章... 12](#_Toc57153852)

[关于下一章... 12](#_Toc57153853)

[1.1    引言... 12](#_Toc57153854)

[1.2    PCI与PCI-X.. 12](#_Toc57153855)

[1.3    PCI基础(PCI Basics) 14](#_Toc57153856)

[1.3.1   基于PCI的系统的一些基础知识... 14](#_Toc57153857)

[1.3.2   PCI总线发起方(Initiator)与目标方(Target) 14](#_Toc57153858)

[1.3.3   典型的PCI总线周期... 15](#_Toc57153859)

[1.3.4   反射波信号传输(Reflected-Wave Signaling) 17](#_Toc57153860)

[1.4    PCI总线体系结构透视探究(PCI Bus Architecture Perspective) 19](#_Toc57153861)

[1.4.1   PCI事务模型... 19](#_Toc57153862)

[1.4.1.1   Programmed  I/O(PIO) 19](#_Toc57153863)

[1.4.1.2   Direct  Memory Access(DMA) 20](#_Toc57153864)

[1.4.1.3   Peer-to-Peer(点对点) 20](#_Toc57153865)

[1.4.2   PCI总线仲裁... 21](#_Toc57153866)

[1.4.3   PCI低效的地方(PCI Inefficiencies) 21](#_Toc57153867)

[1.4.3.1   PCI重试协议(PCI Retry Protocol) 21](#_Toc57153868)

[1.4.3.2   PCI断开连接协议(PCI Disconnect  Protocol) 22](#_Toc57153869)

[1.4.4   PCI中断处理(PCI Interrupt  Handling) 23](#_Toc57153870)

[1.4.5   PCI错误处理(PCI Error Handling) 24](#_Toc57153871)

[1.4.6   PCI地址空间映射(PCI Address Map) 25](#_Toc57153872)

[1.4.7   PCI配置周期的生成(PCI Configuration  Cycle Generation) 26](#_Toc57153873)

[1.4.8   PCI  Function配置寄存器空间(PCI  Function Cfg Reg Space) 26](#_Toc57153874)

[1.4.9   更高带宽的PCI(Higher-bandwidth PCI) 29](#_Toc57153875)

[1.4.9.1   66MHz  PCI总线的限制... 29](#_Toc57153876)

[1.4.9.2   并行PCI总线模型在66MHz以上时出现的信号时序问题... 29](#_Toc57153877)

[1.5    PCI-X简介... 30](#_Toc57153878)

[1.5.1   PCI-X系统示例(PCI-X System  Example) 30](#_Toc57153879)

[1.5.2   PCI-X事务(PCI-X Transactions) 31](#_Toc57153880)

[1.5.3   PCI-X特性(PCI-X Features) 31](#_Toc57153881)

[1.5.3.1   拆分事务模型（Split-Transaction Model）... 31](#_Toc57153882)

[1.5.3.2   MSI消息式中断（Message Signaled  Interrupts）... 32](#_Toc57153883)

[1.5.3.3   事务属性（Transaction Attributes）... 33](#_Toc57153884)

[1.5.4   更高带宽的PCI-X(Higher Bandwidth PCI-X) 33](#_Toc57153885)

[1.5.4.1   PCI与PCI-X 1.0并行总线模型中由于公共时钟方法所带来的问题... 33](#_Toc57153886)

[1.5.4.2   PCI-X  2.0源同步模型（PCI-X  2.0 Source-Synchronous Model）... 34](#_Toc57153887)

[Chapter 2  PCIe Architecture Overview //PCIe体系结构概述... 36](#_Toc57153888)

[关于前一章... 36](#_Toc57153889)

[关于本章... 36](#_Toc57153890)

[关于下一章... 36](#_Toc57153891)

[2.1    PCI Express简介... 36](#_Toc57153892)

[2.1.1   软件的向后兼容性（Software Backward Compatibility）... 37](#_Toc57153893)

[2.1.2   串行传输（Serial Transport）... 37](#_Toc57153894)

[2.1.2.1   对传输速率的需求（The Need for Speed）... 37](#_Toc57153895)

[2.1.2.2   PCIe带宽计算方法... 39](#_Toc57153896)

[2.1.2.3   PCIe的差分信号... 40](#_Toc57153897)

[2.1.2.4   不再使用公共时钟... 40](#_Toc57153898)

[2.1.2.5   基于数据包的协议体系（Packet-based Protocol）... 41](#_Toc57153899)

[2.1.3   链路和通道（Links and Lanes）... 42](#_Toc57153900)

[2.1.3.1   可扩展的性能（Scalable Performance）... 42](#_Toc57153901)

[2.1.3.2   灵活的拓扑结构选择（Flexible Topology Options）... 42](#_Toc57153902)

[2.1.4   关于PCIe拓扑的一些定义... 42](#_Toc57153903)

[2.1.4.1   拓扑特征（Topology Characteristics）... 43](#_Toc57153904)

[2.1.4.2   Root  Complex（RC 根组件）... 43](#_Toc57153905)

[2.1.4.3   Switches  and Bridge（交换结构与桥）... 43](#_Toc57153906)

[2.1.4.4   Native  PCIe Endpoints and Legacy PCIe Endpoints（原生与遗留EP）... 44](#_Toc57153907)

[2.1.4.5   软件兼容特性... 44](#_Toc57153908)

[2.1.4.6   系统示例... 46](#_Toc57153909)

[2.2    设备层次介绍（Introduction to  Device Layers）... 48](#_Toc57153910)

[l   设备核（Device Core）以及它与事务层（Transaction  Layer）的接口。... 48](#_Toc57153911)

[l   事务层（Transaction Layer）。... 48](#_Toc57153912)

[l   数据链路层（Data Link Layer）。... 48](#_Toc57153913)

[l   物理层（Physical Layer）。... 49](#_Toc57153914)

[2.2.1   设备核/软件层（Device Core/Software Layer）... 51](#_Toc57153915)

[2.2.2   事务层（Transaction Layer）... 51](#_Toc57153916)

[2.2.2.1   TLP基础内容... 52](#_Toc57153917)

[l   TLP组包（TLP Packet Assembly）... 54](#_Toc57153918)

[l   TLP拆包（TLP Packet Disassembly）... 55](#_Toc57153919)

[2.2.2.2   Non-Posted事务... 56](#_Toc57153920)

[l   普通读（Ordinary Reads）... 56](#_Toc57153921)

[l   锁定读（Locked Reads）... 57](#_Toc57153922)

[l   IO和配置写（IO and Configuration Writes）... 58](#_Toc57153923)

[2.2.2.3   Posted写事务... 59](#_Toc57153924)

[l   Memory写（Memory Writes）... 59](#_Toc57153925)

[l   Message写（Message Writes）... 60](#_Toc57153926)

[2.2.2.4   QoS服务质量（Quality of Service）... 60](#_Toc57153927)

[2.2.2.5   事务排序（Transaction Ordering）... 62](#_Toc57153928)

[2.2.2.6   流量控制（Flow Control）... 62](#_Toc57153929)

[2.2.3   数据链路层（Data Link Layer）... 62](#_Toc57153930)

[2.2.3.1   DLLPs数据链路层包（Data Link Layer  Packet）... 63](#_Toc57153931)

[l   DLLP组包（DLLP Assembly）... 63](#_Toc57153932)

[l   DLLP拆包（DLLP Disassembly）... 63](#_Toc57153933)

[2.2.3.2   Ack/Nak协议（Ack/Nak Protocol）... 63](#_Toc57153934)

[2.2.3.3   流量控制（Flow Control）... 65](#_Toc57153935)

[2.2.3.4   电源管理（Power Management）... 66](#_Toc57153936)

[2.2.4   物理层（Physical Layer）... 66](#_Toc57153937)

[2.2.4.1   整体说明（General）... 66](#_Toc57153938)

[2.2.4.2   逻辑物理层（Physical Layer-Logical）... 66](#_Toc57153939)

[2.2.4.3   链路训练和初始化（Link Training and Initialization）... 67](#_Toc57153940)

[2.2.4.4   电气物理层（Physical Layer-Electrical）... 68](#_Toc57153941)

[2.2.4.5   命令集（Ordered Sets）... 68](#_Toc57153942)

[2.3    协议回顾（Protocol Review  Example）... 69](#_Toc57153943)

[2.3.1   Memory  Read Request（MRd请求）... 69](#_Toc57153944)

[2.3.2   Completion  with Data（CplD带有数据的完成包）... 71](#_Toc57153945)

[Chapter 3  Configuration Overview //PCIe配置概述... 73](#_Toc57153946)

[关于前一章... 73](#_Toc57153947)

[关于本章... 73](#_Toc57153948)

[关于下一章... 73](#_Toc57153949)

[3.1    总线/设备/功能/的定义（Definition of  Bus,Device and Function）... 73](#_Toc57153950)

[3.2    PCIe总线（PCIe Buses）... 74](#_Toc57153951)

[3.3    PCIe设备（PCIe Devices）... 75](#_Toc57153952)

[3.4    PCIe功能（PCIe Functions）... 75](#_Toc57153953)

[3.5    配置地址空间（Configuration  Address Space）... 75](#_Toc57153954)

[3.5.1   PCI兼容空间（PCI-Compatible Space）... 76](#_Toc57153955)

[3.5.2   扩展配置空间（Extended Configuration Space）... 76](#_Toc57153956)

[3.6    Host-to-PCI Bridge配置寄存器... 77](#_Toc57153957)

[3.6.1   整体说明（General）... 77](#_Toc57153958)

[3.6.2   只有RC发送配置请求（Only the Root Sends Configuration Request）... 77](#_Toc57153959)

[3.7    生成配置事务（Generating  Configuration Transactions）... 78](#_Toc57153960)

[3.7.1   传统PCI机制（Legacy PCI Mechanism）... 78](#_Toc57153961)

[3.7.1.1   配置地址端口（Configuration Address Port）... 78](#_Toc57153962)

[3.7.1.2   总线比较和数据端口的使用（Bus Compare and Data Port Usage）... 79](#_Toc57153963)

[3.7.1.3   单Host系统（Single Host System）... 80](#_Toc57153964)

[3.7.1.4   多Host系统（Multi-Host System）... 82](#_Toc57153965)

[3.7.2   增强型配置访问机制（Enhanced Configuration Access Mechanism）... 83](#_Toc57153966)

[3.7.2.1   整体说明（General）... 83](#_Toc57153967)

[3.7.2.2   一些规则（Some Rules）... 84](#_Toc57153968)

[3.8    配置请求（Configuration  Requests）... 85](#_Toc57153969)

[3.8.1   Type  0配置请求（Type 0  Configuration Request）... 85](#_Toc57153970)

[3.8.2   Type  1配置请求（Type 1  Configuration Request）... 86](#_Toc57153971)

[3.9    PCI兼容配置访问示例（Example PCI-Compatible Configuration Access）... 87](#_Toc57153972)

[3.10   增强型配置访问示例（Example  Enhanced Configuration Access）... 88](#_Toc57153973)

[3.11   枚举——搜索发现拓扑（Enumeration-Discovering the  Topology）... 90](#_Toc57153974)

[3.11.1    搜索某个Function是否存在（Discovering the  Presence or Absence of a Function）  90](#_Toc57153975)

[3.11.1.1   设备不存在（Device not Present）... 90](#_Toc57153976)

[3.11.1.2   设备未准备好（Device not Ready）... 91](#_Toc57153977)

[3.11.2    确认某个Function是EP还是Bridge. 93](#_Toc57153978)

[3.12   单RC枚举示例（Single Root  Enumeration Example）... 93](#_Toc57153979)

[3.13   多RC枚举示例（Multi-Root  Enumeration Example）... 97](#_Toc57153980)

[3.13.1    整体说明（General）... 97](#_Toc57153981)

[3.13.2    多RC枚举过程（Multi-Root Enumeration Process）... 98](#_Toc57153982)

[3.13.3    热插拔注意事项（Hot-Plug Considerations）... 99](#_Toc57153983)

[3.14   MindShare Arbor：调试/验证/分析和学习的软件工具... 100](#_Toc57153984)

[3.14.1    整体说明（General）... 100](#_Toc57153985)

[3.14.2  MindShare  Arbor功能列表... 101](#_Toc57153986)

[Chapter 4  Address Space & Transaction Routing//地址空间与事务路由... 103](#_Toc57153987)

[关于前一章... 103](#_Toc57153988)

[关于本章... 103](#_Toc57153989)

[关于下一章... 103](#_Toc57153990)

[4.1    我需要一个地址（I Need An Address）... 103](#_Toc57153991)

[4.1.1   配置空间（Configuration Space）... 103](#_Toc57153992)

[4.1.2   内存和IO地址空间（Memory and IO Address Spaces）... 104](#_Toc57153993)

[4.1.2.1   整体说明（General）... 104](#_Toc57153994)

[4.1.2.2   可预取的与不可预取的内存空间的对比（Prefetchable vs. Non-Prefetchable  Memory Space）... 104](#_Toc57153995)

[4.2    基地址寄存器BARs（Base Address Registers）... 106](#_Toc57153996)

[4.2.1   整体说明（General）... 106](#_Toc57153997)

[4.2.2   BAR示例1——32bit内存地址空间请求... 108](#_Toc57153998)

[4.2.3   BAR示例2——64bit内存地址空间请求... 110](#_Toc57153999)

[4.2.4   BAR示例3——IO地址空间请求... 112](#_Toc57154000)

[4.2.5   所有的BARs都必须按顺序被评估... 113](#_Toc57154001)

[4.2.6   大小可变的BARs（Resizable BARs）... 114](#_Toc57154002)

[4.3    Base寄存器和Limit寄存器（Base and Limit Registers）... 114](#_Toc57154003)

[4.3.1   整体说明（General）... 114](#_Toc57154004)

[4.3.2   P-MMIO可预取范围（Prefetchable Range）... 115](#_Toc57154005)

[4.3.3   NP-MMIO不可预取范围（Non-Prefetchable  Range）... 117](#_Toc57154006)

[4.3.4   IO范围（IO Range）... 119](#_Toc57154007)

[4.3.5   未被使用的Base和Limit寄存器... 121](#_Toc57154008)

[4.4    完整性检查：对所有用于地址路由的寄存器（Sanity  Check：Registers  Used For Address Routing）... 122](#_Toc57154009)

[4.5    TLP路由基础（TLP Routing Basics）... 123](#_Toc57154010)

[4.5.1   接收器检查3种类型的流量（Recievers Check For Three Types of Traffic）... 124](#_Toc57154011)

[4.5.2   路由元件（Routing Elements）... 124](#_Toc57154012)

[4.5.3   TLP路由的三种方法（Three Methods of  TLP Routing）... 124](#_Toc57154013)

[4.5.3.1   整体说明（General）... 124](#_Toc57154014)

[4.5.3.2   隐式路由和Message的目的（Purpose of Implicit Routing and  Message）... 125](#_Toc57154015)

[l   为什么是Message？（Why Messages？）... 125](#_Toc57154016)

[l   隐式路由是怎么提供帮助的？（How  Implicit Routing Helps？）... 125](#_Toc57154017)

[4.5.4   拆分事务协议（Split Transaction Protocol）... 125](#_Toc57154018)

[4.5.5   Posted与Non-Posted. 126](#_Toc57154019)

[4.5.6   Header中定义数据包格式和类型的字段... 129](#_Toc57154020)

[4.5.6.1   整体说明（General）... 129](#_Toc57154021)

[4.5.6.2   Header中Format/Type字段的编码含义... 130](#_Toc57154022)

[4.5.7   TLP  Header概述... 131](#_Toc57154023)

[4.6    路由机制的应用方法（Applying  Routing Mechanisms）... 131](#_Toc57154024)

[4.6.1   ID路由（ID Routing）... 131](#_Toc57154025)

[4.6.1.1   Bus号，Device号，Function号的使用上限... 131](#_Toc57154026)

[4.6.1.2   TLP  Header中有关ID路由的关键字段... 131](#_Toc57154027)

[4.6.1.3   EP进行一次检查（Endpoint：One Check）... 132](#_Toc57154028)

[4.6.1.4   Switches（Bridges）每个端口进行两次检查... 132](#_Toc57154029)

[4.6.2   地址路由（Address Routing）... 134](#_Toc57154030)

[4.6.2.1   TLP  Header中有关地址路由的关键字段... 134](#_Toc57154031)

[l   使用32bit地址的TLP. 134](#_Toc57154032)

[l   使用64bit地址的TLP. 134](#_Toc57154033)

[4.6.2.2   EP对地址的检查... 135](#_Toc57154034)

[4.6.2.3   Switch进行路由（Switch  Routing）... 136](#_Toc57154035)

[l   TLP向下行移动（由Switch主接口接收并向下转发）... 137](#_Toc57154036)

[l   TLP向上行移动（由次级接口接收并向上转发）... 137](#_Toc57154037)

[4.6.2.4   多播能力（Multicast Capabilities）... 138](#_Toc57154038)

[4.6.3   隐式路由（Implicit Routing）... 138](#_Toc57154039)

[4.6.3.1   仅为Message所使用（Only for Messages）... 138](#_Toc57154040)

[4.6.3.2   TLP  Header中有关隐式路由的关键字段... 138](#_Toc57154041)

[4.6.3.3   Message的Type字段总结... 139](#_Toc57154042)

[4.6.3.4   EP的处理... 139](#_Toc57154043)

[4.6.3.5   Switch的处理... 139](#_Toc57154044)

[4.7    DLLP和Ordered Set不会被路由... 140](#_Toc57154045)

[第二部分    Part Two：Transaction Layer事务层... 141](#_Toc57154046)

[Chapter 5  TLP Element //TLP元素... 141](#_Toc57154047)

[关于上一章... 141](#_Toc57154048)

[关于本章... 141](#_Toc57154049)

[关于下一章... 141](#_Toc57154050)

[5.1    基于包的协议的介绍（Introduction to  Packet-Based Protocol）... 141](#_Toc57154051)

[5.1.1   整体说明（General）... 141](#_Toc57154052)

[5.1.2   使用基于包的协议的原因... 142](#_Toc57154053)

[5.1.2.1   包格式是精心定义的... 142](#_Toc57154054)

[5.1.2.2   使用组帧符号定义包的边界... 143](#_Toc57154055)

[5.1.2.3   使用CRC保障整个数据包的正确传输... 143](#_Toc57154056)

[5.2    TLP细节（TLP Details）... 143](#_Toc57154057)

[5.2.1   TLP的组包和拆包... 143](#_Toc57154058)

[5.2.2   TLP结构... 145](#_Toc57154059)

[5.2.3   通用TLP Header格式（Generic TLP Header Format）... 146](#_Toc57154060)

[5.2.3.1   整体说明（General）... 146](#_Toc57154061)

[5.2.3.2   通用Header各字段总结... 146](#_Toc57154062)

[5.2.4   通用TLP Header详细说明（Generic TLP Header Details）... 149](#_Toc57154063)

[5.2.4.1   Header的Type/Format字段编码含义... 149](#_Toc57154064)

[5.2.4.2   Digest/ECRC字段... 150](#_Toc57154065)

[l   ECRC的生成和检查... 151](#_Toc57154066)

[l   谁来检查ECRC.. 151](#_Toc57154067)

[5.2.4.3   使用字节使能（Using Byte Enables）... 151](#_Toc57154068)

[l   整体说明... 151](#_Toc57154069)

[l   字节使能规则... 152](#_Toc57154070)

[l   字节使能示例... 152](#_Toc57154071)

[5.2.4.4   事务描述符字段（Transaction Descriptor Fields）... 153](#_Toc57154072)

[l   事务ID（Transaction ID）... 153](#_Toc57154073)

[l   流量类型（Traffic Class）... 153](#_Toc57154074)

[l   事务属性（Transaction  Attributes）... 154](#_Toc57154075)

[5.2.4.5   带有数据荷载的TLP的一些额外规则... 154](#_Toc57154076)

[5.2.5   具体的TLP格式：请求TLP和完成TLP. 154](#_Toc57154077)

[5.2.5.1   IO请求（IO Request）... 154](#_Toc57154078)

[l   IO Request Header的格式... 155](#_Toc57154079)

[l   IO Request Header各字段... 156](#_Toc57154080)

[5.2.5.2   Memory请求（Memory Request）... 157](#_Toc57154081)

[l   Memory Request Header各字段... 158](#_Toc57154082)

[l   Memory Request 注意事项... 161](#_Toc57154083)

[5.2.5.3   Configuration请求（Configuration Request）... 161](#_Toc57154084)

[l   Configuration Request Header各字段的定义... 162](#_Toc57154085)

[l   Configuration Request注意事项... 164](#_Toc57154086)

[5.2.5.4   完成包（Completion）... 164](#_Toc57154087)

[l   Completion Header各字段的定义... 165](#_Toc57154088)

[l   完成包状态码总结（Summary  of Completion Status Codes）... 167](#_Toc57154089)

[l   计算低位地址字段（Calculating the  Lower Address Field）... 167](#_Toc57154090)

[l   使用字节数修改位（Using The Byte  Count Modified Bit）... 168](#_Toc57154091)

[l   读请求数据返回（Data Returned For  Read Requests）: 169](#_Toc57154092)

[l   接收者对完成包的处理规则（Receiver  Completion Handling Rules）: 169](#_Toc57154093)

[5.2.5.5   消息请求（Message Requests）... 170](#_Toc57154094)

[l   Message请求Header各字段（Message Request Header Fields）... 171](#_Toc57154095)

[l   Message注意事项（Using The Byte Count Modified Bit）... 173](#_Toc57154096)

[l   INTx中断Message（INTx Interrupt Messages）... 173](#_Toc57154097)

[l   电源管理Message（Power Management  Messages）... 174](#_Toc57154098)

[l   错误Message（Error Messages）... 175](#_Toc57154099)

[l   支持锁定事务（Locked Transaction  Support）... 176](#_Toc57154100)

[l   设置插槽功率限制Message（Set Slot Power Limit  Message）... 176](#_Toc57154101)

[l   厂商定义的Message 0和1（Vendor-Defined Message 0  and 1）... 177](#_Toc57154102)

[l   被忽略的Message（Ignored Message）... 177](#_Toc57154103)

[l   延迟容忍报告Message（Latency Tolerance  Reporting Message）... 178](#_Toc57154104)

[l   优化的Buffer刷新和填充Message（Optimized Buffer Flush  and Fill）... 179](#_Toc57154105)

[Chapter 6  Flow Control //流量控制... 180](#_Toc57154106)

[关于上一章... 180](#_Toc57154107)

[关于本章... 180](#_Toc57154108)

[关于下一章... 180](#_Toc57154109)

[6.1    流量控制概念（Flow Control  Concept）... 180](#_Toc57154110)

[6.2    流量控制Buffer和Credit（Flow Control Buffer and  Credits）... 181](#_Toc57154111)

[6.2.1   VC流量控制Buffer的组成（VC Flow Control  Buffer Organization）... 182](#_Toc57154112)

[6.2.2   流控Credit（Flow Control Credits）... 182](#_Toc57154113)

[6.3    初始流量控制通告（Initial Flow  Control Advertisement）... 183](#_Toc57154114)

[6.3.1   流控通告的最大值和最小值（Minimum and Maximum Flow Control  Advertisement）... 183](#_Toc57154115)

[6.3.2   无限Credit（Infinite Credits）... 184](#_Toc57154116)

[6.3.3   无限Credit通告的特殊用法... 185](#_Toc57154117)

[6.4    流量控制初始化（Flow Control  Initialization）... 185](#_Toc57154118)

[6.4.1   整体说明（General）... 185](#_Toc57154119)

[6.4.2   流控初始化流程（The FC Initialization Sequence）... 186](#_Toc57154120)

[6.4.3   FC_Init1详细说明（FC_Init1 Details）... 187](#_Toc57154121)

[6.4.4   FC_Init2详细说明（FC_Init2 Details）... 188](#_Toc57154122)

[6.4.5   FC_INIT1和FC_INIT2的传输速率（Rate of FC_INIT1  and FC_INIT2 Transmission）... 189](#_Toc57154123)

[6.4.6   流控初始化协议违例... 189](#_Toc57154124)

[6.5    流控机制的介绍（Introduction to  Flow Control Mechanism）... 189](#_Toc57154125)

[6.5.1   整体说明（General）... 189](#_Toc57154126)

[6.5.2   流控相关元素（The Flow Control Elements）... 190](#_Toc57154127)

[6.5.2.1   发送方流控元素（Transmitter Elements）... 190](#_Toc57154128)

[6.5.2.2   接收方流控元素（Receiver Elements）... 191](#_Toc57154129)

[6.6    流控示例（Flow Control Example）... 191](#_Toc57154130)

[6.6.1   阶段一——初始化后的流控（Flow Control  Following Initialization）... 192](#_Toc57154131)

[6.6.2   阶段二——流控Buffer已满（Flow Control Buffer  Fills Up）... 194](#_Toc57154132)

[6.6.3   阶段三——计数器翻转为0（Counters Roll Over）... 195](#_Toc57154133)

[6.6.4   阶段四——流控Buffer溢出错误检查（FC Buffer Overflow  Error Check）... 195](#_Toc57154134)

[6.7    流控更新（Flow Control Update）... 196](#_Toc57154135)

[6.7.1   流控更新DLLP格式和内容（FC_Update DLLP Format and Content）... 197](#_Toc57154136)

[6.7.2   流控更新频率（Flow Control Update Frequency）... 198](#_Toc57154137)

[6.7.2.1   立即通告CREDITS_ALLOCATED（Immediate Notification of CA）... 198](#_Toc57154138)

[6.7.2.2   流控更新DLLP间最大延时（Maximum Latency Between UpdateFC）... 198](#_Toc57154139)

[6.7.2.3   基于Payload Size和链路宽度计算更新频率... 198](#_Toc57154140)

[6.7.3   错误检测计时器——一个软要求（Error Detection  Timer——A Pseudo Requirement）... 201](#_Toc57154141)

[Chapter 7  Quality of Service //QoS服务质量... 202](#_Toc57154142)

[关于上一章... 202](#_Toc57154143)

[关于本章... 202](#_Toc57154144)

[关于下一章... 202](#_Toc57154145)

[7.1    QoS的出发点（Motivation）... 202](#_Toc57154146)

[7.2    基本元素（Basic Elements）... 202](#_Toc57154147)

[7.2.1   流量类型TC（Traffic Class）... 203](#_Toc57154148)

[7.2.2   虚拟通道VC（Virtual Channels）... 204](#_Toc57154149)

[7.2.2.1   为每个VC分配TCs——TC/VC映射... 204](#_Toc57154150)

[7.2.2.2   确定要使用的VC的数量... 205](#_Toc57154151)

[7.2.2.3   分配VC ID.. 207](#_Toc57154152)

[7.3    VC仲裁（VC Arbitration）... 208](#_Toc57154153)

[7.3.1   整体说明（General）... 208](#_Toc57154154)

[7.3.2   严格优先级VC仲裁（Strict Priority VC Arbitration）... 209](#_Toc57154155)

[7.3.3   分组仲裁（Group Arbitration）... 210](#_Toc57154156)

[7.3.3.1   硬件固定仲裁方案（Hardware Fixed Arbitration Scheme）... 213](#_Toc57154157)

[7.3.3.2   加权轮询仲裁方案（Weighted Round Robin Arbitration Scheme）... 213](#_Toc57154158)

[7.3.3.3   建立虚拟通道仲裁表... 213](#_Toc57154159)

[7.4    端口仲裁（Port Arbitration）... 215](#_Toc57154160)

[7.4.1   整体说明（General）... 215](#_Toc57154161)

[7.4.2   端口仲裁机制（Port Arbitration Mechanisms）... 218](#_Toc57154162)

[7.4.2.1   硬件固定仲裁（Hardware-Fixed Arbitration）... 219](#_Toc57154163)

[7.4.2.2   加权轮询仲裁（WRR Arbitration）... 219](#_Toc57154164)

[7.4.2.3   基于时间的加权轮询仲裁（Time-Based WRR Arbitration，TBWRR）... 220](#_Toc57154165)

[7.4.3   载入端口仲裁表（Loading the Port Arbitration Tables）... 220](#_Toc57154166)

[7.4.4   Switch仲裁示例（Switch Arbitration  Example）... 221](#_Toc57154167)

[7.5    多功能 EP的仲裁（Arbitration in  Multi-Function Endpoints）... 223](#_Toc57154168)

[7.6    同步等时服务支持（Isochronous  Support）... 224](#_Toc57154169)

[7.6.1   时间规划就是一切（Timing is Everything）... 224](#_Toc57154170)

[7.6.1.1   如何定义时间规划（How Timing Defined）... 226](#_Toc57154171)

[7.6.1.2   如何实施时间规划（How Timing is Enforced）... 226](#_Toc57154172)

[7.6.2   软件支持（Software Support）... 226](#_Toc57154173)

[7.6.2.1   设备驱动程序（Device Drivers）... 227](#_Toc57154174)

[7.6.2.2   同步等时代理（Isochronous Broker）... 227](#_Toc57154175)

[7.6.3   将一切结合起来（Bringing it all together）... 227](#_Toc57154176)

[7.6.3.1   Endpoints. 227](#_Toc57154177)

[7.6.3.2   Switches. 228](#_Toc57154178)

[l   仲裁问题（Arbitration Issues）... 228](#_Toc57154179)

[l   时间规划问题（Timing Issues）... 229](#_Toc57154180)

[l   带宽分配问题（Bandwidth  Allocation Problems）... 230](#_Toc57154181)

[l   延时问题（Latency Issues）... 231](#_Toc57154182)

[7.6.3.3   Root  Complex. 232](#_Toc57154183)

[l   问题：窥探（Problem：Snooping）... 232](#_Toc57154184)

[l   窥探问题解决方法（Snooping  Solution）... 232](#_Toc57154185)

[7.6.3.4   电源管理（Power Management）... 232](#_Toc57154186)

[7.6.3.5   错误处理（Error Handling）... 232](#_Toc57154187)

[Chapter 8  Transaction Ordering //事务排序... 234](#_Toc57154188)

[关于上一章... 234](#_Toc57154189)

[关于本章... 234](#_Toc57154190)

[关于下一章... 234](#_Toc57154191)

[8.1    介绍（Introduction）... 234](#_Toc57154192)

[8.2    定义（Definitions）... 234](#_Toc57154193)

[8.3    简化后的排序规则（Simplified  Ordering Rules）... 235](#_Toc57154194)

[8.3.1   排序规则与流量类型（Ordering Rules and Traffic Classes）... 235](#_Toc57154195)

[8.3.2   基于包类型的排序规则（Ordering Rules Based On Packet Type）... 236](#_Toc57154196)

[8.3.3   简化后的排序规则表（The Simplified Ordering Rules Table）... 236](#_Toc57154197)

[8.4    生产者/消费者模型（Producer/Consumer  Model）... 237](#_Toc57154198)

[8.4.1   正确的生产者/消费者流程（Producer/Consumer Sequence-No Errors）... 238](#_Toc57154199)

[8.4.2   出错的生产者/消费者排序（Producer/Consumer Sequence-Errors）... 241](#_Toc57154200)

[8.5    宽松排序（Relaxed Ordering）... 243](#_Toc57154201)

[8.5.1   RO对MWr和Message的影响（RO Effects on MWr and  Message）... 243](#_Toc57154202)

[8.5.2   RO对MRd事务的影响（RO Effects on MRd  Transactions）... 244](#_Toc57154203)

[8.6    弱排序（Weak Ordering）... 244](#_Toc57154204)

[8.6.1   事务的排序与流控（Transaction Ordering and Flow Control）... 245](#_Toc57154205)

[8.6.2   事务暂停（Transaction Stalls）... 245](#_Toc57154206)

[8.6.3   VC  Buffer提供了一个优点（VC  Buffers Offer an Advantage）... 246](#_Toc57154207)

[8.7    基于ID的排序（ID Based Ordering，IDO）... 246](#_Toc57154208)

[8.7.1   解决方案（The Solution）... 246](#_Toc57154209)

[8.7.2   何时使用IDO（When to use IDO）... 247](#_Toc57154210)

[8.8    死锁的避免（Deadlock Avoidance）... 248](#_Toc57154211)

 

 

 

 

 

 



#            第一部分    Part One：The Big Picture大局总览

## Chapter 1      Background //背景

### 关于本章

为了建立理解PCI Express（PCIe）体系结构的基础，本章先回顾了早出现于PCIe的总线PCI（Peripheral Component Interface外设组件接口）的总线模型，并描述了他们的基本特征以及各自特点，接着讨论了从早期的并行总线模型迁移到PCIe使用的串行高速总线模型的出发点和动机。

### 关于下一章

下一章对PCIe体系结构进行了介绍，并打算作为一个“操作执行级（executive level）”的概述，它覆盖了高层体系结构的所有基础内容。它介绍了协议规范中给出的分层的PCIe端口设计方法，并描述了每个层级的职责作用。

### 1.1 引言

想要理解PCIe的首先一步，是要对PCIe所基于的先前的技术建立坚实的基础，而本章则将对它们的体系结构进行概述。对PCI已经比较熟悉的读者可以跳过本章去看下一章。如本章标题“背景”，本章仅意在给出一个简明的概述。对于关于PCI和PCI-X的更深更细节的内容，请参考MindShare的书籍《PCI System Architecture》以及《PCI-X System Architecture》。

这里可以通过举一个例子来说明为什么本章的背景介绍是对理解PCIe有帮助的，现在在PCIe上使用的软件（驱动操作）与当初在PCI上使用的软件大致相同。保持这种向后的兼容性，使得旧的设计迁移到新的设计（PCI到PCIe）时可以让软件修改的尽可能少、花费尽可能低的成本，这即是在鼓励就设计向新设计迁移。最终的结果就是，旧的支持PCI的软件在PCIe系统中的工作方式无需发生改变，而新的软件（PCIe的驱动软件）也将按照PCI一样的模型进行操作（例如对配置空间）。基于这样的原因以及其他的一些原因，理解PCI以及它的操作模型将有助于促进对PCIe的理解。

### 1.2 PCI与PCI-X

PCI（Peripheral Component Interface外设组件接口）总线，其被开发出来的时间为1990年代初，当时人们期望用它来解决PCs（personal computer个人电脑）的外设总线的一些缺点。而那个年代的这个领域的技术标准是IBM的AT总线（Advanced Technology），它也被其他供应商称为ISA总线（Industry Standard Architecture）。ISA总线是为286 16位计算机而设计的，它在286上也发挥了足够的性能，但随着新型的32位计算机及其外设们出现，新的需求也出现了：更高的带宽、更多更好的功能（例如即插即用）。除此之外，ISA总线使用的连接器是具有较多针脚数的大型连接器。PC供应商们认识到了需要做出一定的改变了，几种用于替代ISA总线的设计也被提出，例如IBM的MCA（Micro-Channel Architecture），EISA总线（Extended ISA，这是由IBM的竞争对手提出的），以及VESA总线（Video Electronics Standards Association，它由声卡供应商们为适配声卡而提出）。然而，所有的这些设计都具有一些使得它们无法被普及的缺点。最终，一个由PC市场的主要公司们共同联合建立的组织PCISIG（PCI Special Interest Group）开发出了PCI总线，并将其作为一种开放总线。在当时，PCI这种新型总线体系结构在性能上大大的优于ISA，而且它在每个设备内部新定义了一组寄存器，称之为配置空间（configuration space）。这些寄存器使得软件可以查看一个PCI设备内部所需的存储和IO资源，同时让软件为一个系统下的各个设备分配互不冲突的地址。这些特性：开放式设计、高速、软件可见性与可控性（software visibility and control），帮助PCI克服了限制ISA与其他总线的发展障碍，让PCI迅速的称为了PC中的标准外设总线。

几年之后，PCI-X（PCI eXtended）被开发出来，作为PCI体系结构的一种逻辑扩展（logical extension），并将总线性能提升了不少。我们将稍后再讨论PCI-X相较于PCI的改变，但是需提前一提的是PCI-X的一个主要设计目标就是保持与PCI设备的可兼容性，而且是软件和硬件的兼容性都要保证，这将使得从PCI迁移至PCI-X尽可能的简单。不久后，PCI-X 2.0版本将速率提升到了更高，原始数据速率达到了4GB/s。因为PCI-X保持了对PCI硬件上的向后兼容性，所以它依然是一个并行总线，并继承了与该总线模型相关的一些问题。有意思的事情来了，并行总线的有效带宽最终会达到一个实际的上限，然后就无法简单的继续提升。而PCISIG一直在探索如何将PCI-X的速率进一步提升，但最终这项努力还是被放弃了。这种速率上限，连同多针脚的连接方式，虽然是并行总线的缺点但是同时也提供了动力，推动了并行总线模型向新型的串行总线模型转型。

 

这些早期总线的定义被罗列在表 1‑1，它展示了随时间发展而变得更高的带宽与频率。表中有一点很有趣的需要注意，那就是时钟频率和总线上插板卡的插槽数量之间的相关性。这是由PCI的低功耗信号模型带来的，这意味着更高的总线频率需要更短的板上走线以及更少的总线负载（详情可见“Reflected-Wave Signaling”）。另外有趣的一点是，随着总线频率的增加，共享总线上允许的设备数量在减少。当PCI-X 2.0被引入使用时，想要达到它的高速率需要要求总线变为一个点到点的互连（point-to-point interconnect）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

表 1‑1 总线频率、带宽、插槽数量的对比

### 1.3 PCI基础(PCI Basics)

#### 1.3.1     基于PCI的系统的一些基础知识

图 1‑1展示了一个基于PCI总线的老系统结构。该系统包括一个北桥（North Bridge称为“北”是因为如果将图看做一张地图，它位于中央PCI总线的北方向），北桥是处理器与PCI总线之间的接口。与北桥相连的是处理器总线（processor bus）、系统存储总线（system memory bus）、AGP图像总线（AGP graphics bus），以及PCI总线。几个设备将会共享PCI总线，它们要么直接与总线相连，要么作为插入板卡插入到连接器上。在中央PCI总线的“南方”有一个南桥（South Bridge），它用于将PCI与系统外设相连接，这里的系统外设可以是为了使用旧的遗留设备而应用发展了多年的ISA总线。南桥通常也是PCI的中心资源区（central resource），它提供了一些系统信号诸如复位、参考时钟和错误报告操作。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

图 1‑1基于旧PCI总线的平台

#### 1.3.2     PCI总线发起方(Initiator)与目标方(Target)

在PCI层次结构中，总线上的每个设备（device）可以包含多达8个功能（function），这些功能都共享该设备的总线接口，功能编号为0到7（一个仅具有单功能的设备通常将被分配功能号0）。每个功能都能作为总线上事务（transaction）的目标方，而且它们中的大多数还可以作为事务的发起方。这样的发起方（称为总线主设备）有一对针脚（REQ#与GNT#，符号#表示他们为低有效信号），这一对信号用来处理共享PCI总线时的仲裁。如图 1‑2所示，请求信号（REQ#）有效时表示主设备需要使用总线，并将此信息告知总线仲裁器来与此时所有的其他请求一起进行评估仲裁，最终将得出下一时刻由哪一个设备来占用PCI总线。仲裁器通常位于桥（bridge）中，桥是一种在层次结构中位于总线之上的结构，它可以接收总线上所有事务发起方（总线主设备）所发出的仲裁请求。仲裁器将决定谁下一个占用总线，并将这个设备的Grant（GNT#）置为有效（asserts the Grant pin for that device）。根据协议，只要总线上的上一个事务结束且总线进入空闲状态时，对于任意设备，只要在此时看到自己的GNT#信号被置为有效，那么就可以认为它是下一个占用总线的主设备，它就可以开始发起它的事务。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

图 1‑2 PCI总线仲裁

#### 1.3.3     典型的PCI总线周期

图 1‑3展示了一次典型的PCI总线周期。PCI总线是同步总线，这意味着总线上的活动都发生在时钟边沿，因此将时钟信号放在图标的顶部，并将它的上升沿都用虚线标注出来，这是因为信号正是在这些虚线所表示的时刻被发出（driven out）或被采样（sampled）。下面对总线上发生了什么进行一个简明的描述。

\1.    在时钟沿1，FRAME#信号（此信号有效时被用来表示总线访问正在进行中）和IRDY#信号（Initiator ReaDY for data，发起方准备好数据）这两个信号都处于未有效状态，这表示总线处于空闲状态。与此同时，GNT#有效，这意味着总线仲裁器已经选择了该设备作为下一个事务发起方。

\2.    在时钟沿2，发起方将会把FRAME#信号置为有效，这表示一个新的事务已经被它发起。与此同时，它将把这个事务的地址与命令驱动到总线上。总线上其他所有的设备都将会把这些信息锁存起来，并将地址译码，查看自己是否与之相匹配。

\3.    在时钟沿3，发起方将IRDY#信号置为有效，这表示它已经准备好了进行数据传输。图中AD总线上的环形箭头符号表示这个三态总线（可以理解为inout型）正处于“转向周期”，这意味着这个信号的拥有者（三态总线上的信号此时是谁的）发生了变化（这里出现“转向周期turn-around cycles”是因为这是一个读事务；发起者发出地址信息和接收数据的信号引脚是相同的）。打开目标方（target）的缓冲buffer的时钟沿与关闭发起方缓冲buffer的时钟沿不能相同，因为我们想尽量避免两个buffer都在驱动同一个信号的可能性（即都往AD总线上驱动数据），哪怕只是很短的时间也不行。总线的争用将会损坏设备，因此，相反地，需要在前一个buffer关闭后的下一个时钟再打开一个新的buffer。在改变方向之前，每个共享信号都需要这样进行处理。

\4.    在时钟沿4，总线上的一个设备识别到了请求的地址与自己相匹配，于是便通过将DEVSEL#信号（device select）置为有效的方式对事务发出响应，并加入到事务的完成中去。与此同时，它将把TRDY#信号（Target ReaDY）置为有效，这表示它正在发送读数据的第一部分，并将数据驱动到AD总线上（放到总线上的时间可以后延，总线允许目标方在FRAME#有效与TRDY#有效间间隔最多16个时钟周期）。由于此时IRDY#与TRDY#都处于有效状态了，数据将在此上升沿开始传输，至此完成了第一个数据阶段（first data phase）。Initiator知道最终一共要传输多少字节的数据，但是Target并不知道。在事务的命令信息中并不包含字节数信息，因此每当完成一个数据阶段，Target都必须查看FRAME#信号的状态，以此来得知Initiator是否对传输的数据量满意（即是否达到Initiator需要的数据量）。如果FRAME#依然有效，那么说明这并不是最后一个数据阶段，Target仍然需要继续向Initiator传输数据，事务将按照相同的方式继续处理接下来传输的字节。

\5.    在时钟沿5，Target还没有准备好继续发送下一组数据，因此它将TRDY#置为无效。我们将这种情况称为插入了一个“等待状态”，这使得事务被延迟了一个时钟周期。Initiator和Target都可以进行这样的操作，它们最多可以将下一次数据传输后延8个连续的时钟周期。

\6.    在时钟沿6，第二组数据被传输，并且由于传输后FRAME#信号依然有效，Target就知道依然需要继续向Initiator传递数据。

\7.    在时钟沿7，Initiator强制总线进入了一个等待状态。在等待状态中，设备可以使当前事务暂停，并快速地将要发送的数据填充进buffer或是将接收到的缓存在buffer内的数据搬出，能进行这样的操作是因为Initiator和Target允许事务在暂停后直接恢复，而不需要进行中止和重启事务的操作。但是从另一方面来说，这样的操作将会十分低效，因为它不仅使当前事务停顿，同时也禁止其他设备访问和使用总线，尽管此时总线上没有数据在传输。

\8.    在时钟沿8，第三组数据被传输，并且此时FRAME#信号被置为无效，因此Target得知这就是最后一次数据传输。因此，在这一个时钟周期后，所有的控制信号线都关闭，总线再次进入了空闲状态。

 

在PCI总线上，许多信号都具有多种含义，这是为了减少引脚数量，以此来保持PCI设计中的低成本这一设计目标。数据和地址信号一起复用32bit总线，C/BE#（Command/Byte Enable）信号也共享他们的4个信号引脚。尽管减少引脚数量是一种可行的方法，并且它也是PCI使用“转向周期”这种会增加延时的方法的原因。但是这种复用引脚的做法阻止了对事务的流水线操作（在发送上一个周期的数据的同时发送下一个周期的地址）。握手信号诸如FRAME#、DEVSEL#、TRDY#、IRDY#以及STOP#被用来控制在事务（transaction）进行期间各事件（event）何时进行。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

图 1‑3简单的PCI总线传输

#### 1.3.4     反射波信号传输(Reflected-Wave Signaling)

PCI体系结构支持每路总线上最多挂载32个设备，但是由于实际的电气上限要小得多，在33MHz的基频下大约可以有10到12个电气负载。出现这种现象是由于总线使用了一种技术称为“反射波信号传输”，该技术可以降低总线上的功率损耗（如图 1‑4）。在这个模型中，设备通过实现弱传输缓冲buffer（weak transmit buffer）来达到节省成本与功耗的目的，这种方法可以仅需一半的驱动电压即可完成信号电平的翻转。信号的入射波沿着传输线传输下去，直到到达传输线的末端。按照设计，虽然到了传输线的末端，但是信号传输并没有就此中止，波阵面（wavefront）在遭遇传输线末端的无穷大阻抗后会被反射回来。这种反射本质上是叠加性的，当信号返回发射方时，它会与入射信号叠加使信号增加到全电压电平。当这样的反射信号到达初始的buffer时，buffer驱动器的低输出阻抗会中止这个信号的传输以及停止其继续反射。因此，从buffer发出一个信号一直到接收方检测到有效的信号所花费的时间，就等于信号沿线路传输的时间加上反射回来的延迟时间和建立时间。所有这些都必须小于一个时钟周期。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

图 1‑4 PCI的反射波信号传输

当走线长度和总线上的电气负载数量增加时，信号往返所需要的时间也增加。一个33MHz的PCI总线只能满足最多10-12个电气负载的信号时序要求。一个电气负载指的是系统板上安装的一个设备，但是实际上一个插了板卡的连接器插槽被算作两个电气负载。因此，如表 1‑1所示的那样，一个33MHz的PCI总线最多只能有4或者5个插入板卡的连接器，否则将无法保证可靠性。

为了在一个系统中接入更多的负载，需要使用PCI-to-PCI bridge，如图 1‑5所示。当出现了越来越多更为先进的主芯片组后，外设的发展也十分迅速，以至于竞争访问共享PCI总线变成了限制外设性能的原因。PCI总线的速率没有跟上外设速率增长的脚步，虽然它依旧是主流的外设总线，但是已经成为了系统的性能瓶颈。这个问题的解决办法为，把PCI移到系统外设与存储器之间的主路径之外，将芯片组互连用一些专有的解决方案来替代（例如Intel的Hub Link Interface）。

PCI Bridge是拓扑结构的延伸。每个Bridge都能产生新的PCI总线，而且这种由PCI Bridge产生的PCI总线与上层PCI总线在电气上是相隔绝的，因此它也可以独立的提供额外的10-12个电气负载。PCI总线上的一些设备也可以作为Bridge，可以令较多的设备接入系统中。PCI体系结构允许在单系统中存在256路总线，每路总线下最多可以挂载32个设备。

 

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

图 1‑5 包含PCI-to-PCI bridge的33MHz PCI系统

### 1.4 PCI总线体系结构透视探究(PCI Bus Architecture Perspective)



#### 1.4.1     PCI事务模型

PCI同先前的总线模型一样，在数据传输上使用三种模型：Programmed I/O（PIO）、Peer-to-peer、以及DMA。这些模型的图解如图 1‑6所示，接下来的几个小节将对它们进行描述。

##### 1.4.1.1   Programmed I/O(PIO)

PIO在早期PC中被广泛使用，因为设计者不愿意增加设备中由于事务管理逻辑而带来的成本开销以及额外增加的复杂度。在当时，处理器比任何其他设备工作的都要快，所以在这个模型中，处理器包揽了所有的工作。举例来说，当一个PCI设备向CPU发出中断，并表示它需要向memory中放入数据，则由CPU最终将数据从PCI设备中读出并放入一个内部寄存器中，然后再将这个内部寄存器的值复制进memory中。反之，如果数据要从memory移动到PCI设备中，那么软件会指示CPU将memory中的数据读出至内部寄存器中，然后再将内部寄存器的值写入到PCI设备中去。

这样的操作流程虽然可行，但是效率却比较低，其主要有两个原因。第一个原因，每一次数据传输都需要CPU产生两个总线周期。第二个原因，在数据传输过程中CPU都要忙于数据传输而不能做其他更有意义的任务。在早期，这是一种最快速的传输方式，而且单任务处理器也没有其他的任务要做。然而这种低效的方式显然不适用于更为先进的系统中，因此这种方式已不再是数据传输的常用方式，取而代之首选方法的是下一节将讲述的DMA方法。然而为了使软件与设备交互，Programmed IO仍然是一种必要的事务模型。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

图 1‑6 PCI事务模型

##### 1.4.1.2   Direct Memory Access(DMA)

在传输数据的方法上，一种更为高效的方式成为DMA（Direct Memory Access，直接内存访问）。在这个模型中有另一种设备称为DMA Engine（DMA引擎），由它来代表处理器来负责与掌握从memory向外设进行数据传输的各种细节操作，这就将这个繁琐的任务从CPU卸载了下来。一旦CPU将起始地址（starting address）和数据字节总量（byte count）写入DMA Engine，DMA Engine将自己处理总线协议与地址排序。这样的做法对PCI外设来说并不引入新的变化，可以让外设们保持它们的低成本设计目标。后来，经过集成性改进，外设自身可以集成入这种DMA功能，这样它们就不再需要一个外部的DMA Engine。这些设备有能力处理自己发起的总线传输，我们将它们称为总线主设备（Bus Master device）。

图 1‑3就是一个PCI总线上的总线主设备正在进行事务的过程。北桥可以对地址进行译码，以此来识别自己是否是这个事务的Target，比如北桥译码后发现地址与自己相匹配则认为自己是这个事务的Target。在总线周期中的数据传输阶段，数据在总线主设备与北桥之间传输，北桥即是数据传输的Target。北桥将对数据依次产生DRAM总线周期以进行与系统memory的通信，而PCI外设也可以产生一个中断来通知系统。DMA这种数据传输方法将效率提升了很多，因为这种方式在搬移数据时不需要CPU的参与，那么对于CPU来说只需要一个总线周期即可完成高效的数据块搬移（move a block of data），即只需把起始地址和总数据量写入DMA Engine即可。

##### 1.4.1.3   Peer-to-Peer(点对点)

如果一个设备能够作为总线主设备，那么它就提供了一个有趣的作用。一个PCI总线Master可以发起针对其他PCI设备的数据传输，而对于PCI总线自身来说这整个事务都是在本地进行的，并未因为如任何其他系统的资源。因为这个事务是在总线上的两个设备之间进行的，且这两个设备被认为是总线中两个对等的节点，因此这个事务被称作一个“点对点”的事务。这种事务具有明显的高效性，因为系统中其余的部分仍然可以自由的完成其他的任务。然而，在实际场景中点对点事务很少被使用，这是因为Initiator和Target通常不会使用相同的数据格式，除非二者都是由一个供应商制造的。因此，数据一般必须要首先发送到memory，这样CPU就可以在数据传输到Target之前对其进行格式转换，而这就阻碍并破坏了点对点传输的设计目标。

#### 1.4.2     PCI总线仲裁

思考图 1‑2，因为当今的PCI设备基本都能作为总线Master，它们都可以进行DMA与peer-to-peer的数据传输。在像PCI这种共享总线的体系结构中，各设备需要轮流占用总线，因此当一个设备想要发起事务时必须首先向总线仲裁器请求总线所有权（ownership）。仲裁器将查看当前所有的请求，并使用一些特定的仲裁实现算法来决定哪个Master可以下一个占用总线。PCI协议规范并没有描述这个仲裁算法，但是有声明这个仲裁必须是“公平”的，不得在访问中差别对待任何一个设备。

仲裁器可以在上一个占用总线的Master还正在进行数据传输时就决定出下一个占用总线的设备，这样总线上就不需要引入额外的时延来对下一个总线所有者进行排序。因此，总线仲裁器的仲裁作用发挥在“幕后”，被称为“隐藏”的总线仲裁，这是一种对早期总线协议的改进。

#### 1.4.3     PCI低效的地方(PCI Inefficiencies)

##### 1.4.3.1   PCI重试协议(PCI Retry Protocol)

当一个PCI Master发起一个事务，用于访问一个Target设备，而此时Target设备并未处于Ready状态，那么Target就会给出一个事务重试的信号，这种场景的示意图如图 1‑7。

 

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)

图 1‑7 PCI事务重试机制

考虑接下来给出的例子：北桥发起一个读memory事务，希望从以太网设备中读取数据。这个以太网设备Target响应这个事务，请求并参与到总线周期中来。然而，这个以太网设备Target此刻并没有立即将数据返回给北桥Master。这个以太网设备此时有两个办法来推迟这次数据传输。第一个办法就是在数据传输中插入等待态（wait-state）。如果过程中仅需插入少量的等待态，那么数据的传输还能算是比较有效率。但是如果Target设备需要延迟更多的时间（从事务被发起后延迟16个时钟周期以上），那么就需要用第二种办法了，第二种办法就是Target发出一个STOP#信号来表示事务重试（retry）。Retry操作是通知Master在数据没有传输前就提前结束总线周期。这样做可以防止总线长时间处于等待状态而降低了总线效率。当Master接收到Target发来的Retry请求，Master至少等待2个时钟周期，然后必须重新向总线发起仲裁请求，当再次获得总线的使用权之后重新发起相同的总线周期。在当前总线Master正在处理Retry的这段时间中，仲裁器可以将总线使用权授予其他的Master，以便于PCI总线能被更有效率的利用起来。当此前执行Retry的Master再次占用总线并重新启动与此前相同的总线周期时，Target可能也已经准备好了需要传输的数据，并参与到总线周期中进行数据传输。否则，若Target仍然未准备好，那么它将会再次发起Retry，按照这样的流程循环下去直到Master成功完成了数据传输。

##### 1.4.3.2   PCI断开连接协议(PCI Disconnect Protocol)

当一个PCI Master发起一个事务来访问一个Target，并且如果这个Target可以传输至少一个双字（doubleword，后面使用dw简写）的数据，但是它无法完成整个完整的全数据量的传输，那么它将会在它无法继续进行传输时断开与事务操作的连接。这种场景的示意图如图 1‑8所示。

考虑接下来给出的例子：北桥发起一个突发读memory事务（burst memory read），希望从以太网设备中读取数据。以太网设备Target请求并参与到这个总线周期中来，传输了一些数据，但是随后不久把已有的所有数据都发送光了，却依然没有达到Master需要的数据总量。以太网设备Target此时有两个选择来延迟数据的传输。第一个选择是在数据传输阶段插入等待态，然后自身也同时在等待收到新增的数据。如果过程中仅需插入少量的等待态，那么数据的传输还能算是比较有效率。但是如果Target设备需要延迟更多的时间（PCI协议规范中允许在数据传输中途最多有8个时钟周期的等待态，这里区别于上一节Retry中的16个周期，那是因为那时传输还没开始，而这8个周期针对的是传输已经开始了一段时间的传输中途等待），Target必须发出断开连接的信号。要断开连接，Target需要在总线周期运行中将STOP#置为有效，这样就能告知Master提前结束总线周期。Disconnect与Retry的一个区别就在于，Disconnect有数据已经被传输，而Retry则是数据传输根本就没开始。Disconnect这种操作也让总线避免长时间处于等待态。Master至少等待2个时钟周期，然后才能再次向总线发起仲裁请求，当再次获得总线的使用权之后就可以再次访问刚才断开连接的设备地址，继续完成此前断开未完成的总线周期。在当前总线Master正在经历Disconnect的这段时间中，仲裁器可以将总线使用权授予其他的Master，以便于PCI总线能被更有效率的利用起来。当此前经历Disconnect的Master再次占用总线并准备继续此前未完成的总线周期时，Target可能也已经准备好了需要继续传输的数据，并参与到总线周期中继续完成数据传输。否则，若Target仍然未准备好，那么它将会再次发起Retry或者Disconnect，然后按照这样的流程循环下去直到Master成功完成了所有数据的传输。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image018.jpg)

图 1‑8 PCI事务断开连接机制

#### 1.4.4     PCI中断处理(PCI Interrupt Handling)

PCI设备使用4个边带信号（sideband）作为中断信号，分别为INTA#、INTB#、INTC#、INTD#，并从中选取一个来向系统发送中断请求，即使用4个中断信号中的1个来发送中断请求。当其中一个中断引脚被置为有效时，单CPU系统的中断控制器将会对中断作出响应，相应的方式为将INTR（interrupt request）信号置为有效，将中断请求发送给CPU。后来出现的多CPU系统不再适用这种单信号线输入作为中断的方式，因此进行了改进，将中断改为了APIC（Advanced Programmable Interrupt Controller）模型，在这种模型中中断控制器将会向多CPU发送报文（message）而不是向其中一个CPU发送INTR信号。不过无论中断传输模型是什么，接收到中断的CPU都必须确认中断来源，然后为中断提供服务。传统的模型需要好几个总线周期来完成确认中断和服务中断的操作，效率不是很高。APIC模型会比传统模型好一些但是也依然存在改进的空间。

#### 1.4.5     PCI错误处理(PCI Error Handling)

PCI设备可以在事务进行器件有选择的检测并报告地址与数据的奇偶校验错误。在事务进行期间，PCI通过使用PAR信号来在绝大多数信号上产生偶校验位。这表示如果传输的数据或者地址信息中为逻辑电平1的bit数量是奇数，那么就将PAR信号置为1，以此来使得“1”的数量为偶数个。Target设备在接收到数据或地址时将会进行校验检查错误。奇偶校验（在这里为偶校验）的方法仅在有奇数个信号出错时才能检测到。如果设备检测到数据的奇偶校验错误，它将把PERR#（parity error）置为有效。这有可能是一个可以恢复的错误，因为例如对于memory读取来说，只需要再次发起相同的事务就可以解决问题。然而，PCI自身并不包含任何的自动的或是基于硬件的错误恢复机制，因此软件将完全负责去尝试解决错误。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image020.jpg)

图 1‑9 PCI错误处理

然而，上面所说的是数据出错的情况，若地址校验出错则情况不同。在图 1‑9的例子中，地址信息损坏，这导致一个Target匹配上了这个错误的地址。我们无法得知损坏的地址信息变成了什么，也无法得知总线上哪个设备匹配上了这个错误的地址，所以对于这种情况就不存在能够简单进行错误恢复的方法。因此，这种类型的错误将会导致SERR#（system error）被置为有效，然后通常会使得系统错误处理程序被调用。在老式机器中，为了预防引起更进一步的错误，将会强行让系统停止工作，从而导致出现“蓝屏死机”。

#### 1.4.6     PCI地址空间映射(PCI Address Map)

PCI体系结构支持3种地址空间，如图 1‑10所示，包含：Memory（内存）、I/O、Configuration Address Space（配置地址空间）。x86处理器可以直接访问Memory和I/O空间。一个PCI设备映射到处理器内存地址空间，可以支持32或64位寻址。在I/O地址空间，PCI设备可以支持32位寻址，但是因为x86 CPU仅使用16位的I/O地址空间，许多平台都会将I/O空间限制在64KB（对应16bit）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image022.jpg)

图 1‑10地址空间映射

PCI还引进了第三种地址空间，称为配置空间，CPU只能对它进行间接访问而非直接访问。PCI设备中的每个Function（功能）都包含专为配置空间所准备的内部寄存器，这些寄存器对于软件来说是可见的，并且软件可以通过一个标准化的方式来控制它们的地址与资源，这为PC提供了一个真正的即插即用（plug and play）的环境。每个PCI Function最多可以有256 Bytes的配置地址空间。考虑到PCI最多可支持单个设备含有8个Function、每路总线含有32个设备、单个系统包含256路总线，那么可以得到一个系统的配置空间总量为256B/Function *8Function/device * 32devices/bus *256buses/system =16MB，即一个系统的配置空间总大小为16MB。

因为x86 CPU无法直接访问配置空间，所以它必须通过IO寄存器进行索引（然而在PCI Express中引入了一种新方法来访问配置空间，这种新方法是通过将配置空间映射入内存地址空间来完成的）。在传统模型中，如图 1‑10所示，使用了一种被称为配置地址端口（Configuration Address Port）的IO端口，它位于地址CF8h-CFBh；还使用了一种被称为配置数据端口（Configuration Data Port）的IO端口，它位于地址CFCh-CFFh。关于通过这种方法以及通过内存映射方法访问配置空间的细节将会在下一节进行解释。

#### 1.4.7     PCI配置周期的生成(PCI Configuration Cycle Generation)

由于IO地址空间的大小是有限的，传统模型的设计上对地址十分保守。在IO空间中常见的做法是令一个寄存器来指向一个内部位置，然后用另一个寄存器来进行数据的读取或写入。PCI的配置过程包含如下两步。

l 第一步：CPU产生一个IO写，写入的位置为北桥中IO地址为CF8h的地址端口（Address Port），这样就给出了需要被配置的寄存器的地址，即“用一个寄存器来指向一个内部位置”。如图 1‑11所示，这个地址主要由三部分组成，通过这三部分就可以定位一个PCI Function在拓扑结构中的位置，它们为：在256条总线中我们想访问哪一条总线、在该总线上的32个设备中访问哪一个、在该设备的8个Function中访问哪一个。除了这些以外，唯一还需要提供的信息是要确认访问这个Function的64dw（256Bytes）中的哪个dw。

l 第二步：CPU产生一个IO读或者IO写，操作的位置为北桥中的地址为CFCh的数据端口（Data Port），即“用另一个寄存器来进行数据的读取或写入”。在此基础上，北桥向PCI总线发起一个配置读事务（configuration read）或配置写事务（configuration write），事务要操作的地址即为步骤一中地址端口中所指定的地址。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image024.jpg)

图 1‑11配置地址寄存器

#### 1.4.8     PCI Function配置寄存器空间(PCI Function Cfg Reg Space)

没个PCI Function可以包含最多256Bytes的配置空间。这个配置空间的首64Bytes包含一个被称为Header（配置空间头）的结构，剩余的192Bytes用来支持一些其他可选功能。系统配置首先由Boot ROM固件来执行，在操作系统被加载完成后，它将重新对系统进行配置，重新进行资源分配。这也就是说系统配置的过程可能会被执行两次。

根据Header的类型，PCI Function被分为两个基本的类别。第一种Header称为Type 1 Header（类型1），它的结构如图 1‑12，它用于标识这个Function是一个Bridge，Bridge将会在拓扑结构上创建另一条总线。而Type 0 Header（类型0）就是用来指示这个Function不是一个Bridge（如图 1‑13）。关于Header类型的信息包含在dword3的字节2的同名字段中（Class Code字段），当软件在系统中发现某个Function时，第一件事就是要检查其Header的这个字段（软件发现系统中Function的过程称为枚举enumeration）。

关于配置寄存器空间以及枚举过程的更为详细的讲解将会稍后再进行。在这里我们只是希望你先熟悉一下各个部分是如何相互配合工作的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image026.jpg)

图 1‑12 PCI配置Header Type 1（Bridge）

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image028.jpg)

图 1‑13 PCI配置Header Type 0（非Bridge）



 

#### 1.4.9     更高带宽的PCI(Higher-bandwidth PCI)

为了支持更高的带宽，PCI协议规范更新成了支持位宽更宽（64bit）时钟速率更快（66MHz）的版本，使其可以达到533MB/s的速率。图 1‑14展示了一个使用66MHz 64bit PCI总线的系统。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image030.jpg)

图 1‑14 基于66MHz PCI总线的平台

##### 1.4.9.1   66MHz PCI总线的限制

66MHz总线的吞吐量相较于33MHz已经翻倍，但是其存在一定的问题，图 1‑14显示了它的一个主要缺点：66MHz总线与33MHz总线使用相同的反射波开关模型，使得留给在传输线上信号传输的时间减半，这将会大大的降低总线的负载能力。最终造成的结果是，一条总线上只能有一个插入板卡。增加更多的设备意味着需要增加更多的PCI Bridge来产生更多的总线，这将会增加成本以及对PCB板材的要求。同时，64bit PCI总线相较于32bit增加了引脚数，这也会增加系统成本且降低可靠性。将上述结合起来，就可以很容易看出来为什么这些因素限制了64bit或者66MHz版本的PCI总线的广泛使用。

##### 1.4.9.2   并行PCI总线模型在66MHz以上时出现的信号时序问题

鉴于PCI总线上的实际负载以及信号渡越时间（signal flight times），PCI总线的时钟频率无法在66MHz上继续增加了。对于66MHz时钟来说，其时钟周期为15ns。接收器的建立时间为3ns。因为PCI使用的“非寄存输入（non-registered input）”信号模型，想要将其3ns的建立时间继续减小是不现实的。剩余的12ns的时间预算分配给了发送器的输出延迟以及信号传输时间。总线上传输的信号将会因为无法及时被接收而使得其无法在接收端被正确采样。

在下一节将会介绍PCI-X总线，它将所有的输入信号都先用触发器来寄存一下，然后再去使用。这样做可以将信号的建立时间降低到1ns以下。在建立时间上的优化使得PCI-X总线可以跑到一个更高的频率，例如100MHz甚至133MHz。下一节中我们将会对PCI-X总线体系结构进行简明的介绍。

### 1.5 PCI-X简介

PCI-X对PCI的软件和硬件都能做到向后兼容，同时PCI-X还能提供更好的性能和更高的效率。它和PCI所使用的连接器也是相同的，因此PCI-X与PCI的设备可以插入相互的插槽。除此之外，它们二者还使用相同的配置模型，因此在PCI系统中能使用的设备、操作系统、以及应用，在PCI-X系统中仍然可以使用。

为了在不改变PCI信号传输模型的基础上达到更高的速率，PCI-X使用了几个技巧来改善总线时序。首先，他们实现了PLL（锁相环）时钟发生器，利用它在内部提供相移时钟。这使得输出可以提前一点驱动出去，而输入可以稍晚一点进行采样，从而改善总线上的时序。同时，PCI-X的输入信号都在Target设备的输入引脚被寄存（锁存），这样就使得建立时间更小。通过这些方法节省出来的时间可以有效增加信号在总线上传输的可用时间，并使得总线可以进一步提高时钟频率。

#### 1.5.1     PCI-X系统示例(PCI-X System Example)

如图 1‑15所示，这是一个基于Intel 7500服务器芯片组的系统平台。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image032.jpg)

图 1‑15基于66MHz/33MHz PCI-X总线的平台

MCH（Memory Controller Hub）芯片提供了3个额外的高性能的Hub-Link 2.0端口，它们各与一个PCI-X 2 Bridge（P64H2）相连接。每个Bridge支持两条PCI-X总线，总线最高时钟频率可以达到133MHz。Hub Link 2.0可以支撑PCI-X更高带宽的数据流。需要注意的是，在PCI-X上我们依然存在与当初改进66MHz PCI时遇到的一样的负载方面的问题，这使得如果我们要支持更多的设备那么就需要使用Bridge来生成更多的总线，并且这将会是一个相对成本昂贵的解决方案。不过，现在的带宽确实提高了不少。

#### 1.5.2     PCI-X事务(PCI-X Transactions)

如图 1‑16所示，这是一个PCI-X总线上Memory突发读取（burst memory read）事务。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image034.jpg)

图 1‑16 PCI-X Memory突发读取的总线时序周期示例

需要注意的是，PCI-X并不允许在第一个数据阶段（first data phase）后插入等待态。之所以这样做是因为PCI-X中会将需要传输的数据总量告诉Target设备，这个信息将会在事务的属性阶段（Attribute Phase），因此Target是知道自己需要传输多少数据的，这里就与PCI不同了。此外，多数PCI-X总线时序周期是使用突发模式传输的，并且数据通常以128Bytes作为一个数据块（block）来进行传输。这些特性使得总线利用率提高，同时也提高了设备buffer管理的效率。

#### 1.5.3     PCI-X特性(PCI-X Features)

##### 1.5.3.1   拆分事务模型（Split-Transaction Model）

在传统的PCI读事务中，总线Master向总线上某个设备发起读取。如前面的内容所述，若Target设备未准备好，无法完成事务，那么它既可以选择在获取数据的同时让总线保持等待态，也可以发起Retry来推迟事务。

PCI-X则不同，它使用拆分事务的方法来处理这些情况，如图 1‑17所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image036.jpg)

图 1‑17 PCI-X 拆分事务处理协议

为了方便描述追踪每个设备正在做什么，我们现在将例子中发起读取操作的一方称为Requester（请求方），将完成读取请求提供数据的一方称为Completer（完成方）。如果Completer无法马上对请求做出相应的服务，它将把这个事务的相关信息存储起来（地址、事务类型、总数据量、Requester ID），并发出拆分响应信号。这就告诉Requester可以将这个事务先放置在队列中，并结束当前的总线时序周期，释放总线使之回到空闲状态。这样，在Completer等待所请求的数据这段时间里，总线上可以执行其他的事务。Requester在这段时间里也可以自由的做其他事情，比如发起另一个请求，甚至是向此前的那个Completer发起请求都可以。一旦Completer收集到了所请求的数据，它将会申请总线占用仲裁，当获取到总线的使用权后，它将发起一个拆分完成（Split Completion）来返回此前Requester所请求的数据。Requester将会响应并参与拆分完成这个事务的总线时序周期，在这个过程中接收来自Completer的数据。对于系统来说，拆分完成事务其实与写事务非常相似。这种拆分事务模型（Split Transaction Model）是可行的是因为在请求发起时就通过属性阶段（Attribute Phase）指出了总共需要传输多少数据，并且也告诉了Completer是谁发起了这个请求（通过提供Requester自己的Bus:Device:Function号码），这使得Completer在发起拆分完成事务时能够找到正确的目标。

对于上述的整个数据传输过程来说，需要通过两个总线事务来完成，但是读请求和拆分完成这两个事务之间的这段时间里，总线是可以执行其他任务的。Requester不需要重复的轮询设备来检查数据是否已经准备好。Completer只需要简单的申请总线占用仲裁，然后在能使用总线时将被请求的数据返回给Requester即可。就总线利用率而言，这样的操作流程使得事务模型更加高效。

到目前为止，对PCI-X所做的这些协议增强改进使得PCI-X的传输效率提高至约85%，而使用标准PCI协议则为50%-60%。

##### 1.5.3.2   MSI消息式中断（Message Signaled Interrupts）

PCI-X设备需要支持MSI中断（Message Signaled Interrupts），在传统中断体系结构中经常见到多个设备需要共享中断，开发这种功能是为了减少或者消除这种需要。想要产生一个MSI中断请求，设备需要发起一个memory写事务，其操作地址为一段预先定义好的地址区域，在这个地址区域内的一个写入操作就会被视为是一个中断，这个信息将会被传送给一个或多个CPU，写入的数据则是发起中断的设备的中断向量，这个中断向量在系统中是唯一的，仅指向这一个设备。对于CPU来说，拥有了中断号之后就可以迅速的跳转到对应设备的中断服务程序，这样就避免了还需要花费额外的开销来查找是那个设备发起的中断。此外，这样的中断方式就不需要额外的中断引脚了。

##### 1.5.3.3   事务属性（Transaction Attributes）

最后，我们来介绍一下PCI-X在每个事务开始时新加入的一个阶段，称之为属性阶段（如图 1‑16所示）。在段时间中，Requester会将所发起事务的一些信息传递给Completer，信息包括请求的总数据量大小、Requester是谁（Bus:Device:Function 号码），这些信息可以有效的让总线上的事务执行提升效率。除了这些以外，还新引入了2bit的信号来描述所发起的事务：“No Snoop（无窥探）”位与“Relaxed Ordering（宽松排序）”位。

l **No Snoop****（NS****）：**一般来说，当一个事务要把数据移入或者移出Memory时，CPU内部的Cache需要检查被操作的Memory区域中是否存在已经被拷贝到Cache中的部分。若存在，那么Cache需要在事务访问Memory之前就把数据写回Memory，或是把Cache内的数据按失效处理。当然，这个窥探Memory内容有无出现在cache中的过程将会消耗一定的时间，并且会增加这个请求的延迟。在有些情况下，软件是知道某个请求的Memory位置永远不会出现在Cache中（这可能是因为这个位置被系统定义为不可缓存的），因此对于这些位置是不需要进行窥探的，应该跳过这一步骤。No Snoop位正是基于这种原因而被引入进来。

l **Relaxed Ordering****（RO****）：**通常情况下，当事务通过Bridge的buffer时，事务间需要保持当初被放置到总线上时的顺序。这被称为强排序模型（Strongly ordered model），PCI与PCI-X一般都会遵循这种规则，除了一些个别情况。这是因为强排序模型可以解决有相互联系的事务之间的依赖问题，例如对同一个位置先进行写入再进行读取，按照强排序模型就可以使得读取操作读取到写入后的正确数据，而不是写入前的旧数据。然而实际上并不是所有的事务都存在依赖关系。如果有些事务不存在依赖关系，那么依然强制它们保持原来的顺序将会造成性能损失，这就是新添加这一位想解决的问题。如果Requester知道某个特定的事务是与此前其他事务不相关联的，那么就可以将这个事务的这一位置为有效，这样就能告诉Bridge可以让这个事务在队列中往前移动提前执行，以此带来更好的性能。

#### 1.5.4     更高带宽的PCI-X(Higher Bandwidth PCI-X)

##### 1.5.4.1   PCI与PCI-X 1.0并行总线模型中由于公共时钟方法所带来的问题

当尝试将PCI这样的总线升级到更高的速率时，一个问题变得越来越明显，那就是并行总线设计存在一些固有的先天局限。图 1‑18可以有助于理解这个问题。这些设计通常使用公共时钟（common clock）或者是分布式时钟（distributed clock），其中，发送方在公共时钟的某一个时钟沿将数据输出，然后接收方在公共时钟的下一个时钟沿将数据锁存，也就是说用于传输的时间预算长度就是一个时钟周期。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image038.jpg)

图 1‑18并行设计的固有问题

需要注意的第一个问题是信号歪斜（signal skew）。当数据的多个bit在同一时刻被发出，他们所经历的延时会存在轻微的差别，这使得他们到达接受伐的时刻也会存在轻微的差别。如果某些原因造成这种差别过大，那么就会出现图中所示的信号采样错误的问题。第二个问题是多个设备间的时钟歪斜问题。公共时钟到达各个设备的时间点并不是十分精确一致的，这也会大大的压缩了用于传输信号的时间预算长度。最后，第三个问题与一个信号从发送方传输到接收方所需要的时间有关，我们称这个时间为渡越时间（flight time）。时钟周期或是传输时间预算必须要大于信号渡越时间。为了确保满足这样的条件，PCB硬件板级设计需要让信号走线尽可能短，以此来让信号传输时延小于时钟周期。在许多电路板设计中，这种短信号走线的设计并不现实。

尽管存在这些自身局限，但是为了更进一步提升性能，还是有几项技术可以使用的。首先，可以对现有的协议进行简化而提高效率。第二，可以将总线模型更改为源同步时钟模型（source synchronous clocking model），在这种时钟模型中，总线信号和时钟（strobe选通脉冲）被驱动后将经历相同的传输延迟到达接收端（这里可以对比一下common clock与其的区别）。这种方法被PCI-X 2.0所采用。

##### 1.5.4.2   PCI-X 2.0源同步模型（PCI-X 2.0 Source-Synchronous Model）

PCI-X2.0更进一步的提高了PCI-X的带宽。如此前升级时一样，PCI-X 2.0依然保持了对PCI的软件与硬件的向后兼容性。为了达到更高的速率，总线使用了源同步传输模型，来支持DDR（双倍数据速率）或是QDR（四倍数据速率）。

“源同步”这个术语的意思是，设备在传输数据的同时，还提供了额外一个与数据信号基本传输路径相同的信号。如图 1‑19所示，在PCI-X 2.0中，那个“额外”的信号被称为“strobe（脉冲选通）”，接收方使用它来锁存发来的数据。发送方可以分配data与strobe信号之间的时序关系，只要data与strobe信号的传输路径长度相近且其他会影响传输延迟的条件也相近，那么当信号到达接收方时，这种信号间的时序关系也将不会发生变化。这种特点可以用于支持更高速率的传输，这是因为在源同步时钟模型中，接收方采样所使用的strobe（类似于原来的时钟）也是数据的来源方发送的，这样就避免了公共时钟到达各设备的延时不同而造成的时钟歪斜问题，因为如上所述此时的data与strobe之间的时序关系是基本相对不变的，这样就将时钟歪斜的这段时间单独剔除了出去，不再占用原来的传输时间预算。除此之外，因为存在了strobe信号来随路指示接收方何时锁存采样数据，所以信号渡越时间不再是个问题，即使走线较长也依然能由伴随data的strobe信号来让接收方正确的采样数据，而不必纠结在下一个“时钟上升沿”必须要进行采样，现在strobe何时指示则在何时采样。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image040.jpg)

图 1‑19源同步时钟模型

需要再次强调的是，这种非常高速的信号时序使得共享总线模型无法被使用，它只能被设计为点对点的传输形式。因此，想要增加更多的设备就需要更多的Bridge来产生更多的总线。为了达到这样的使用方法，可以设计一种设备，它拥有3个PCI-X 2.0接口，其内部还有一个Bridge结构以此来让他们三者进行相互通信。不过，这样的设备将会具有很高的引脚数量，而且成本也会更高，这使得PCI-X 2.0被困在了高端市场而无法大面积普及。

由于协议制定者认识到了PCI-X 2.0的昂贵解决方案仅能吸引更多的高端开发者而不是一般开发者，因此PCI-X 2.0提供了更符合高端设计的技术标准，例如支持ECC的生成与校验。PCI-X的ECC比PCI中使用的奇偶校验要更为复杂，鲁棒性（robust）也要高得多，它可以动态的自动纠正单bit错误，以及对多bit错误进行可靠的检测。这种改进后的错误处理方法增加了成本，但是对于高端平台来说，它所能增加的可靠性才是更为看中的，因此即使增加了成本这也是一个合理的选择。

尽管PCI-X 2.0将带宽、效率以及可靠性都提升了，但是并行总线的终究还是走到了它的终点，需要一种新的模型来满足人们对于更高带宽且更低成本的不断的需求。最终被选中的新型总线模型使用的是串行接口，从物理层面来看它与PCI是完全不同的总线，但是它在软件层面保留了对PCI的向后兼容性。

这个新模型，就是我们所知道的PCI Express（PCIe）。



## Chapter 2      PCIe Architecture Overview //PCIe体系结构概述

### 关于前一章

前一章节为我们提供了PCI技术发展的历史，以此来建立更好的理解PCIe的基础。它主要了回顾PCI与PCI-X1.0/2.0的基础内容，目的是为了给接下来对PCIe的概述内容提供一些内容上的前因后果，更方便对PCIe进行理解。

### 关于本章

本章节对PCI Express体系结构进行了全面的介绍，旨在将本章作为一个“执行层”概述，涵盖该体系结构在高层的所有基础知识。它介绍了PCIe协议规范中给出的分层的方法，并描述了每个层级的职责作用。介绍了各种数据包类型的同时也一起介绍了用于通信和增强传输可靠性的协议。

### 关于下一章

下一章节介绍了PCI Express环境中的配置部分。它包括用于实现Function的配置寄存器的空间，一个Function如何在总线上被发现，配置事务是如何被生成和路由到正确的位置，PCI兼容空间与PCIe扩展空间之间的差异，以及软件是如何区分Endpoint（EP，端点）和Bridge。

### 2.1 PCI Express简介

PCI Express的出现代表了其前身并行总线的重大转变。作为一种串行总线，它与早期的串行设计（例如InfiniBand或者Fibre Channel）有许多的共同点，但是它仍然保持了在软件层面对PCI的向后兼容。

正如许多高速串行传输方法一样，PCIe使用双向连接方式，可以在同一时间同时进行信息的收发操作（即区分开了TX、RX，而不是像PCI一样在同一根线上作为inout）。这种模型被称为双单工连接（Dual-simplex connection），因为每个接口都有一个单工发送路径和一个单工接收路径，图 2‑1展示了这种模型。因为数据流可以同时进行双向传输，因此在技术层面上来说两个设备间的通信其实是全双工的，但是PCIe协议规范依然使用双单工这个术语，这是因为这种称呼对实际通信信道也进行了一点描述，即通信信道由两个方向的各自的单工信道组成。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image042.jpg)

图 2‑1双单工链路

用于描述设备之间信号传输路径的术语为“链路（Link）”，它由一个或以上的接收发送对组成。这样的一对接收和发送被称为一个“通道（Lane）”，协议规范允许一条链路内有1、2、4、8、12、16或32个Lane。链路内Lane的数量称为链路宽度，通常用x1、x2、x4、x8、x16以及x32来进行表示。用于权衡在实际设计中使用多少Lane的思路其实很简单：使用更多的Lane可以增加带宽，但是也会增加成本、增加空间占用以及增加功耗。更多关于这方面的信息，可以阅读“Links and Lanes（链路与通道）”这一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image044.jpg)

图 2‑2一个Lane（通道）

#### 2.1.1     软件的向后兼容性（Software Backward Compatibility）

PCIe设计目标中极为重要的一点就是要保持对PCI软件的向后兼容性。如果一个设计使用的是现有的系统，而且这个设计已经能够在这个现有系统中正常工作，那么想要促使它转向使用另一种系统则需要满足两件事：第一，新技术要有足够吸引人的性能提升，第二，尽可能减小从来技术迁移到新技术时的成本、风险以及工作量。对于第二点来说，在计算机中通常的做法是让那些为旧模型所编写的软件在新模型中依然可用。为了在PCIe中做到这一点，PCI中所用到的所有的地址空间要么不做更改便直接照搬，要么仅仅进行了了简单的扩展。Memory、IO和配置空间对于软件来说依然是可见的，而且连写入方式都和以前一样。因此就算是数年前为PCI而写的软件（BIOS代码、设备驱动等等）将依然可以在现在的PCIe设备上使用。配置空间已经被大大的扩展，添加了许多新的寄存器来支持新的功能，但是老的寄存器依然存在且可以按照常规方式来访问他们（这部分的详细内容见“Software Compatibility Characteristic软件兼容性特性”）。

#### 2.1.2     串行传输（Serial Transport）

##### 2.1.2.1   对传输速率的需求（The Need for Speed）

很明显，串行传输模型必须要比并行的设计跑的快很多才能达到相同的带宽，这是因为串行传输一次只发送1bit数据。然而，事实证明这并不困难，过去的PCIe在2.5GT/s和5.0GT/s都能稳定的工作。之所以PCIe可以达到这样的速率，甚至达到更高的8GT/s，是因为串行传输模型克服了并行模型的不足。

l **克服问题**

通过回顾并行总线的内容我们知道，它的性能被一些问题所限制，如图 2‑3展示了其中的三个问题。首先回想一下，并行总线使用公共时钟（common clock）；信号在一个时钟沿被输出，然后在下一个时钟沿被接收方接收。这个模型的第一个问题来自于信号从发送端传输到接收端所花费的时间，称为渡越时间。渡越时间必须小于一个时钟周期，否则将会出问题，这使得难以通过继续减小时钟周期来提升速度。因为若需要继续减小时钟周期，为了让信号渡越时间依然小于时钟周期，需要布线更短并减少负载的设备数量，但是最终这样的做法都会到达极限，在实际情况下变得无法达到。第二个造成并行模型性能受限的因素是使用公共时钟时，时钟到达发送方和接收方的时刻不一致，这称为时钟歪斜。PCB布线设计人员尽力去减小时钟歪斜的值，因为时钟歪斜将会降低信号传输时间预算，但是这种歪斜永远无法彻底消除。第三个因素是信号歪斜，它指的是多bit位宽数据的各个bit到达接收端的时刻存在差异。显然，这样的多bit位宽数据在所有的bit都到达且稳定之前都不能被接收方采样，这使得我们必须去等待最慢的那一bit。

 

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image046.jpg)

图 2‑3 并行总线的局限

PCIe这样的串行传输方法是如何处理这些问题的呢？首先，信号渡越时间将不再是一个问题，因为用于指示接收端锁存数据的时钟现在已经被内置入数据流中，不在需要额外传输线来传输参考时钟。因此，无论再小的时钟周期或是再长的信号传输时间都不会产生以前的问题了，这是因为内置在数据流中的时钟必然能与数据一起到达接收方。同样地，时钟歪斜的问题将不再存在，这还是因为时钟被内置入数据流中，接收方通过恢复出数据流中的时钟来进行数据采样，自然不存在时钟歪斜的问题。最后，信号歪斜的问题在一个Lane（通道）内被消除了，因为一个Lane一次只传输1bit数据。在多通道设计中，虽然信号歪斜的问题再次出现了，但是接收端将自动对其进行纠正，它可以修复大量歪斜。虽然串行设计克服了并行模型的许多问题，但是串行设计自身也有一系列的复杂问题。不过，我们稍后将看到，对这些问题的解决方法是易于控制的，并可以让我们做到高速的、可靠的通信。

l **带宽**

PCIe支持的高速且含有多Lane（通道）的链路带来了傲人的带宽数值，如表 2‑1所示。这些数字都来源于比特率与总线特性。其中一种特性与其他许多串行传输方法类似，那就是PCIe的前两代版本中使用了被称为8b/10b的编码过程，这种编码过程会根据8bit的输入而生成10bit的输出。尽管这样做会引起一些开销，但是有几个很好的理由支持我们这样做，我们将会在后面讲到。对于现在来说，只需要知道对于8b/10b编码来说，要发送1Byte的数据实际需要传送10bit。第一代PCIe（称为Gen1或者PCIe协议规范版本1.x）中，比特率为2.5GT/s，将它除以10即可得知一个Lane（通道）的速率将可以达到0.25GB/s。因为链路可以在同一时刻进行发送和接收，因此聚合带宽可以达到这个数值的两倍，即每个Lane（通道）达到0.5GB/s。第二代PCIe（称为Gen2或者PCIe 2.x）中将总线频率翻倍，这也使得它的带宽相较于Gen1翻倍。第三代PCIe（称为Gen3或者PCIe 3.0）再次让带宽翻倍，但是这次协议的制定者们并没有选择将频率翻倍。相反，出于一些原因（我们稍后将会进行讨论），他们仅将频率提升到8GT/s（Gen2中是5GT/s，未翻倍），并不再采用8b/10b的编码方式，而是采用了另一种编码机制，即128b/130b编码（关于这个内容的更多信息，可以阅读“Physical Layer-Logical（Gen3）”章节）。表 2‑1列出了当前几代PCIe的各种Lane（通道）数量情况下的带宽，展示出了链路对应情况下的峰值吞吐量。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image048.jpg)

表 2‑1 PCIe Gen1,Gen2,Gen3的各种链路宽度下的带宽汇总

##### 2.1.2.2   PCIe带宽计算方法

要计算上述表格中的PCIe带宽大小，可以参照如下的计算方法。

n Gen1 PCIe带宽=（2.5Gb/s x 2 directions）/10bits per symbol=0.5GB/s

n Gen2 PCIe带宽=（5.0Gb/s x 2 directions）/10bits per symbol=1.0GB/s

需要注意，上述计算中，我们是除以10bit而不是8bit（1Byte），这是因为Gen1和Gen2的协议中要求将Byte进行8b/10b编码后封装为10bit，然后才进行数据包的传输，因此原数据中的1Byte在实际传输时其实是需要传输10bit。

n Gen3 PCIe带宽=（2.0Gb/s x 2 directions）/8bits per byte=2.0GB/s

注意到在Gen3速率的计算中，我们除以的是8bit而不再是10bit了，这是因为Gen3中并不再使用8b/10b编码方式，而是128b/130b编码方式。这种编码方式每128bit引入2bit开销，这个开销非常小以至于我们暂且可以将它在我们的计算中忽略。

上述三种方法计算出来的带宽只需要再乘以链路宽度即可得到整个多Lane（通道）链路的链路带宽。

##### 2.1.2.3   PCIe的差分信号

每个通道都使用差分信号进行传输，差分信号是指每次传输一个信号时同时发送它的正信号和负信号（D+和D-，这两种信号振幅相同相位相反），如图 2‑4所示。当然，这样会将引脚增加一倍，但是相对于单端信号而言，差分信号在高速传输上的两个明显的优点足以抵消其引脚数方面的不足：它提高了噪声容限，并降低了信号电压。

差分信号的接收端将会接收这一对相位相反的信号，用正信号的电压减去其反相信号的电压，得到它们的差值，以此来判定这1bit的逻辑电平值。差分传输设计内置了抗噪声干扰的设计，因为它要求成对的差分信号必须位于每个设备的相邻的引脚上，它们的走点也必须彼此非常靠近，以保持合适的传输线阻抗。因此，任何因素在影响差分对中的一个信号的时候，都会同等程度且同样方向地影响到另一个信号。但是接收端所在意的是它们的差值，而这些噪声干扰并不会改变这个差值，所以带来的结果就是大多数情况下噪声对信号的影响并不会引起接收端对bit的正确判别。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image050.jpg)

图 2‑4 差分信号示意图

##### 2.1.2.4   不再使用公共时钟

在先前的内容中有提到，PCIe链路不再像PCI一样使用公共时钟（common clock），它使用了一个源同步模型（source-synchronous）,这意味着需要由发送端给接收端提供一个时钟来用于对输入数据进行锁存采样。对于PCIe链路来说，并不含有转发时钟（forwarded clock）。相反地，发送端会将时钟通过8b/10b编码来嵌入数据流中，然后接收端将会从数据流中恢复出这个时钟，并用于对输入数据进行锁存。这一过程听起来可能非常神秘，但是其实很简单。在接收端中，PLL电路（Phase-Locked Loop锁相环，如图 2‑5）将输入的比特流作为参考时钟，并将其时序或者相位与一个输出时钟相比较，这个输出时钟是PLL按照指定频率产生的时钟。也就是说PLL自身会产生一个指定频率的输出时钟，然后用比特流作为的参考时钟与自身产生的输出时钟相比较。基于比较的结果，PLL将会升高或者降低输出时钟的频率，直到所比较的双方达到匹配。此时则可以称PLL已锁定，且输出时钟（恢复时钟recovered clock）的频率已经精确地与发送数据的时钟相匹配。PLL将会不断地调整恢复时钟，因此由温度、电压因素对发送端时钟频率造成的影响都会被快速的补偿修正。

关于时钟恢复，有一件需要注意的事情，PLL需要输入端的信号跳变来完成相位比较。如果很长一段时间数据都没有任何跳变，那么PLL的恢复时钟可能会偏离正确的时钟频率。为了避免这种问题，8b/10b编码中的设计目标之一就是要确保比特流中连续的1或者0的数量不能超过5个（想获得更多关于这部分的内容，请参考“8b/10b Encoding”一节）

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image052.jpg)

图 2‑5简单的锁相环图示

一旦时钟被恢复出来，就可以使用它来锁存输入数据流的bit，并将锁存到的结果给到解串器（deserializer）。有时候学生们可能想知道这个恢复时钟能否作为接收端的所有逻辑所使用的工作时钟，但是这个问题的答案是“不行”。一个原因是，接收端不能指望用于恢复出时钟的比特流参考时钟一直存在且活跃，因为当链路的低功耗状态就包括了停止数据传输，此时必然也就无法继续恢复时钟。因此，接收端必须要有自己本地生成的内部时钟。

##### 2.1.2.5   基于数据包的协议体系（Packet-based Protocol）

将并行传输转变为串行传输可以极大的减少数据传输需要的引脚数量。如其他大多数串行传输协议一样，PCIe通过消除了绝大部分原来并行总线中常用的边带控制信号（side-band control signal）来减少了引脚数量。然而，如果没有控制信号来指示被接收的信息的类型，接收端如何知道输入的bit是什么信息呢？因此，在PCIe中，所有的事务在发送时都使用已经定义好的结构，称为数据包。接收端需要找到数据包的边界（即包头和包尾），并且知道这个数据包的被定义的结构是什么样子的（即数据包的模板），然后解析数据包结构来获知它需要执行什么操作。

关于数据包协议体系的详细信息将在“TLP Elements”这一章进行全面讲解，但在本章也可以找到对各种包格式的介绍以及他们各自用途的概述，请见“Data Link Layer”一节。

#### 2.1.3     链路和通道（Links and Lanes）

如先前的内容所提到，两个PCIe设备之间的物理连接被称为一条链路（Link），这个链路由许多通道（Lane）所构成。如图 2‑2，每个通道都含有一对发送差分信号对和一对接收差分信号对。这样的一个通道已经足够用于设备之间的通信，不需要其他额外的信号。

##### 2.1.3.1   可扩展的性能（Scalable Performance）

尽管一个通道（Lane）就可以满足两个设备之间通信的需求，但是使用多的通道的链路可以提升链路的传输性能，这个性能取决于链路的传输速率以及链路宽度。例如，使用多通道链路可以增加每个时钟传输的bit数量，因此这样就可以提升带宽。如表 2‑1所提到，PCIe协议规范支持一条链路中含有的通道数目为2的幂，最多32通道。除了2的幂以外，x12链路也是支持的，这可能是为了支持InfiniBand的x12链路，它是一种较早的串行设计。PCIe这种允许多种链路宽度的特性，使得平台设计者可以在成本和性能之间做出适当的权衡，根据链路中通道的数量轻松地进行增减。

##### 2.1.3.2   灵活的拓扑结构选择（Flexible Topology Options）

一条PCIe链路必须是一个点对点的连接，而不是像PCI一样的共享总线，这是因为PCIe使用的链路速率非常高。由于一条链路只能连接两个接口，因此需要一种扩展连接的方法来构建一个不琐碎的系统（non-trivial system），这里不琐碎的意思是指不过于细碎和冗杂，例如若直接对所有设备都采用直接的两两相连，那么会使得整个系统十分冗杂琐碎。这种需求在PCIe中通过Switches（交换结构）和Bridge来实现，这两者可以灵活的构建系统拓扑——系统中元素之间的连接集。对系统中一个元素的定义以及一些拓扑结构的例子将在下面的小节中给出。

#### 2.1.4     关于PCIe拓扑的一些定义

如图 2‑6展示了一个简单的PCIe拓扑示例，它将有助于我们对本节的一些定义内容进行讲解。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image054.jpg)

图 2‑6 PCIe拓扑示例

##### 2.1.4.1   拓扑特征（Topology Characteristics）

在图的最上方是一个CPU。这里需要指出，CPU被认为是PCIe层次结构的顶端。就像PCI一样，PCIe只允许简单的树结构，这意味着不允许出现循环或者其他复杂的拓扑结构。这样做的原因是为了保持与PCI软件的向后兼容性，因为PCI软件使用简单的配置方法来记录拓扑结构，它并不支持复杂的环境。

为了保持这种向后兼容性，软件必须能够以与从前一样方式产生配置周期，并且总线拓扑结构也必须与以前一样。因此，软件希望找到的所有配置寄存器依然存在，而且它们的行为方式等也依然与从前相同。我们将稍晚些在回过头来讨论这一块的内容，因为我们要先定义一些术语概念。

##### 2.1.4.2   Root Complex（RC 根组件）

CPU与PCIe总线们支架可能包含一系列的组件（处理器接口，DRAM接口等等），甚至是包含多个芯片。将这些组件合起来，称这一组组件为Root Complex（根组件，简称为RC或者Root）。RC存在于PCI倒转的树状拓扑的“根部”，并代表CPU与系统的其余部分通信。但是，PCIe协议规范并没有很仔细的对RC进行定义，而是给出了一个RC必需功能与可选功能的列表。从广义上说，Root Complex可以被理解为系统CPU与PCIe拓扑之间的接口，这个PCIe端口，即RC，在配置空间中被标记为“根端口（Root Port）”。

##### 2.1.4.3   Switches and Bridge（交换结构与桥）

Switch提供了扇出以及聚合能力，使得单个PCIe端口上可以连接更多的设备。它扮演数据包路由器的角色，可以根据所给数据包的地址或者其他路由信息来识别这个数据包要走哪条路径。

Bridge提供了一个通往其他总线的接口，例如PCI或者PCI-X，甚至也可以是其他的PCIe总线。图 2‑6中展示的Bridge有时被称为“forward bridge（前向Bridge）”，它使得一个旧的PCI或者PCI-X板卡可以插入一个新系统中（PCIe系统）。与forward bridge相反类型的Bridge称为“reserve bridge（反向Bridge）”，它使得新的PCIe板卡可以插入到旧的PCI系统中。

##### 2.1.4.4   Native PCIe Endpoints and Legacy PCIe Endpoints（原生与遗留EP）

Endpoints（EP，端点）是PCIe拓扑中的既不是Switch也不是Bridge的设备，它们可以作为总线上事务的Initiator也可以作为事务的Completers。EP存在于树状拓扑的分支的底部，仅实现一个上行端口（面向Root），这里的上行指的是拓扑结构的向上，表示EP已经是树的分支的端点，不再产生下级分支。相比之下，一个Switch可以有几个下行端口。设计用于老式总线（例如PCI-X）的设备如今拥有了可供它们使用的PCIe接口，这种PCIe接口将在配置寄存器中将自身标识为“Legacy PCIe Endpoints（遗留PCIe端点）”。它们使用了在PCIe设计中被禁止的东西，例如IO空间、支持IO事务以及支持锁定请求。与之相反，“Native PCIe Endpoints（原生PCIe端点）”是从一开始就被设计用来在PCIe系统中使用的，这区别于在旧的PCI设备上添加PCIe接口。原生PCIe端点设备是内存映射设备（MMIO设备）。

##### 2.1.4.5   软件兼容特性

一种保持软件兼容性的方法是保持EP和Bridge的配置头（configuration header）与PCI一致，如图 2‑7所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image056.jpg)

图 2‑7 配置头

有一点不同的是在现在PCIe中Bridge经常会被聚合进Switch和Root，但是遗留的PCI软件是无法发现这种区别的，它只把这些内含Bridge的设备简单的当做Bridge。在这里我们先对一些概念进行熟悉，所以我们不会在这里讨论这些寄存器的细节。配置是一个相当大的主题，关于它的具体介绍将在“Configuration Overview”这一节进行。

为了举例说明PCIe系统在软件中的展现形式，我们可以参考图 2‑8中所展示的拓扑结构的例子。像之前所说的一样，RC位于整个层次结构的顶端。RC自身的内部可以非常复杂，但是它通常会实现一个内部总线结构以及一些桥接结构，以便将拓扑扇出到几个端口。RC的内部总线将被配置软件视作PCI总线0，且RC上这几个PCIe端口将被视作是PCI-to-PCI Bridge。这种内部结构其实并不是一个实际的PCI总线，但是它在软件中看起来就是这种结构。因为这个总线是在RC内部的，所以它实际的逻辑设计并不需要遵循任何的标准，这里是可以由供应商来进行自定义的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image058.jpg)

图 2‑8拓扑结构示例

类似的，PCIe Switch（交换结构）的内部结构对于软件来说就是简单地几个Bridge共享一条公共总线，如图 2‑9所示。这样做的主要优点就是使得事务的路由方法能够与PCI一致。枚举，这是配置软件用来发现系统拓扑结构并分配总线号和系统资源的过程，它的工作方式也与PCI中相同。在稍后的内容中我们将通过一些例子来讲述枚举是如何工作的。一旦枚举完成，系统中的总线号就将按照图 2‑9所示的方式进行分配。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image060.jpg)

图 2‑9系统枚举结果的示例

##### 2.1.4.6   系统示例

如图 2‑10中举例说明了一个基于PCIe的系统，它被设计用于一些低成本应用比如消费品台式计算机。有一些PCIe端口实现时附带了一些板卡插槽，但是基本的框架与老式的PCI系统并没有太大区别。

相比之下，如图 2‑11所示的高端的服务器系统展示了一些内置在系统的用于连接其他网络的接口。在早期PCIe中，有人曾考虑将其作为一个网络来运行，用来取代那些旧的模型。毕竟，如果PCIe在大体上是其他网络协议的简化版本，或许它也能满足所有需求。由于各种各样的原因，这一概念从来没有真正得到大力发展，基于PCIe的系统通常仍然需要使用其他的协议来连接到外部网络。

这也给了我们一个机会来重新审视一个问题，RC是由什么组成的。在这个例子中，被标识为“英特尔处理器”的方块包含了许多组件，大多数现代的CPU架构都是如此。这个处理器包含了一个用于访问图形设备（例如显卡）的x16 PCIe端口，以及两条DRAM通道，这两条DRAM通道意味着内存控制器以及一些路由逻辑已经被集成到CPU封装（CPU Package）中。总的来说，这些资源通常被称为“非核心”资源，这样的称呼用于将他们与CPU封装中的几个CPU核心（CPU Core）区分开来。此前，我们描述过Root是CPU与PCIe拓扑相连接的接口，这意味着在CPU封装中必须含有Root的一部分。正如图 2‑11中虚线所框出的，Root由多个封装的一部分共同组成。这种Root的组成方式可能将会是未来很长一段时间内的系统的设计方式。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image062.jpg)

图 2‑10 低成本的PCIe系统

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image064.jpg)

图 2‑11服务器PCIe系统

 

### 2.2 设备层次介绍（Introduction to Device Layers）

PCIe定义了一种分层的体系结构，如图 2‑12是这种分层体系结构的示意图。我们可以认为这些层在逻辑上是相互独立的部分，因为他们各自都有一个用于发出信息流的发送端和一个接收信息流的接收端。这种分层的设计方法对硬件设计者来说有不少的优点，因为如果在设计中对逻辑进行了仔细的划分，那么就可以在以后升级到新的协议规范版本时仅改变原设计中的某一层即可，而不会影响或者变动其它层。虽然如此，但是需要注意的是，这些层只是定义了接口的工作职责，而实际设计并不要求为了符合规范而将设计按照层级来分成几个部分，即分层的概念更偏重于在逻辑层面，不强制要求划分的很严格。本节的目的在于描述各个层的功能职责，以及描述完成一次数据传输时的事件流程。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image066.jpg)

图 2‑12 PCI Express设备层次示意图

图 2‑12所展示的PCIe设备内部层次包括：

l 设备核（Device Core）以及它与事务层（Transaction Layer）的接口。设备核实现设备的主要功能。如果设备是一个EP（端点），那么它最多可以包含8个Function（功能），每个Function实现自己的配置空间。如果设备是一个Switch（交换结构），那么它的Switch核是由数据包路由逻辑和为了实现路由的内部总线而构成。如果设备是一个RC（Root根设备），那么他的Root核会实现一个虚拟的PCI总线0，在这个虚拟的PCI总线0中存在着所有的芯片组嵌入式EP，以及存在着虚拟Bridge。

l 事务层（Transaction Layer）。事务层负责在发送端产生Transaction Layer Packet（TLP，事务层包），在接收端对TLP进行译码。这一层也负责QoS功能（Quality of Service，服务质量）、流量控制功能（Flow Control）以及事务排序功能（Transaction Ordering）。所有的这四个事务层的功能将在本书的第二部分Part Two进行讲解。

l 数据链路层（Data Link Layer）。数据链路层负责在发送端产生Data Link Layer Packet（DLLP，数据链路层包），在接收端对DLLP进行译码。这一层也负责链路错误检测以及修正（Link error detection and correction），这个数据链路层功能被称为Ack/Nak协议。这两个数据链路层功能会在本书的第三部分Part Three进行讲解。

l 物理层（Physical Layer）。物理层负责在发送端产生Ordered-set Packet（命令集包），在接收端对Ordered-set进行译码。这一层将处理上述三种类型的包（TLP、DLLP、Ordered-set）在物理链路上的发送与接收。数据包在发送端要经过byte striping logic（字节条带化逻辑，主要在多lane时体现作用）、Scrambler（扰码器）、8b/10b encoder（对于Gen1/Gen）或是128b/130b编码器（对于Gen3）以及数据包并串转换模块的处理。最终数据包以训练后的链路速率在所有Lane（通道）上按照时钟以差分形式输出。在物理层的接收端，数据包的处理包括串行的接收差分信号所传输的经过编码的bit，将其转换为数字信号形式，然后将输入比特流做串并转换。完成这个操作（对输入数据的采样）所基于的时钟是来源于CDR（Clock and Data Recovery，时钟数据恢复）电路所提供的恢复时钟。接收下来的数据包要经过elastic buffers（弹性缓存）、8b/10b解码器（对于Gen1/Gen2）或者128b/130b解码器（对于Gen3）、de-scramblers（解扰器）以及byte un-striping logic（字节反条带化逻辑）。最终，物理层的LTSSM（Link Training and Status State Machine，链路训练与状态 状态机）负责进行链路初始化以及训练。所有这些物理层功能将在本书的第四部分Part Four进行讲解。

每个PCIe接口都支持这些层的功能，包括Switch端口，如图 2‑13所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image068.jpg)

图 2‑13 Switch端口的层次

在早期大家经常会产生一个疑问，那就是一个Switch端口是否还需要实现所有的层呢，毕竟它通常只用于转发数据包。答案是需要，因为对包的内容进行解析来确定它们的路由是需要查看数据包的内部细节的，而这件事情是在事务层（Transaction Layer）中做的。

原则上，本设备上的每个层都与链路对端设备的各自对应层进行通信，例如本设备的事务层与对端的事务层通信、本设备的数据链路层与对端的数据链路层通信。上面的两层通过在数据包内添加一串特定的bit信息，产生一个接收端的对应层可以识别的字段样式来实现对应层之间的通信。数据包通过其他层的转发，从而做到到达或者离开链路。物理层也直接与另一个设备中的物理层通信，但是方式与此不同。

在我们讲的更深入之前，让我们先大致了解一下这些层是如何交互的。从广义上说，设备所发出的请求包或者完成包是在事务层进行组包的，组包所用到的信息是由device core所提供的，有时我们将device core称为Software Layer软件层（尽管协议规范中并没有使用这一术语）。它提供的组包信息通常包括期望的命令类型、目标设备的地址、请求的属性特征等等。刚组好的数据包会被存入一个buffer称为Virtual Channel（虚拟通道），直到这个包可以被发往下一个层级。当这个包被向下发给数据链路层后，数据链路层会在数据包中加入额外的信息以供对端的接收方进行错误检查，而且这个数据包会在本地储存下来，这样我们就可以在对端检测到传输出错时重新发送这个数据包。当数据包到达物理层后，它被编码，并使用链路上的所有可用的通道、以差分信号的形式进行传输。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image070.jpg)

图 2‑14 PCIe设备层次的详细框图

接收端对物理层的输入bit进行解码，检查本层级所能发现的错误，如果没有检查到错误那么就将接收下来的数据包向上转发给数据链路层。在数据链路层，数据包进行不同于物理层的错误检查，如果没有发现错误那么就向上转发给事务层。这个数据包在事务层被缓存和错误检查，并被拆包拆成各原始信息（命令、属性等等），这样就可以这些原始信息内容呈交给device core，这里的device core是这次数据传输的接收端设备的device core。接下来，让我们更深入的探索每一个层需要怎么做才能完成上述这样的操作过程，这个操作过程在图 2‑14进行了展示。我们从最顶端开始。

#### 2.2.1     设备核/软件层（Device Core/Software Layer）



Device Core是一个设备的核心功能，例如网络接口或是硬盘驱动控制器。它并不是PCIe协议规范中所定义的一个层级，但是我们可以把它当做一个PCIe的层级，这是因为它位于事务层的上方，而且它是所有请求（Request）的源头或是目的地。它为事务层提供了需要发送的请求信息，其中的信息包括事务类型、地址、需要传输的数据量等等。当事务层接收到输入数据包时，它也是事务层向上转发输入数据包信息的目的地。

#### 2.2.2     事务层（Transaction Layer）

为了响应（response）来自软件层的请求（request），事务层生成出站数据包（outbound packet）。它也会检查入站数据包（inbound packet），并将入站数据包内包含的信息向上转发给软件层。事务层支持non-posted（非投递式）请求的拆分事务协议（Split Transaction Protocol，前面在PCI-X的介绍中提到过），并将入站完成包（Completion）与先前传输的出站non-posted请求包关联起来，即知道这个完成包是对应到哪个non-posted请求包。事务层所处理的事务使用的数据包种类为TLP（Transaction Layer Packet），TLP可以分为四个请求种类：

\1.    Memory

\2.    IO

\3.    Configuration

\4.    Message

前三种（Memory、IO、Configuration）在PCI和PCI-X中就已经得到支持，但是Message是PCIe中的一个新的请求种类。一个请求包向目标设备传送命令，目标设备作为响应请求而发回的一个或多个完成包，这二者组合起来就是对一个事务的定义，即一个事务由一个请求包以及所有返回的完成包共同组成。如表 2‑2列出了PCIe请求的类型。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image072.jpg)

表 2‑2 PCIe请求类型

这些请求还可以被归为两类，如上表的右边一列：**non-posted**和**posted**。对于non-posted请求，Requester首先向Completer发送一个请求数据包，Completer应该产生一个完成包作为响应。读者们应该认出来了，这就是从PCI-X那里继承来的拆分事务协议（split transaction protocol）。例如，所有的读请求都是non-posted的，因为这个读请求所请求的数据需要通过完成包来返回给Requester。可能令你感到出乎意料，IO写请求和Configuration写请求也是non-posted的。尽管它们在发送给目标设备的命令中就已经包含了要写入目标设备的数据，但是这两种请求依然需要目标设备在写入完成后返回完成包，以此来让Requester确认数据被正确无误的写入到目的设备中。

与上述的请求相反地，Memory Write请求（Mwr）和Message请求都是posted的，这意味着Completer在完成这些请求后不需要向Requester返回完成包TLP。Posted事务对整体性能提升是有好处的，因为Requester不需要等待响应，也不需要承担对完成包进行处理的额外负担。这里做出的取舍就是Requester无法得到写请求是否被正确无误的完成的反馈信息。Posted这种操作行为继承自PCI，它依然被认为是一个不错的操作方法，虽然无法得到反馈信息，但是发生错误的可能性比较小并且使用posted得到的性能提升也比较明显。需要注意的是，尽管它们要求返回完成包，Posted写操作仍然要参与数据链路层的Ack/Nak协议，以此来保证较为可靠的数据包传输。关于这一点的更多内容，请见Chapter 10“Ack/Nak Protocol”。

##### 2.2.2.1   TLP基础内容

PCIe请求包和完成包的包类型被罗列在表 2‑3中。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image074.jpg)

表 2‑3 PCIe TLP类型

TLPs起始于发送方的事务层，终止于接收方的事务层，如图 2‑15所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image076.jpg)

图 2‑15 TLP的起点与目的地

当TLP途经发送方的数据链路层以及物理层时，这两层分别会向数据包中添加一些信息，接收方的数据链路层和物理层会分别根据发送方对应层所添加的信息来进行校验，以此确认数据包是否在链路传输中依旧保持正确没有出错。

###### l TLP组包（TLP Packet Assembly）

如展示了一个封装完成的TLP的各个部分是如何在链路上进行传输的，我们可以从中发现这个数据包中的不同部分是分别由不同的层来添加的。为了更容易看出这个数据包是怎么构成的，我们将TLP的不同部分用不同的颜色进行标识，以此来表示对应的部分是由哪一层添加的：红色代表事务层，蓝色代表数据链路层，绿色代表物理层。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image078.jpg)

图 2‑16 TLP的组包形式

Device Core负责将组成TLP的核心部分的信息发送给事务层，即上图中的Header和Data。每个TLP都会有一个Header（头部），而有些TLP会没有数据部分，例如读请求。事务层还可以选择添加ECRC（End-to-End CRC）字段，这个ECRC由事务层进行计算并附加在数据包的后面。CRC的意思是Cyclic Redundancy Check/Code（循环冗余校验/循环冗余校验码），它被几乎所有的串行传输架构所使用，原因很简单，它实现起来比较简单同时又能提供很强的错误检测功能。CRC还可以检测“突发错误（burst error）”，即一串重复的错误bit，这一串bit的长度取决于CRC的长度（对于PCIe来说是32bits）。因为这种类型的错误在发送一长串bit时会遇到，因此CRC的这种特性非常适用于串行传输。ECRC字段区域在通过发送方和接收方之间的任何服务点（service point，通常指的是Switch或者Root端口这些有TLP路由功能的地方）时都不改变，这使得目的端可以用它来验证在整个传输过程中都没有发生错误。

现在说说TLP的传输，TLP的核心部分由事务层转发至数据链路层，数据链路层负责在TLP中添加一个序列号（Sequence Number）和另外一个CRC字段区域称为链路CRC（LCRC，Link CRC）。LCRC被对端接收方用来进行错误检查，并将链路上传输的每个数据包的检查结果都汇报给发送方。善于思考的读者可能会有疑问，如果LCRC已经证明了这次链路传输是无差错的，而LCRC又是数据包必需含有的字段，那么ECRC还有什么作用呢？这个疑问的回答是，还使用ECRC是因为还有一个地方的传输错误没有被检查，那就是负责路由数据包的设备内部。一个数据包到达一个端口，并进行错误检查，也进行路由检查，然后当它被从另一个端口发出时会计算出一个新的LCRC值并添加在数据包中，新的LCRC会取代老的LCRC。内部端口之间的转发可能会遇到PCIe协议没有检查到的错误，这时候就需要ECRC来发挥它的作用了。这里可以理解为，当TLP到达Switch的一个接收端口时，Switch会在接收端口对其LCRC进行校验，然后根据路由信息对数据包进行转发，但是这个转发过程是不对LCRC进行校验的，直到转发到对应的输出端口，然后在输出端口给数据包加上新的LCRC后发送出去，不难发现因为在内部转发过程中不对LCRC进行校验，因此若转发过程中出现了错误则就需要通过更内层封装的ECRC来进行校验了，否则内部转发过程是否出错将无从可知。

最后，数据链路层封装好的数据包被转发给物理层，物理层将其他一些字符添加到数据包中，这些字符可以让接收方知道接下来将会接收什么（例如标识一个数据包的开始或结尾）。对于前两代的PCIe，物理层将在数据包的头和尾添加控制字符。而在第三代PCIe中不再使用控制字符，而是在数据包中添加一些额外的bit来提供关于数据包的信息。经过这些处理后，数据包被编码（8b/10b for Gen1/Gen2，128b/130b for Gen3），然后在链路上所有可用通道中以差分形式传输。

###### l TLP拆包（TLP Packet Disassembly）

当对端的接收方看到了输入的比特流时，它需要对此前组包时添加的哪些部分进行识别和剥除，这样就能恢复出发送方设备的Device Core的原始请求信息。如图 2‑17所示，物理层将会确认当前比特流中是否存在正确的“起始”、“结束”或者其他字符，并将它们剥除，然后将剥除了这些字符后的TLP转发给数据链路层。数据链路层将首先进行LCRC以及序列号（Sequence Number）的错误校验。如果并未发现错误，那么数据链路层将会把LCRC和序列号从TLP剥除，并将TLP转发给事务层。如果接收方是一个Switch，那么将在事务层对这个数据包进行解析评估，从它的数据包头中找到路由信息来确定这个数据包要被转发到哪一个端口。即使这个Switch并不是TLP最终的目的地，它也可以对这个TLP进行ECRC校验以及在发现错误时进行ECRC错误汇报。但是， Switch不能更改这个TLP中的ECRC，这是希望最终的目标设备也可以检测到这个ECRC错误。

如果目标设备有能力并且启用了ECRC校验的功能，那么它将对ECRC进行错误校验。若TLP到达了最终的目的设备，而且校验无错，那么在事务层中将把这个ECRC字段剥除，使得整个数据包只剩下Header和数据部分，并将这些剩余部分转发给软件层。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image080.jpg)

图 2‑17 TLP的拆包形式

##### 2.2.2.2   Non-Posted事务

###### l 普通读（Ordinary Reads）

如图 2‑18展示了一个Memory Read请求，这个请求由EP发往系统memory。在Memory Read请求TLP中有一个很重要的部分，那就是目标地址（关于更多有关TLP内容的详细讲解，请见Chapter 5 “TLP Element”）。一个Memory请求的地址可以是32bit或者64bit的，这也决定了数据包的路由方式。在这个例子中，这个请求事务通过两个Switch的路由后向上转发给了目标设备，例子中的目标设备为RC。当RC对请求包进行译码，并识别出了数据包中的地址指向了系统memory，它将从那段系统memory中取出EP所请求的数据。为了将这些从memory中取出来的数据返回给Requester，RC端口的事务层将产生足够多的完成包（Completion），这些数量的完成包足以将Requester所请求的全部数据都返回回去。PCIe中规定一个数据包中数据荷载（data payload）最大为4KB，但是实际设计的设备中一般会使用的payload会比4KB小，因此需要好几个完成包才能足够将比较大量的数据返回给Requester。

这些完成包内也包含了路由信息，用于将它们指引回到Requester，这是因为Requester在原先的请求包中就附带了需要返回的地址的信息，所以Completer就可以将这个地址用来放在完成包中作为完成包的路由信息。这个“返回地址”其实很简单，它就是PCI中定义的Device ID，这个ID由三个东西组成：Requester所属PCI总线在系统中的PCI总线号、Requester在所属PCI总线上的设备号、Requester在所属设备中的Function号。这样的总线号、设备号、Function号组合起来的信息（有时被缩写为BDF，Bus、Device、Function）就是完成包用来返回到Requester的路由信息。正如PCI-X所做的那样，Requester可以同时有多个正在进行的拆分事务（split transaction），它必须能够将输入的完成包和正确的请求关联起来。为了便于实现这一点，Requester在原先的请求包中加入了一个值称为Tag，这个Tag对于每一个请求而言都是独一无二的，也就是说每一个未完成的请求都有一个与其他未完成的请求不同的Tag号。Completer会将这个事务的Tag拷贝进完成包中，这样Requester就可以快速地通过完成包中的Tag来将这个完成包与正确的请求关联起来，也就是找到了这个完成包是用来服务哪个请求的。最后还要说一点，Completer还可以设置完成包中的完成状态字段中的bit，以此来表示出事务的错误情况。这至少可以让Requester对哪里可能出错了有一个大致的了解。Requester如何去处理大多数的错误，这个事情一般是由软件来决定，这并不在PCIe协议规范的范围内。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image082.jpg)

图 2‑18 Non-Posted Read的示例

###### l 锁定读（Locked Reads）

Locked Memory Read这种读请求是为了支持一种叫做原子读-修改-写操作，所谓原子操作就是一种不可中断的事务，处理器使用这种事务来进行测试以及设置信号标（semaphore）等任务。当测试和设置信号标正在进行中时，不允许其他对信号标的访问发生，不予许发生竞争的情况。为了避免发生竞争情况，处理器使用一个锁定指示符（lock indicator）（例如并行总线前段的一个单独的pin），来阻止总线上的其他事务，直到这个锁定的事务完成。接下来是对这个主题内容的一个高层次介绍。有关被锁定事务的更多信息，请参阅附录D，“Appendix D：Locked Transactions”。

回顾一下历史，在PCI的早期，协议规范的制定者们预期到PCI将会实际取代处理器总线。因此，PCI协议规范中要求总线要能支持处理器要完成的事情，比如锁定事务。然而，PCI很少以这种方式使用，最终，这种处理器总线支持的东西大部分都被丢弃了。不过，锁定周期依然存在，用以支持一些特殊情况，在更进一步的PCIe的发展中也保留了它，以实现对一些遗留事务的支持。也许是为了加速向PCIe的迁移，在新的PCIe设备中进制接收锁定请求（locked request），只有那些遗留设备才能够使用它。在图 2‑19所示的例子中，一个Requester发出了一个MRdLk（Memory Read Locked）来发起事务。根据定义这样的请求仅允许来自CPU，因此在PCIe中仅有RC的端口可以发起这种事务。

这个locked request使用目标memory地址作为路由信息，并最终到达了遗留设备EP（Legacy Endpoint）。当数据包经过沿途的每个路由设备时（称为服务点），数据包的出口端口被锁定，这意味着在被解锁之前这条路径都不能通过其他的数据包。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image084.jpg)

图 2‑19 Non-Posted Locked Read事务协议

当Completer接收到这个请求包并将其内容进行译码，它将收集请求所需要的数据，并使用这些数据产生一个或多个Locked完成包。这些完成包将会被路由回到Requester，使用的路由信息是Requester ID，完成包路径上的出口端口也会被锁定。

如果Completer遇到了一些问题，无法正常响应请求，那么它将向Requester返回一个不含有数据的Lock完成包（正常的读请求完成包应该含有要返回的数据，而这里不包含数据那么我们就知道出现了一些问题），并使用完成包的状态指示区域来标识发生的错误的一些信息。Requester接收到这样的完成包就明白了这个Lock Request失败了，它就会取消这个事务的执行，然后由软件来决定接下来要做什么。

###### l IO和配置写（IO and Configuration Writes）

如图 2‑20中展示了一个Non-Posted IO写事务。与Locked request相类似，这样的一套IO操作流程的目的设备只能是一个遗留EP。请求包使用IO地址作为路由信息在Switch中被路由转发，直到它到达目标EP。当Completer接收到这个写请求包，它接收请求包内的数据并返回一个单独的不含有数据的完成包，以此来确认自己收到了这个写请求包。完成包中的状态标识区域将会指示出是否发生了错误，如果发生了错误，Requester的软件需要对错误进行处理。

如果完成包中显示并未出现错误，那么Requester就认为写数据被成功的送达目标设备并成功写入，可以允许针对这一个Completer的指令序列继续向下执行。也就是说，在Requester中有一系列的针对Completer的指令，当知道这一个IO写请求完成了之后，允许执行指令序列中的下一个指令，即想表达的是需要确认这个写请求成功了才允许继续执行下一条指令。这很好的总结了Non-Posted写的目的：不同于一个Memory写（MWr是Posted的），Non-Posted写不能仅仅知道数据被送往了目的方，还必须要知道数据真的到达了目的方才行，否则在逻辑上不能继续执行下一步。又如同Locked操作一样，Non-Posted写请求只能由处理器发出。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image086.jpg)

图 2‑20 Non-Posted Write事务协议

##### 2.2.2.3   Posted写事务

###### l Memory写（Memory Writes）

Memory写请求必然是Posted类型的，它不需要返回完成包。一旦这种写请求包被发出，Requester不需要等到任何反馈就可以继续去进行下一条请求，不需要在返回完成包这件事情上花费时间和带宽。因此，Posted写会比Non-Posted写更加快速且高效，并很好的提升了系统的性能。如图 2‑21所示，请求包使用memory地址作为路由信息在系统中进行路由转发，并最终到达Completer。一旦链路成功的将这个请求发送过去（这里的成功是指把数据包传输了过去，而不需要得到Completer的成功确认），这个事务在链路上就已经结束了，链路上就可以继续传输其他的数据包了。最终，Completer接收写请求包中的数据，这个事务才真正完成。当然，这种事务执行方法在提升效率的同时也舍弃了一些东西，因为Completer不需要发送完成包，所以这也意味着它无法将错误报告给Requester。如果Completer发生了错误，那么它可以记录下这个错误，并向RC发一个Message来通知系统软件这些情况，但是Requester对这些是完全不知情的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image088.jpg)

图 2‑21 Posted Memory Write事务协议

###### l Message写（Message Writes）

十分有趣的是，Message不同于我们前面所说的那些请求事务，它有好几种路由方法（前面的都是通过memory地址、IO地址中的一种），在Message内部专门有一个区域来标识使用的是哪一种路由方式。例如，有一些Message是Posted写请求，其目的方为特定的Completer；有一些是RC向所有EP广播的请求；还有一些是EP发出的要自动路由到RC的请求。想要学习更多关于不同的路由类型的内容，请参阅Chapter 4“Address Space & Transaction Routing”。

Message在PCIe中很有用，它可以使得PCIe达到减少引脚数量的设计目标。它使得PCIe不再需要边带信号（side-band signals），而在PCI中则是需要用边带信号来报告中断、功耗管理事项以及错误信息，而在PCIe中使用Message则可以将这些信息使用数据包来进行报告，数据包是直接在一般数据路径上传输的，因此不再需要额外的边带信号。

##### 2.2.2.4   QoS服务质量（Quality of Service）

PCIe从一开始就被设计为能够支持时间敏感事务，例如流视频、音频应用程序，对于这些应用程序来说数据必须被及时传输才能保证数据有效。这被称为提供了QoS，它是通过添加一些东西来实现的。首先，每个数据包都被软件分配了一个优先级，这个优先级是通过设置数据包内的一个3bit的字段区域来进行标识的，称之为流量类型Traffic Class（TC）。一般来说，给一个数据包分配一个编号较大的TC表示希望给与这个数据包一个更高的优先级。第二，使用多缓冲区，称为虚拟通道Virtual Channels（VC），将其构建在硬件的每一个端口中，数据包会根据其TC值也就是优先级，来被放入响应的虚拟通道中（相应的缓存区中）。第三，由于现在对于一个端口来说，在一个时刻将会有多个缓冲区都存在可以进行传输的数据包，因此需要有对VC进行选择的仲裁逻辑。最后，Switch必须在相竞争的各个输入端口间做出选择，以便访问相应端口的VC。这一步被称为端口仲裁，它可以由硬件来进行分配或是由软件来进行编程配置。这与第三点的不同之处在于，第三点是在一个端口内对多个VC进行仲裁选择，而端口仲裁是指各个端口已经选择好了此次访问的VC，需要由Switch仲裁选择访问哪一个端口。所有的这些硬件部件都必须要能够支持系统对数据包进行优先级排序。如果一个这样的系统被正确的编程配置，它可以为给定的路径提供有保障的服务。

为了用图片来解释这个概念，请见图 2‑22，其中的视频摄像头以及SCSI设备都需要向系统DRAM发送数据。这二者的不同之处在于，摄像头是对时效性要求严格的，如果传输路径无法满足摄像头的带宽，那么将出现丢帧的现象。因此系统需要保障摄像头所需要的最小带宽，否则它捕捉的视频画面将会出现不稳定。与此同时，SCSI数据需要正确无误的进行传输，但是对于它来说传输需要的时间长短并不是那么重要。显然，当视频数据和SCSI数据同时需要被发送时，视频数据流应当具有更高的优先级。QoS指的是系统的一种能力，是系统为数据包分配不同优先级并将这些数据包按照确定性的延时、带宽在系统拓扑中进行路由的能力。想了解更多关于QoS的细节，请参阅Chapter 7“Quality of Service”。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image090.jpg)

图 2‑22 QoS示例

##### 2.2.2.5   事务排序（Transaction Ordering）

在一个虚拟通道VC内，数据包一般全都按照它们到达的先后顺序进行排序和移动，但是也有例外情况。PCIe协议继承了PCI的事务排序模型，包括支持PCI-X中加入的宽松排序（relaxed-ordering）。这些排序规则保证了使用相同TC（Traffic Class）的数据包都会按照正确的顺序在拓扑结构中被路由转发，防止潜在的死锁或者活锁情况。需要注意的一点是，由于排序规则仅应用于VC，且使用不同TC的数据包一般不会被划分进同一个VC，因此使用不同的TC的数据包在软件看来就不存在排序关系。这种排序在事务层中的VC中维护。

##### 2.2.2.6   流量控制（Flow Control）

串行传输所使用的一个典型协议是，要求发送方仅在对端有足够的缓冲区接收时才发送数据包。这样的规定删去了总线上的性能浪费操作事件，比如PCI中允许进行的断开（disconnect）与重试（retry），这使得这类问题在传输中得到消除。这样做的代价是，接收方必须足够频繁的报告它的可用缓冲区空间来避免不必要的传输停顿，而且这样的报告也需要占用接收方自己的一点带宽。在PCIe中，这个可用缓冲区空间的报告是由DLLP（Data Link Layer Packet）来完成的，我们将在下一节讲到DLLP。这里不使用TLP的原因是为了避免可能出现的死锁现象，如果使用TLP则可能出现，例如一个设备A作为发送方需要对接收方B的可用缓冲区空间进行更新，但是当A的接收缓冲区满导致它又无法接收来自B的可用缓冲区报告，这样就出现了一个死循环。而DLLP可以不管缓冲区状态就进行收发，这样就避免了死锁的问题。这样的流量控制协议由硬件级进行自动管理，对于软件来说是透明的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image092.jpg)

图 2‑23流量控制基础操作

如图 2‑23所示，接收方内的VC Buffer内缓存了接收到的TLP。接收方将自己的缓冲区大小通过Flow Control DLLP（流量控制DLLP）来告知发送方。发送方将会跟踪接收方的可用缓冲区空间的值，并且不允许发出大于这个可用空间的数据包。当接收方对缓冲区中的TLP进行了处理，并将这个TLP移出了缓冲区，此时缓冲区中就空闲出了新的可用空间，那么接收方将会定期的发送Flow Control Update DLLP（流量控制更新DLLP）来保持发送方能够获取到最新的可用空间的值。想了解更多关于这方面的内容，请见Chapter 6“Flow Control”。

#### 2.2.3     数据链路层（Data Link Layer）

数据链路层这一层的逻辑是用来负责链路管理的，它主要表现为3个功能：TLP错误纠正、流量控制以及一些链路电源管理。它是通过如图 2‑24所示的DLLP（Data Link Layer Packet）来完成这些功能的。

##### 2.2.3.1   DLLPs数据链路层包（Data Link Layer Packet）

DLLP是一种在位于同一条链路上的本端设备的数据链路层与对端设备的数据链路层之间传输的数据包。事务层对这些数据包并无感知，它们只在两个相临近的设备之间传输，不会被路由到任何其他地方。DLLP通常要小于TLP，大部分情况下DLLP只有8Bytes，这是一件好事，因为DLLP的大小也反应着维护链路所需要的开销。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image094.jpg)

图 2‑24 DLLP的起始地和目的地

###### l DLLP组包（DLLP Assembly）

如图 2‑24所示，一个DLLP起源于发送方的数据链路层，并结束于接收方的数据链路层。在这个DLLP中由发送方给DLLP Core加上了一个16bit CRC，用来供接收方进行错误校验。在加完CRC之后，这个DLLP被转发至物理层，在这一层里给DLLP加上包起始字符和包结束字符（这种方法是Gen1和Gen2的方法，Gen3不再使用），并对加完两种字符的DLLP进行编码，然后通过链路上的所有通道（Lane）进行差分传输。

###### l DLLP拆包（DLLP Disassembly）

当接收方的物理层接收到了一个DLLP，这个DLLP的比特流将被译码并剥去包起始字符与包结束字符。剩余的DLLP部分被转发至数据链路层，在这里进行CRC错误校验，并根据包的内容执行相应的操作。接收方的数据链路层就已经是DLLP的最终目的地，它不会被继续向上呈交给事务层。

##### 2.2.3.2   Ack/Nak协议（Ack/Nak Protocol）

如图 2‑25所示是一种基于硬件的自动重试机制（hardware-based automatic retry mechanism）。如图 2‑26所示，每一个被发送方发出的TLP都被加上了LCRC与序列号（Sequence Number），在接收方会对这两个信息进行检查。发送方的重传Buffer（replay buffer）保存着每个被发送的TLP的副本，直到对端确认成功收到了这个TLP。这个确认成功收到的过程是通过接收方发送Ack DLLP来实现的，在这个Ack DLLP中包含有接收方成功接收的上一个TLP的序列号（Sequence Number）。当发送方收到这个Ack DLLP，它将会把里面Sequence Number所对应的TLP、以及这个TLP之前的所有TLP，都从重传Buffer内清除。

如果接收方检测到了一个TLP错误，它将会把这个TLP丢弃，并向发送方返回一个Nak DLLP，以期望发送方能对未确认成功接收的TLP进行重传，并通过重传获得一个完好的TLP。由于错误检测通常是十分迅速的操作，因此从错误检测开始到发起重传的时间也比较短，这样就可以在较短的时间内纠正发生的错误。这一操作过程被称为Ack/Nak协议（Acknowledge/NotAcknowledge）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image096.jpg)

图 2‑25 数据链路层重传机制

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image098.jpg)

图 2‑26 在数据链路层中的TLP与DLLP的包结构

一个DLLP的基础形式如图 2‑26所示，它由4Byte的DLLP类型域（DLLP Type field）以及2Byte CRC组成，其中DLLP类型域中包含了一些其他的信息。

如图 2‑27所示，它展示了一个memory read TLP通过Switch的示例。一般来说，这个过程的步骤如下：

**1.**    **步骤1a****：**Requester发送一个memory read请求，并在自身的重传Buffer（replay buffer）中也保存一个请求包的副本。Switch接收这个MRd TLP（Memory Read TLP，以后简写为MRd TLP），并校验LCRC以及序列号（Sequence Number）。

**步骤1b****：**校验未发现出错，因此Switch向Requester返回一个Ack DLLP。作为响应，Requester将其重传Buffer中保存的这个MRd TLP的副本丢弃。

**2.**    **步骤2a****：**Switch将这个MRd TLP转发到正确的输出端口，使用的路由信息是这个MRd TLP的memory地址，在转发出去的同时，Switch会在这个输出端口的重传Buffer内保存一份这个TLP的副本。Completer接收到这个MRd TLP并对其进行错误校验。

**步骤2b****：**校验未发现出错，因此Completer向Switch返回一个Ack DLLP。作为响应，Switch之前输出这个TLP的输出端口会将其重传Buffer中保存的这个MRd TLP的副本丢弃。

**3.**    **步骤3a****：**作为这个请求的最终目的地，Completer将会校验MRd TLP中的ECRC字段域，这个字段域是可选的不是必需的。若校验未出错，则将这个请求呈交给Device Core。然后根据这个请求命令中的相关信息，设备将会收集被请求的数据，并向Requester返回一个带有数据的完成包（Completion with Data TLP，CplD），同时它也将在自己的重传Buffer中保存这个CplD TLP的副本。当Switch接收到这个CplD TLP时将会对其进行错误校验。

**步骤3b****：**校验未发现出错，因此Switch向Completer返回一个Ack DLLP。作为响应，Completer将其重传Buffer中保存的这个CplD TLP的副本清除。

**4.**    **步骤4a****：**Switch对这个CplD TLP中的Requester ID进行译码，并通过这个信息将CplD TLP路由至正确的输出端口，输出的同时在这个输出端口的重传Buffer内保存CplD TLP的副本。当Requester接收到这个CplD TLP时将会对其进行错误校验。

**步骤4b****：**校验未发现出错，因此Requester向Switch返回一个Ack DLLP。作为响应，Switch之前输出这个CplD TLP的输出端口会将其重传Buffer中保存的这个CplD TLP的副本丢弃。Requester将校验ECRC字段域（这个字段是可选的，不是必需的），校验未出错则将完成包里的数据向上呈交给Device Core逻辑。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image100.jpg)

图 2‑27使用Ack/Nak协议的Non-Posted事务

##### 2.2.3.3   流量控制（Flow Control）

数据链路层的第二个主要功能就是流量控制。在系统上电和复位之后，流量控制机制被数据链路层的硬件自动初始化，并在整个运行过程中更新。关于这些内容的概述已经在TLP的那一节给出，所以这里不再赘述。想要了解更多关于这一主题的内容，请参阅Chapter 6“Flow Control”。

##### 2.2.3.4   电源管理（Power Management）

最后要介绍的一点，数据链路层也参与了电源管理，因为链路与系统电源状态相关的请求和握手也可以通过DLLP来进行。想了解关于这一主题的更详细的内容，请参阅Chapter 16“Power Management”。

#### 2.2.4     物理层（Physical Layer）

##### 2.2.4.1   整体说明（General）

物理层时PCIe协议层次里的最底层，如图 2‑14。TLP和DLLP这两种类型的数据包都需要从数据链路层向下转发至物理层，这样才能通过链路传输至对端接收方设备，并从接收方的物理层向上转发至它的数据链路层。协议规范中将关于物理层的讨论分为两部分：逻辑部分和电气部分。我们这里也将保留这种划分方式。逻辑物理层（Logical Physical Layer）包含了一系列的数字逻辑，这些数字逻辑是关于准备将数据包在链路上进行串行传输的逻辑以及相反的输入数据包的处理逻辑。电气物理层（Electrical Physical Layer）是物理层的模拟电路接口，它与链路直接相连，它为每个通道（Lane）提供差分驱动器（differential driver）以及差分接收器（differential receiver）。

##### 2.2.4.2   逻辑物理层（Physical Layer-Logical）

由数据链路层转发来的TLP与DLLP被物理层中的buffer所记录，在这个buffer中会对这些数据包加上包起始字符和包结束字符，这两个字符将有助于接收端用来检测数据包的边界。由于包起始字符和包结束字符分别出现在数据包的两端（头和尾），因此他们也被称作“组帧（framing）”字符。如图 2‑28展示了组帧字符被添加在TLP和DLLP上，它也展示了这两种字符的字段长度。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image102.jpg)

图 2‑28物理层中的TLP与DLLP的结构

在物理层中，数据包中的每一个字节都将被分割到链路所使用的所有通道，这一过程被称为字节条带化（Byte Striping）。实际上，每个通道在链路上都扮演着一个独立的串行传输路径，这些通道们各自传输的数据会在接收端被聚合。每个字节都进行了扰码，以此来减少在传输线上传输连续重复的“0”或“1”，这有助于减少链路上的EMI（electro-magnetic interference，电磁干扰）。对于前两代的PCIe（Gen1和Gen2），8bit的字符将被编码为10bit“符号（symbol）”，所使用的编码方式被称为8b/10b编码。这种编码增加了输出数据流的额外开销，但是也带来了不少有用的特性（关于这里的更多内容请见“8b/10b Encoding”一节）。PCIe Gen3的物理层逻辑在使用Gen3速率进行传输时（因为也可以向下兼容Gen1、Gen2速率），不再使用8b/10b编码方式，而是采用了另一种被称为128b/130b的编码，它会将数据包的字节扰码后进行传输。每个通道（Lane）的10b符号（对于Gen1 Gen2）或者每个通道的数据包的字节（对于Gen3）随后就会在链路的每个通道上以串行差分的形式被传输，对应的速率为Gen1 2.5GT/s，Gen2 5GT/s，Gen3 8GT/s。

当数据包的bit到达接收端时，接收端以训练完成的时钟速率对这些数据包的bit进行接收。如果使用的是8b/10b编码，那么输入的比特流将被串并转换器转换成10bit符号，然后准备用来做8b/10b解码。然而，在解码之前，10bit符号将会经过一个弹性缓存（elastic buffer），这是一个聪明的设备，它可以对两个相连接设备各自内部时钟之间的微小频率差异进行补偿。接下来，10bit符号被8b/10b解码器解码，正确地恢复成8bit字符。PCIe Gen3与前面所述的这个过程不同，对于Gen3的物理层逻辑来说，当接收到以Gen3速率传输的数据包串行比特流时，将使用串并转换器将这个比特流转换为字节流，这个串并转换器已经建立了块锁定（Block Lock，这是链路训练中的一个要素）。随后字节流会通过一个弹性缓存进行时钟容忍度补偿（clock tolerance compensation）。在Gen3速率下数据包数据并没有使用8b/10b编码，因此可以跳过8b/10b解码这一步。最终，每个通道中的8bit字符都将经过解扰，然后经过反条带化（un-striped）之后将所有通道内的字节聚合重新恢复成单符号流（single character stream），这样就在接收端恢复出了发送端所发出的数据流。

##### 2.2.4.3   链路训练和初始化（Link Training and Initialization）

物理层的另一个任务就是负责链路的初始化以及训练过程。在这个全自动化的过程中，为了让链路准备好进行正常工作，我们采用了几个步骤，主要为确定几个可选条件的状态。例如，链路宽度可以从1通道到32通道，并且可以提供多种速度。链路训练过程将会发现这些可选项并通过一个状态机来寻求一个建立最佳连接的组合，也就是说链路训练将会确认这些可选项具体的值（链路宽度、速率等）。在这个过程中，为了确保执行正确以及最优的操作，我们需要检查或者建立一些东西，例如：

u 链路宽度（Link width）

u 链路数据率（Link data rate）

u 通道调转（Lane reversal）——通道收发连接相反了，本应TX连对端RX，若相反则变成TX连了对端TX。

u 极性反转（Polarity inversion）——通道极性连接相反，TX_P应当连接对端的RX_P，若相反则变成了TX_P连接了对端的RX_N。

u 每个通道的位锁定（bit lock）——恢复出发送方的时钟。

u 每个通道的符号锁定（symbol lock）——在比特流中找到一个可辨认的位置。

u 多通道链路中的Lane-to-Lane歪斜去除（de-skew）。

##### 2.2.4.4   电气物理层（Physical Layer-Electrical）

物理的发送器和接收器是通过一个AC耦合链路相连接，如图 2‑29所示。所谓“AC耦合（交流耦合）”，意思是两个设备相连接的物理路径上放置有电容，它用于让信号的高频部分（AC交流）通过，而阻塞低频（DC直流）部分。许多串行传输方式中都使用了这种方法，因为它允许发送端和接收端的共模电压不同（共模电压指的是0、1电平交界处的电压），这意味着发送端和接收端可以使用不同的参考电压。当两个设备的物理连接距离很近时，这一特性优势并不明显，而若它们距离较远例如在不同的楼里，那么它们将很难使用一个完全相同的公共的参考电压。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image104.jpg)

图 2‑29 电气物理层

##### 2.2.4.5   命令集（Ordered Sets）

最后要介绍的一种在设备之间发送的流只使用物理层。尽管这种信息对于接收端来说很容易进行识别，但是它并没有被做成数据包的形式，原因是比如它没有包起始字符和包结束字符。因此作为一种替代方法，这种信息被组织成了一种被叫做“命令集（Ordered Sets）”的东西，它起始于发送端的物理层，终止于接收端的物理层，如图 2‑30所示那样。对于Gen1和Gen2的数据率下，一个命令集使用一个单独的COM字符作为起始，然后后面接着3个或以上的其他字符用于定义要发送的信息。关于这些不同类型字符在PCIe中的命名的更细节的讨论请见“Character Notation”一节，对于现阶段来说只要知道COM字符的特性能让我们达到想要的设计目标即可。命令集的大小总是4byte的整数倍，图 2‑31展示了一个命令集的例子。在Gen3操作模式中，命令集的格式就不同于上述的Gen1/Gen2格式了。更多详细内容请见Chapter 14“Link Initialization & Training”。命令集永远只会终止于链路的对端设备，而不会被Switch路由转发至PCIe网络中，也就是说对端设备如果是Switch那么它的最终目的地就是Switch。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image106.jpg)

图 2‑30命令集的出发地和目的地

命令集在链路训练（Link Training）过程中同样有应用，请参阅Chapter 14“Link Initialization & Training”。它们也被用于补偿发送端和接收端内部时钟的轻微差异，这一过程被称为时钟容忍度补偿（clock tolerance compensation）。最后一点，命令集还可以用于指示链路进入或者退出低功耗状态。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image108.jpg)

图 2‑31命令集结构

### 2.3 协议回顾（Protocol Review Example）

现在，让我们通过一个示例来回顾整个链路协议，这个例子将从Requester发起一个Memory Read请求（MRd）开始，直到Requester从Completer获得所请求的数据为止，讲解这过程中所发生的操作步骤。

#### 2.3.1     Memory Read Request（MRd请求）

关于我们开始讨论的第一部分，请参考图 2‑32。Requester的Device Core或者说是软件层将会向事务层发起一个请求，这个请求中包含了这些信息：32bit或64bit的memory地址、事务类型、需要读取的数量总量（以dw为单位计数）、流量类型、字节使能、属性等等。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image110.jpg)

图 2‑32 Memory Read请求的各阶段

事务层使用上面所述的那些信息来构建一个MRd TLP。关于TLP内部格式的细节我们稍后再进行描述，目前我们只需要知道TLP的Header大小为3DW或4DW，这取决于它的地址字段的大小（32bit地址对应3DW Header，64bit地址对应4DW Header）。此外，在Header中还有事务层加入的Requester ID字段（bus#，device#，function#），Completer可以通过这个Requester ID字段来返回完成包。TLP随后被置入相应优先级的虚拟通道buffer中，等待轮到它被发送。一旦这个TLP被选中，流量控制逻辑将会确认对端设备的接收buffer（VC，虚拟通道）有足够的可用空间来接收这个TLP，然后MRd TLP就被向下发送至数据链路层。

在数据链路层中，数据包被加上12bit的序列号（Sequence Number）以及32bit的LCRC。并在重传Buffer（Replay Buffer）中保存这个TLP的一个副本，然后这个数据包就被向下转发至物理层。

在物理层中，数据包被加上包起始字符以及包结束字符，然后在所有可用的通道上进行字节条带化（byte striping），再进行扰码（Scramble）、8b/10b编码。最终这个数据包的比特在链路上的每个通道内以串行差分的形式传输到对端。

Completer接收到输入的比特流，它将这个比特流先进行串并转换将比特流恢复成10bit符号，然后让它们通过一个弹性缓存。随后10bit符号被解码后恢复成字节，然后每个通道上的字节都被解扰（de-scramble）以及反条带化（un-striped）。接下来Completer物理层检测到包起始字符和包结束字符，并将它们从TLP中剥除。剩余的TLP被向上转发至数据链路层。

Completer的数据链路层将对接收到的TLP进行LCRC错误校验，并检查TLP序列号以确定是否存在TLP丢失或TLP失序。若这些步骤都无错，数据链路层将生成一个Ack信息，里面包含了与这个MRd TLP中相同的序列号。然后再给这个Ack信息加上16bit的CRC，这样就组成了一个Ack DLLP，将这个Ack DLLP送回物理层，由物理层加上相应的组帧符号（framing symbol），并把Ack DLLP传输给Requester。

Requester的物理层接收到Ack DLLP后，对其组帧符号进行检查和剥除，然后向上转发至数据链路层。如果数据链路层对其CRC校验无错，它将会用Ack DLLP内的序列号与重传Buffer中保存的TLP副本的序列号进行比较，并将与Ack DLLP中序列号相匹配的TLP副本从重传Buffer中清除。相反地，若Requester接收到的是一个Nak DLLP，那么它将把序列号匹配的这个MRd TLP进行重传。由于DLLP仅对数据链路层有意义，所以在这些DLLP的操作中将不会有东西向上转发给事务层。

除了上述的产生Ack DLLP之外，Completer的数据链路层还将把TLP向上转发给事务层。在Completer的事务层中，TLP被置于与其优先级相对应的虚拟通道接收buffer中，等待被处理。然后可以对TLP进行ECRC校验（ECRC为可选项），若校验无错，那么TLP Header的内容（地址、Requester ID、MRd事务类型、请求数据总量、流量类型等）将被向上转发至Completer的软件层。

#### 2.3.2     Completion with Data（CplD带有数据的完成包）

下面开始介绍此次举例回顾PCIe协议的另一半内容，请参考图 2‑33。为了服务MRd请求，Completer的Device Core/软件层将会向它的事务层发送一个带有数据的完成包（Completion with Data，CplD）请求，这个完成包请求中包含了与MRd请求中相同的Requester ID、Tag，还包含了事务类型以及另外的完成包头内容，并且完成包中还包含了被请求的数据。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image112.jpg)

图 2‑33带有数据的完成包的各阶段

事务层使用CplD请求中的这些信息来构建一个CplD TLP，这种TLP的Header固定为3DW（这是由于CplD TLP使用Requester ID作为路由信息，因此不会需要用到64bit的地址）。事务层也会将自身的Completer ID添加到CplD TLP Header中。这个数据包随后被放置入一个合适的虚拟通道的发送Buffer中，一旦这个虚拟通道被仲裁选中并要发送这个CplD TLP，流量控制逻辑将会确认对端设备有足够的可用缓冲区空间来接收它，当确认足够后则将这个CplD TLP向下转发至数据链路层。

如前文所述那样，数据链路层给数据包加上12bit的序列号（Sequence Number）和32bit的LCRC。然后将添加完这些信息的TLP保存一份副本在重传Buffer（Replay Buffer）中，之后变将数据包向下转发至物理层。

依然如前文所述那样，物理层给数据包加上包起始字符和包结束字符，并将其在所有可用通道上进行字节条带化，再进行扰码、8b/10b编码。最终，这个CplD TLP数据包在链路的所有通道上以串行差分的形式被传输至对端。

当Requester接收到这个CplD TLP的输入比特流时，它将比特流转换恢复成10bit符号（10bit symbol），并让10bit symbol流通过一个弹性缓存。随后10bit符号被解码恢复为字节，并进行解扰和字节反条带化（un-striped）。然后物理层就能检测到这个CplD TLP的包起始字符和包结束字符，并将这两个字符剥去。之后便将剩余的CplD TLP向上转发至数据链路层。

和之前一样，数据链路层对CplD TLP进行LCRC校验，并检查序列号以确定是否存在TLP丢失或出现TLP失序。如果并未出现错误，数据链路层将产生一个Ack DLLP，其中包含了与CplD TLP中相同的序列号，并给这个Ack DLLP加上16bit CRC，然后将其送回给物理层加上相应的组帧字符（framing symbol）并将这个Ack DLLP传输给Completer。

Completer的物理层将会检测并剥除Ack DLLP的组帧字符，并将剩余的部分向上转发给数据链路层，在这里对Ack DLLP的CRC进行校验。若校验无错，Completer的数据链路层将Ack DLLP中的序列号与重传Buffer中的TLP的序列号进行对比。与Ack DLLP序列号相匹配的TLP将被从重传Buffer中清除。反之，若Completer接收到的是一个Nak DLLP，那么它将使用重传Buffer中存储的CplD TLP副本来进行重传。

与此同时，Requester的事务层在某个虚拟通道的缓存中接收到了这个CplD TLP。作为一个可选操作，事务层可以校验这个TLP的ECRC。若校验无错，事务层将这个CplD TLP的Header内容、数据荷载以及请求完成状态信息向上呈交给Requester的软件层。至此，大功告成。

 



## Chapter 3      Configuration Overview //PCIe配置概述

### 关于前一章

前一章节对PCIe体系结构进行了全面介绍，我们将其看作是一种“执行层（executive level）”概述。它对协议中描述PCIe端口分层设计方法进行了介绍。在介绍事务协议时也一并介绍了各种数据包的种类。

### 关于本章

本章节将对如何对PCIe环境进行配置来展开介绍。内容包括了实现Function配置寄存器的空间、如何发现一个Function、如何产生并路由转发配置事务、 PCI兼容配置空间（PCI-compatible configuration space）与PCIe扩展配置空间的不同点（PCIe extended configuration space），以及软件是如何区分开EP和Bridge的。

### 关于下一章

下一章的内容将描述Function如何通过基址寄存器（Base Address Register，BARs）来请求memory或者IO地址空间，以及这种请求的目的与作用，并且将介绍软件是如何初始化这两种地址空间的。另外在下一章还会描述Bridge的Base/Limit寄存器（基/边界寄存器）是如何被初始化的，因为只有当Base/Limit寄存器初始化后Switch才能在PCIe网络中路由转发TLP。

### 3.1 总线/设备/功能/的定义（Definition of Bus,Device and Function）

正如PCI一样，每个PCIe功能（Function）在其所在的设备内，以及这个设备所连接的总线内，都是被唯一标识的。这个唯一的标识符一般被称为“BDF”。配置软件负责在一个给出的PCIe拓扑中检测出每个Bus、Device和Function，这就是BDF。接下来的几节将会结合一个PCIe拓扑示例的上下文，来讨论BDF的主要特征。图 3‑1展示了一个PCIe拓扑结构，图中着重标识了示例系统中的Buses、Devices和Functions。在本章的后面，将解释如何分配总线号和设备号。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image114.jpg)

图 3‑1示例系统

### 3.2 PCIe总线（PCIe Buses）

软件总舵可以分配256个总线号。最初的总线号，Bus 0，通常由硬件分配给RC（Root Complex）。Bus 0由一个带有内部集成EP的虚拟PCI总线，一些虚拟PCI-to-PCI Bridges（P2P）组成，这其中的P2P Bridges拥有确定写死（hard-coded）的设备号和功能号。每个P2P Bridge都会产生一个新的总线，额外的PCIe设备可以连接在这些总线上面。每个总线都必须被分配一个唯一的总线号。配置软件分配总线号的过程中，第一件事情就是从Bus 0/Device 0/Function 0开始搜索其他的Bridges。当找到一个Bridge之后，软件就给这个Bridge产生的新总线分配一个总线号，新总线的总线号会比Bridge所属的上一级总线的总线号要大。一旦新总线被分配了一个总线号之后，软件就会在这个新总线上开始搜索Bridges，而不是在上一级总线上继续搜索。这被称为“深度优先搜索（depth first search）”，关于这种搜索的细节内容，请参阅“Enumeration – Discovering the Topology”一节。

### 3.3 PCIe设备（PCIe Devices）

PCIe允许在单个PCI总线上最多挂载32个设备，然而PCIe点对点（point-to-point）的性质意味着只有一个设备可以直接连接在PCIe链路上，该设备只能是Device 0。RC和Switches都含有虚拟PCI总线，这些虚拟总线使得可以让更多的设备“连接”在总线上。每个设备都必须实现Function 0，其最多可以有8个功能（Function）。当一个设备拥有2个或以上的Function时，便把这个设备成为多功能设备（Multi-Function Device）。

### 3.4 PCIe功能（PCIe Functions）

正如此前所讨论的一样，Function是被设计在每个设备中的。这些Function可能包含硬盘驱动接口、显示控制器、以太网控制器、USB控制器等等。具有多Function的设备不需要依次来逐个实现它们。例如，一个设备可以实现Function 0、2、7。因此，当配置软件检测到了一个多Function设备时，必须检查所有可能的Function，以了解当前Device存在哪些Function。每个Function都有它们自己的配置地址空间，这个配置地址空间用于设置与Function相关的资源。

### 3.5 配置地址空间（Configuration Address Space）

初期的PC需要用户设置开关和跳线来给每个安装上去的板卡分配资源，这样的方法经常会导致memory、IO和中断的设置出现冲突。这之后出现的两种IO架构——扩展ISA（EISA，Extended ISA）和IBM PS2系统，这二者是初次实现了即插即用（plug and play）的架构。在这些架构中，配置文件随每个插件卡一起被提供，允许系统软件分配基本资源。PCI扩展了这种能力，它通过实现标准化的配置寄存器，允许通用的Shrink-Wrapped（压缩包装）操作系统管理几乎所有的系统资源。当拥有了一个标准化的方式来进行错误报告的开启与关闭、传送中断、进行地址映射以及更多其他的操作之后，就使得可以通过一个实体——配置软件，来进行系统资源的分配和配置，这样做就可以消除几乎所有的资源冲突。

PCI为每个Function都定义了一个专用的配置地址空间块。映射入这个配置空间的寄存器们使得软件可以发现这个Function的存在，并对这个Function进行一般的操作和检查状态。大多数需要标准化的基本功能都存在于配置寄存器块的Header中，但是PCI架构师意识到若将可选功能（option feature，区别于前面的基本功能）也标准化可以带来很多好处，称这些可选功能标准化后的结构为“能力结构capability structures”，（例如电源管理、热插拔等等）。对于每个Function，都含有256byte的PCI兼容配置空间（PCI-Compatible configuration space）。

#### 3.5.1     PCI兼容空间（PCI-Compatible Space）

在阅读下面的讨论内容时，请同时参阅图 3‑2。之所以将这256Byte命名为PCI-compatible configuration space（PCI兼容配置空间），是因为这些配置空间原本就是为PCI所设计的。这个配置空间的前16DW（64bytes）就是配置头部（header），有两种类型的Header，分别为Type 0和Type 1。Type 0 Header对于每个Function都是必须含有的，除了Bridge，对于Bridge Function来说它使用的是Type 1 Header。剩余的48DW是一些可选寄存器，包括PCI能力结构（capability structure）。对于PCIe Functions，也需要capability structure中的一部分。例如PCIe Function就必须实现如下的能力结构：

u PCI Express能力（PCI Express Capability）

u 电源管理

u MSI、MSI-X

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image116.jpg)

图 3‑2 PCI兼容配置寄存器空间

#### 3.5.2     扩展配置空间（Extended Configuration Space）

在阅读下面的讨论内容时，请同时参阅图 3‑3。当引入PCIe之后，最初始的256byte配置空间已经不足以放下所有的新的需要的Capability Structure了。因此配置空间的大小从原先的每个Function 256Byte扩展至了每个Function 4KByte。新增加出来的960DW扩展配置空间只能通过增强配置机制（Enhanced configuration mechanism）来进行访问，因此这部分区域对于传统的PCI软件是不可见的，因为它无法发现这个区域并进行访问。在扩展配置空间内包含了新增加的PCIe可选扩展能力寄存器（Extended Capability register），通过图 3‑3就可以看到一些被罗列出来的扩展能力寄存器（列出来的并非全部）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image118.jpg)

图 3‑3每个PCIe Function所拥有的4KB配置空间

### 3.6 Host-to-PCI Bridge配置寄存器

#### 3.6.1     整体说明（General）

对Host-to-Bridge配置寄存器的访问不必使用前面的章节所提到的配置机制，它通常作为设备特定寄存器在内存地址空间中被实现，这是平台固件本身就知道的。然而，它的配置寄存器格式排布和用法都必须遵从PCI 2.3协议规范中所定义的Type 0模板。

#### 3.6.2     只有RC发送配置请求（Only the Root Sends Configuration Request）

在协议规范中声明了，只有RC可以发起配置请求。RC作为了系统处理器的联络员，将请求包传入网络中并收回完成包。之所以仅限于处理器通过RC发起配置事务，是因为要是其他设备也有这种能力，那么这些设备可能会改变配置内容，这样就会带来混乱。

由于只有RC能发起这些配置请求，所以这些配置请求只能在拓扑中向下转发，也就是它们无法从拓扑低层被向上转发回到RC，这就意味着Peer-to-Peer的配置请求是被禁止的。请求包使用目的设备ID作为路由信息进行路由转发，这个目的设备ID就是它的BDF（拓扑中的Bus编号，Bus上的Device编号，Device中的Function编号）。

### 3.7 生成配置事务（Generating Configuration Transactions）

处理器一般无法直接完成读请求和写请求，因为他们只能产生memory请求和IO请求。这意味着RC需要将某些访问转换成配置请求，这样才能支持进行配置的操作过程。配置空间可以通过以下两种机制进行访问：

u 传统的PCI配置机制，使用IO间接访问（IO-indirect access）

u 增强型配置机制，使用内存映射访问（memory-mapped access）

#### 3.7.1     传统PCI机制（Legacy PCI Mechanism）

在PCI协议规范中定义了IO-indirect方法，用来指示系统（RC或其等效系统）进行PCI配置访问。在当时的历史背景下，占主导地位的PC处理器（Intel x86）被设计为仅寻址64KB的IO地址空间。当PCI被定义出来的时候，这个有限的IO空间已经变得非常混乱，只有几个地址范围仍然可用：0800h-08FFh，和0C00h-0CFFh。因此，将所有可能的Function的配置寄存器都映射到IO空间是不可行的。与此同时，内存地址空间也十分有限，将这些配置空间都映射进内存地址空间也并不是一个好方法。于是，协议规范的作者使用了一种常用的方法来解决这个问题，使用间接地址映射（indirect address mapping）。为此，需要一个寄存器保存住目标地址，同时用第二个寄存器保存住来自或是发往目标的数据。先对地址寄存器进行写入，随后再对数据寄存器进行读取或写入，这样就完成了对目标Function正确的内部地址的一次读事务或是写事务。这很好的解决了地址空间有限的问题，但是这意味着产生一次配置访问需要两次IO访问。

PCI兼容机制使用RC的Host Bridge中的两个32bit的IO端口。它们分别是配置地址端口（Configuration Address Port），位于IO地址区域0CF8h-0CFBh，和配置数据端口（Configuration Data Port），位于IO地址区域0CFCh-0CFFh。

要访问一个Function的PCI兼容配置寄存器，首先要将目标的Bus、Device、Function和DW号写入配置地址端口，并将其使能bit置为有效。然后第二步，一个1或2或4Byte的IO读或写将会发送到配置数据端口。RC的Host Bridge将对给定的目标总线和在Bridge下现存的总线范围进行比较。若目标总线在这个范围内，这个Bridge则会发起配置读请求或者配置写请求（这取决于对配置数据端口的IO访问是读操作还是写操作）。

##### 3.7.1.1   配置地址端口（Configuration Address Port）

配置地址端口仅在处理器对其完成一个完整的32bit写操作时，锁存住写入的信息，如图 3‑4，若对这个端口进行读操作则会返回它的这些内容。写入配置地址空间的信息必须遵照下面所描述的模板（图 3‑4）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image120.jpg)

图 3‑4地址位于0CF8h的配置地址端口

u Bits[1:0]是写死不变的且只读的，在读取时只能返回0。它的位置是DW对齐的，不允许字节特定（byte-specific）的偏移量。

u Bits[7:2]用于标识目标Function的PCI兼容配置空间内的目标DW（target dword，其也被称作寄存器号），也就是用来标识Function内的寄存器号，要求这个寄存器必须位于兼容配置空间中。这种机制仅限于兼容的配置空间中使用。（例如一个Function配置空间的前64DW）。

u Bits[10:8]用于标识目标Device内的目标Function号(0-7)。

u Bits[15:11]用于标识目标Device号(0-31)。

u Bits[23:16]用于标识目标Bus号(0-255)。

u Bits[30:24]为保留字段，必须为0。

u Bit[31]为使能位，若要将随后的对配置数据端口的IO访问转换为配置访问则必须要将该bit置为1。当该bit为0时，若有IO读或者IO写被发送到配置数据端口，那么这些事务都会被当成普通的IO请求来处理。

##### 3.7.1.2   总线比较和数据端口的使用（Bus Compare and Data Port Usage）

如图 3‑5，RC内的Host Bridge实现了一个“次级总线号寄存器（Secondary Bus Number）”和一个“从属总线号寄存器（Subordinate Bus Number）”。次级总线号（图 3‑5中RC的Sec=0中的Sec）指的是当前Bridge下直接紧挨的总线的编号，例如图中RC的Host/PCI Bridge产生了Bus 0，因此Host/PCI Bridge的次级总线号就是0。从属总线号（图 3‑5中RC的Sub=9中的Sub）指的是Bridge之下的可作为目标总线的总线号，例如图中Device 1的Sub=9，那么就说明在Device 1之下从属的总线号最大到Bus 9，而它的Sec=5则说明其下的总线号从Bus 5开始，简单来说就是Sec和Sub共同指定出了这个设备下可访问总线的范围（对于Device 1来说就是5-9）。

在一个单RC的系统中，Host-Bridge的次级总线号应该被固定为0，也就是它的可读可写的次级总线号寄存器从一复位就被强制置为0，或者说，Host-Bridge知道它访问到的第一个总线一定是Bus 0。若配置地址端口（Configuration Address Port，见图 3‑4）的bit 31被置为1，那么Bridge就会将目标总线号与Bridge之下的从属总线范围进行比较，以此来检查这个目标总线是否从属于当前Bridge之下。

当Bridge收到一个请求，它将会评估目标总线号是否在其下的从属总线号范围内，这个范围是从次级总线号到从属总线号（包括两个边界）。若目标总线号与当前的次级总线号相匹配，那么说明当前的次级总线就是目标总线，这个请求就会被作为一个Type 0配置请求来传输。当设备们收到一个Type 0请求，它们就知道自己所属的本地总线上的某个设备就是这个请求的目标设备（目标设备不在再往下级的从属总线上）。

若目标总线号要大于Bridge的次级总线号，但是小于或者等于Bridge的从属总线号，那么这个请求将会作为Type 1配置请求被转发到Bridge的次级总线上。对于一个Type 1配置请求，可以这样理解它：尽管这个请求需要经过这条总线，但是它的目标设备并不在这一级总线上，相反地，这个请求将会由当前总线上的Bridge们向下转发到各自的下一级总线上，当然这些转发这个Type 1配置请求的Bridge必须是从属总线范围包含了目标总线的。基于这样的原因，Type 1配置请求只对Bridge设备有作用。更多关于Type 0和Type 1配置请求的信息，请参阅“Configuration Request”一节。

##### 3.7.1.3   单Host系统（Single Host System）

RC中的Host/PCI Bridge会将写入配置地址端口（Configuration Address Port）的信息锁存起来，如图 3‑1。若bit 31被置为1且目标总线在当前Bridge下方从属总线范围内，那么Bridge将把接下来的处理器对配置数据端口（Configuration Data Port）的访问转换成针对Bus 0的配置请求。处理器将会向配置数据端口（0CFCh）发起一个IO读请求或者一个IO写请求。这促使Bridge生成一个配置请求，这个配置请求是读请求还是写请求取决于IO访问是读还是写。若目标总线为Bus 0，那么它将是一个Type 0配置请求。若目标总线是从属总线范围内的其他总线，那么它将是一个Type 1配置请求。若目标总线不在从属总线范围内，那么这个Bridge将不会对这个请求进行转发操作。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image122.jpg)

图 3‑5 单Root系统

 

 

 

 

##### 3.7.1.4   多Host系统（Multi-Host System）

若在一个系统中存在多个RC，如图 3‑6所示，那么配置地址端口和配置数据端口将被这两个RC各自的Host/PCI Bridge复用，且两种配置端口各自的IO地址在两个Host/PCI Bridge中相同，也就是两个RC使用相同的一个IO地址来访问各自配置地址寄存器，同理访问配置数据寄存器也是。为了防止争用，在两个Host/PCI Bridge中仅有一个会响应处理器对配置端口的访问。

\1.    当处理器发起对配置地址端口的IO写操作时，两个Host Bridge是经过配置的，因此仅有一个Host Bridge会处于活跃并参与到此次事务中来。

\2.    在枚举过程期间（Enumeration），软件将搜索到所有活跃Bridge之下的所有总线，并对这些总线进行编号。当这一总线搜索过程完成后（不是枚举过程完成），软件将使能那个不活跃的Host Bridge，并给它赋予一个总线编号，这个总线编号会在之前那些活跃Bridge的总线编号范围之外，然后软件继续进行枚举过程。这两个Host Bridge都会看见请求，但是由于它们二者拥有相互不重叠的总线编号范围，而它们各自又只会对自己的范围内的请求作出响应，因此并不会存在冲突的情况。

\3.    在上面的过程之后，两个Host Bridge都会收到对各自配置地址端口的访问，而随后进行的的对配置数据端口的读访问或写访问，则仅会被包含目标总线的Host/PCI Bridge所接收。接收访问的Bridge将会响应处理器的事务，而另一个Bridge则会忽略这个事务。

¡ 若目标总线就是次级总线，那么Bridge将把这个访问转换成Type 0配置访问。

¡ 否则，这个访问将被转换成Type 1配置访问。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image124.jpg)

图 3‑6多Root系统

#### 3.7.2     增强型配置访问机制（Enhanced Configuration Access Mechanism）

##### 3.7.2.1   整体说明（General）

当协议制定者在选择PCI-X和之后的PCIe该如何访问配置空间时，一般有两个考虑。第一个，每个Function的256Byte空间限制了那些想要在这个空间内放一些专有信息的厂商，而且未来的协议制定者可能也需要更多的空间来放置更多的能力结构（capability structure）。为了解决这个问题，这个空间从每个Function 256Bytes扩展至4Kbytes。第二个要考虑的是，在PCI被开发应用的时候，多处理器系统还比较少被使用。对于旧的模型来说，当系统中仅有一个CPU且其只有一个工作线程时，使用2步来生成一次访问的这种方法并不会有什么问题。但是对于新型的使用多核多线程的CPU的计算机来说，使用IO间接访问模型就会产生一些问题，因为并没有东西来阻止多线程同一时间访问配置空间。因此，在没有锁定语意（lock semantics）时不再使用“2步”模型（IO间接访问）。在没有锁定语意（lock semantics）的情况下，当线程A往配置地址端口（CF8h）写入一个值后，在它继续对配置数据端口做相应的操作之前，并没有一个机制来防止线程B将这个值覆盖掉。为了解决这个新问题，协议制定者决定采用一个不同于IO间接访问的方法。他们不再尝试去保留地址空间，而是通过将所有配置空间都映射到内存地址，以此来创造出一个单步（single-step）、不可中断（uninterruptable）的访问过程。由于一个针对特定地址范围的memory请求会在总线上产生一个配置请求，所以这种访问方式只需要一个命令序列即可。在这种方式中进行的折中（trade-off）是地址大小。对所有实现的每个Function都映射4KB，这需要256MB的memory（内存）地址空间。在这方面，现代体系结构一般支持36到48位的物理内存地址空间。因此相较于现在的内存地址空间的总大小，256MB的空间占用很少。

为了处理这个映射，每个Function的4KB配置空间都以一个4KB对齐的地址作为起始地址，且这些4KB配置空间所映射的地址都要位于那段为了配置访问而预留的256MB内存地址空间内。并且现在的地址中的bit还携带了识别信息（identifying information），用于表示哪一个Function是访问目标（见表 3‑1）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image126.jpg)

表 3‑1增强型配置机制的内存映射地址范围

##### 3.7.2.2   一些规则（Some Rules）

如果一个访问跨了dword地址边界（跨越了相邻的两个内存DW的边界，即操作长度大于了一个DW，或者是操作长度等于一个DW但是起始地址没有位于DW对齐的地址），那么RC可以不支持它访问增强型配置内存空间。RC们也不需要支持某些总线锁定协议（bus locking protocol），这些总线锁定协议中的一些处理器典型用于原子操作或是不可中断的一系列命令。软件在访问配置空间时，需要避免上述这两种情况，除非软件知道RC可以支持它们。

 

### 3.8 配置请求（Configuration Requests）

Bridge为了响应配置请求，将会产生两种类型的配置请求，分别为Type 0和Type 1。具体产生哪一种类型的配置请求则是取决于目标总线号是否与当前Bridge的次级总线号（Secondary Bus Number）相匹配。下面将进行讲解。

#### 3.8.1     Type 0配置请求（Type 0 Configuration Request）

如果目标总线号与次级总线号相匹配，Bridge将产生一个Type 0配置读/写，并转发到它的次级总线上，并：

\1.    总线上的设备们检查配置请求中的Device Number（设备号），看谁才是这个配置请求的目标设备。需要注意，外部链路（external Link）上的EP总是Device 0。

\2.    被选中的设备检查Function Number，看看设备内的哪个Function被选中。

\3.    被选中的Function使用Register Number字段域来选择它配置空间中的DW，这个DW就是目标DW，并使用First Dword Byte Enable字段域来选出这个DW中要被进行读写操作的字节们。

如图 3‑7中给出了Type 0配置写请求和读请求的Header格式。在读写两种情况下，Type字段都是00100，而Format（Fmt）字段则用于指示是读请求还是写请求。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image128.jpg)

图 3‑7 Type 0配置读请求和写请求的Header

#### 3.8.2     Type 1配置请求（Type 1 Configuration Request）

当Bridge得到的配置请求的目标总线号并不是自己的次级总线号，但是目标总线号又在从属总线（Subordinate Bus）的范围内，那么Bridge将向次级总线转发一个Type 1请求包。次级总线上的非Bridge的设备（EP）将会忽略Type 1请求，因为这个请求的目标总线并不是当前总线。但是次级总线上的Bridge看到Type 1请求则将会进行与上一级Bridge相同的对比操作，当目标总线在从属总线范围内时：

n 如果目标总线就是当前Bridge的次级总线，这个请求包将从Type 1被转换成Type 0，并转发到次级总线上。次级总线上的本地设备将会像此前描述的一样检查请求包的Header。

n 若目标总线号不是当前Bridge的次级总线，但是在从属总线范围内，请求包将会继续以Type 1请求的格式被转发到当前Bridge的次级总线上。

如展示了Type 1配置读请求和写请求的Header格式。在读写两种情况下，Type字段域都是00101，而Format（Fmt）字段则用来指示这是读还是写。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image130.jpg)

图 3‑8 Type 1配置读请求和写请求的Header

### 3.9 PCI兼容配置访问示例（Example PCI-Compatible Configuration Access）

请参阅图 3‑9，在此图中纠正了原书版本的一个错误（该错误为errata中的第1条），Bus 6的中间的Bridge的Sub应该为9而不是8，原图中为8。

为了更好的对使用传统的CF8h/CFCh机制来产生配置请求进行说明，请参考下面的x86汇编代码，这段代码使得RC执行一个2byte的读操作，操作目标为Bus 4，Device 0，Function 0，Register 0（这个Register 0就是厂商Vendor ID）。

 

*mov  dx,0CF8h   ;将dx设置为配置地址端口的地址*

*mov  eax,8004000h ;enable bit=1，bus 4，dev 0，func 0，DW 0*

*out  dx,eax    ;IO写来设置地址端口*

*mov  dx,0CFCh   ;将dx设置为配置数据端口的地址*

*in   ax,dx    ;从配置数据端口读出2byte数据*

 

\1.    代码中的out指令将产生一个IO写操作，从处理器写入配置地址端口，这个配置地址端口位于RC的Host Bridge（0CF8h），如图 3‑4。

\2.    Host Bridge将配置地址端口中的目标总线号（这里是4）与Bridge下方的从属总线范围（这里是0到10）进行比较。很明显目标总线号在从属总线范围内，因此Bridge就准备好了接下来产生配置请求的目的地。

\3.    代码中的in指令将产生一个IO读事务，读事务产生自处理器并发往RC Host Bridge中的配置数据端口。这是一个对配置数据端口的前两个byte进行读取的读事务。

\4.    由于目标总线号不是bus 0，所以Host/PCI Bridge对bus 0发起一个Type 1配置读请求。

\5.    bus 0上的所有设备都会锁存这个事务请求，并发现它是一个Type 1配置请求。因此RC中的两个虚拟PCI-to-PCI Bridges各自都将Type 1请求中的目标总线号与下方的从属总线范围进行比较。

\6.    目的地bus 4位于左侧Bridge的下方从属总线范围中，所以左侧Bridge将把这个请求数据包发送到它的次级总线上，但是这个请求仍然是一个Type 1请求，因为目标总线并不是这个左侧Bridge的次级总线。

\7.    图左侧的Switch的上方端口将接收到这个请求数据包，并将它传送给Switch中靠上方的PCI-to-PCI Bridge。

\8.    这个Switch中靠上方的PCI-to-PCI Bridge确定目标总线在自己下方，但是并不是自己的次级总线，因此它将请求数据包继续以Type 1请求的格式转发到bus 2上。

\9.    bus 2上的两个Bridge都接收到了Type 1请求包。偏右侧的Bridge确定目标总线就是自己的次级总线。

\10.  bus 2上偏右侧的Bridge将这个配置读请求发送到bus 4上，而且这次会将原本为Type 1的请求转换成Type 0配置读请求来发送，这是因为这个请求的目标总线就是当前Bridge的次级总线。

\11.  bus 4上的Device 0接收到这个请求包，并对数据包中的目标Device号、Function号、Register号的字段进行译码，以此来选中配置空间中的目标DW。（如图 3‑3）

\12.  First Dword Byte Enable字段中的bit 0和bit 1都被置为有效，因此Function将在完成包中返回它的前两个字节（前两个字节也就是Vendor ID）。然后完成包使用从Type 0请求包中获得的Requester ID作为路由信息，通过路由转发，最终回到Host Bridge。

\13.  读出来的2byte数据将会呈交给处理器，以此来完成in指令的执行。Vendor ID的值被放置在处理器的AX寄存器中。

### 3.10     增强型配置访问示例（Example Enhanced Configuration Access）

请参阅图 3‑9。

下面的x86汇编代码将使得RC执行一个2byte的读操作，操作目标为Bus 4，Device 0，Function 0，Register 0（这个Register 0就是厂商Vendor ID）。在这段代码工作之前，Host Bridge必须已经被分配了一个基地址（base address value）。在这个例子中，我们假设增强型配置内存映射范围（Enhanced Configuration memory-mapped range）的256MB对齐基址为E0000000h。

 

*mov  ax,[E0400000h] ;内存映射配置的读操作*

 

n 地址[63:28]表示整体增强型配置地址范围的256MB对齐的基地址的高36bit，在本例中，这个整体增强型配置地址范围为00000000 E0000000h，地址[63:28]则是表示这个范围中256MB对齐基地址的高36bit。

n 地址[27:20]用于选择目标bus（本例中为bus 4）

n 地址[19:15]用于选择bus上的目标device（本例中为device 0）

n 地址[14:12]用于选择device内的目标function（本例中为function 0）

n 地址[11:2]用于选择function配置空间中的目标DW（本例中为DW 0）

n 地址[1:0]用于定义选中的DW中的起始byte的位置（本例中为byte 0）

 

处理器发起一个2byte的memory read，读取的起始地址为E0400000h，这个地址会被RC中的Host Bridge锁存下来。Host Bridge识别到这个地址位于用来进行配置的区域之内，它将产生一个配置读请求，读取的位置为Bus 4—Device 0—Function 0—dword 0—首两个字节。剩下的操作就跟前面的小节中所讲的一样了，也就是如何把读出来的信息通过完成包返回给Host。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image132.jpg)

图 3‑9配置读访问示例



 

### 3.11     枚举——搜索发现拓扑（Enumeration-Discovering the Topology）

在完成了系统上电或是复位之后，配置软件需要扫描PCIe网络结构，来搜索发现整个机器的拓扑，并学习这个网络结构是如何被填充的（例如里面都有多少总线、多少设备以及它们的编号等等）。在这进行之前，如图 3‑10所示，软件唯一知道的就是拓扑中有一个Host/PCI Bridge以及这个Bridge的次级总线Bus 0。需要注意，一个Bridge自身上方相连的总线称为主总线（Primary Bus），而这个Bridge自身下方相连的总线称为次级总线（Secondary Bus）。扫描PCIe结构来发现整体拓扑结构的过程被称为枚举过程。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image134.jpg)

图 3‑10刚启动时软件所认为的拓扑结构

#### 3.11.1    搜索某个Function是否存在（Discovering the Presence or Absence of a Function）

处理器上运行的配置软件发现一个Function的方式一般是读取这个Function的Vendor ID寄存器。PCI-SIG给每个厂商都分配了一个唯一的Vendor ID，每个厂商都会将自己设计的Function中的Vendor ID寄存器的值固定为自己的Vendor ID。通过读取整个系统中的Bus—Device—Function这三者所有组合中的Vendor ID寄存器，枚举软件可以搜索遍整个拓扑，并得知有哪些设备存在。这个过程相当的简单，但是可能会出现两个问题：目标设备可能不存在，或者它虽然存在但是没有准备好响应事务请求。下面将介绍如何处理这两种情况。

##### 3.11.1.1 设备不存在（Device not Present）

目标设备在系统中不存在的这个情况，可能会在枚举搜索过程中发生很多次。当它发生时，我们需要对它有正确的理解。在PCI中，配置读请求（Configuration Read Request）有可能在总线上执行超时（timeout），那么就会产生一个主设备放弃（Master Abort）的错误情况。由于没有设备来驱动总线，所有的信号都是拉高的，那么数据位在总线上就是全为1的，这个全为1的值将成为总线上被看见的数据值。这使得值为FFFFh的Vendor ID并没有被分配出去，而是作为一个保留值。如果枚举软件收到的读取Vendor ID的结果是FFFFh，那么它就知道它读取的设备不存在。由于这种情况其实并不是一种错误情况，因此在枚举过程中，Master Abort并不会被当做是一个错误来进行报告。

对于PCIe来说，针对一个不存在的设备的配置读请求将使得目标设备上方连接的Bridge返回一个不携带数据的完成包，这个完成包的状态字段将被置为UR（Unsupported Request，不被支持的请求）。为了向后兼容传统的枚举模型，若RC在枚举过程中收到这样的完成包，它将会给处理器返回数据FFFFh。注意，枚举软件是依赖于接收到一个返回值为全1（FFFFh）的配置读请求来判定目标设备是不存在的，而系统中的在读取这样一个不存在的设备Function的Vendor ID时，其实是返回了一个状态为Unsupported Request的不携带数据的完成包。

当出现设备不存在的情况时，也需要避免意外地发送错误报告，这很重要。尽管这种超时或者UR的结果在系统正常运行是确实是出错的情况，但是在枚举过程中它并不能被认为是错误，它是一种正常的可以预料到的结果。为了能更简单的避免这种混乱，设备们一般在这过程中都不启用错误信号，直到枚举完成才启用。对于PCIe来说，记录这种事件（目标设备不存在）仍然是有作用的，这也就是为什么PCIe能力寄存器块（PCIe Capability register block）中存在第4个“错误”状态位，被称为Unsupported Request Status不受支持的请求状态（更多关于这部分的内容请见“Enabling/Disabling Error Reporting”一节）。由于有这个状态位的存在，发生目标设备不存在的情况就可以被记录下来，而且不会被当做是一个错误，这一点非常重要，因为若检测到一个错误那么枚举过程将被停止并调用系统错误处理程序。在枚举还没进行完的这个时间段，错误处理软件的能力可能十分有限，使得问题无法被解决。这将造成枚举软件运行失败，因为通常是在操作系统（OS）或者其他错误处理软件可用之前就已经执行枚举软件。为了避免这种风险，在枚举期间，通常不应该报告错误。

##### 3.11.1.2 设备未准备好（Device not Ready）

前面提到的可能会发生的另一个问题就是目标设备虽然存在，但是可能并未准备好响应配置访问。对于配置操作来说，需要考虑发起配置的时间点，因为设备准备好被访问是需要时间的。如果数据速率小于等于5.0GT/s，软件必须在复位后等待100ms再发起配置请求。如果数据速率高于5.0GT/s（Gen3速率），软件必须在链路训练（Link Training）完成100ms之后再尝试发起配置操作。之所以更高的速率需要更多的延时（Gen3需要在链路训练完成后等待100ms而不是复位完成等待100ms），这是因为Gen3的链路训练中的均衡过程（Equalization Process）可能需要比较长的时间（约50ms，关于这里的更多内容请参阅“Link Equalization Overview”一节）。

在PCI 2.3中定义了初始化时间（Initialization Time，Trhfa-从复位被置为高释放到第一个访问所经历的时间），初始化时间起始于RST#被置为无效，结束于225个PCI时钟周期之后。这相当于整整一秒钟，在这一秒内，Function正在为响应它的第一次配置访问做准备，并且这个时间值也被PCIe所使用，在PCIe中为1.0s（+50%/-0%）。一个Function可以利用这段时间来填充自己的配置寄存器，例如这一过程可以通过加载一个外挂的串行EEPROM的内容来实现。加载EEPROM内容需要花费一点时间，在这段时间内Function都没有准备好响应配置请求，直到加载完成。在PCI中，若在Function准备好之前就收到了一个配置访问，那么它有三个选择：忽略这个请求、重试（Retry）这个请求、接收这个请求但是延期响应直到Function自身完全准备好为止。最后一种选择的响应可能会给热插拔（Hot-plug）系统带来麻烦，因为延期响应的时长可能会达到1s，在这1s中总线都是停止的，直到这个请求被执行完，总线才重新活动起来。

在PCIe中我们也面临着相同的问题，但是问题的过程有一点不同。首先，PCIe Function在临时无法响应配置访问时必须要给出一个完成包，这个完成包也要被置为指定的状态，这个状态为Configuration Request Retry Status（CRS，配置请求重试状态）。这个状态只有在响应配置请求的时候才是合法的，如果其他的请求收到了这个状态的完成包，则可能会认为数据包格式错误（Malformed Packet error）。这种状态的响应也只在复位后的1秒后有效，因为在1秒之后Function就被认为是可以正常响应配置请求了，如果1秒之后这个Function都还无法正常响应配置请求那么就认为它已经损坏了。

除了系统复位之后的那一段时间之外，RC处理配置读请求的CRS（配置重试状态）完成包的方式是特定实现的而不是通用的。而在系统复位后的那一小段时间里，RC有两个可以进行的操作供选择，选择哪一种则取决于Root控制寄存器（Root Control Register）中CRS Software Visibility这一bit的值（如图 3‑11）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image136.jpg)

图 3‑11 PCIe能力块中的Root控制寄存器

n 如果这个bit被置为1，而且发出的请求是配置读请求，要读取Vendor ID寄存器的全部两个byte（枚举过程通过这样的操作来搜索发现一个Function是否存在），那么在接收到CRS完成包时RC需要给Host返回伪造的0001h作为读取的寄存器值，并将其他的所有字节都置为全1。这个Vendor ID并没有被任何真实的设备所使用，软件将把这个0001h的Vendor ID理解为访问这个设备可能需要很长的延迟。这个信息很有用，因为软件就可以选择去执行其他的任务，更充分的利用这段等待设备响应的时间，稍后再返回来查询这个设备。要进行这样子的操作，软件必须确保其在复位后对Function的第一次访问就是读取2 byteVendor ID的配置读访问。

n 对于配置写访问或者是其他的配置读访问（并不是读2byte Vendor ID），RC必须自动地以一个新请求的形式对之前的配置请求进行重新下达。

#### 3.11.2    确认某个Function是EP还是Bridge

枚举过程的一个关键部分就是可以确定一个Function是Bridge还是EP。如图 3‑12所示，Header类型寄存器（Header Type Register，位于配置空间Header的偏移地址0Eh）的低7bit用于标识这个Function的基本种类，总共定义了三种不同的值：

n 0 = 不是一个Bridge（那也就是一个PCIe EP）

n 1 = PCI-to-PCI Bridge（缩写为P2P），用于连接两条总线

n 2 = 插件卡Bridge（CardBus Bridge，现在很少使用的遗留接口）

在图 3‑1中，每个虚拟P2P的Header Type字段（DW3，byte2）的返回值都为1，且PCI Express-to-PCI Bridge（Bus 8，Device 0）的Header Type字段也将返回1。但是对于EP来说，它返回的Header Type字段是0。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image138.jpg)

图 3‑12 Header类型寄存器

### 3.12     单RC枚举示例（Single Root Enumeration Example）

现在，让我们通过一个例子来讨论一下枚举过程中的基本要素。如图 3‑13展示了一个经过总线、设备枚举后的系统示例。接下来的讨论基于这样的假设：配置软件将使用本章节定义的两种配置访问机制中的一种来完成枚举过程。在系统启动时，处理器上执行的配置软件将如接下来所讲述的那样来进行枚举过程。

\1.    软件将Host/PCI Bridge的次级总线号（Secondary Bus Number）更新为0，并将从属总线号（Subordinate Bus Number）更新为255。之所以将其设置为最大值是因为Sub Num可以这样一直保持不变，直到Host/PCI Bridge下方的所有总线都被识别出来，然后再更新成准确的值。在此时此刻，暂且认为在Host/PCI Bridge下方识别到的总线范围为Bus 0到255。

\2.    从Device 0开始，枚举软件将会尝试去读取Bus 0上的32个可能存在的Device，读取的内容为它们各自的Function中的Vendor ID。如果Bus 0—Device 0—Function 0返回了一个有效的Vendor ID，那么就认为这个设备存在并至少含有一个Function。若Bus 0—Device 0—Function 0没有返回一个有效的Vendor ID，则继续去探测Bus 0—Device 1—Function 0。

\3.    在本例中的值为1的Header Type字段（如图 3‑12）表示这是一个PCI-to-PCI Bridge。Header Type寄存器中的Multifunction位（bit 7）为0，这表示Function 0就是这个Bridge中唯一的Function。协议规范其实并没有阻止在Bridge这样的设备中去实现多个Function，相反地，当有多个Function时每个Function都可以作为又一个虚拟PCI-to-PCI Bridge，甚至还可以是一个非Bridge的Function。

\4.    现在软件已经找到了一个Bridge，并执行了一系列配置写操作来将这个Bridge的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=0

n 次级总线号（Secondary Bus Number）=1

n 从属总线号（Subordinate Bus Number）=255

现在，这个Bridge就认为在它下方直接相连的总线的编号为1（次级总线号=1），它下方从属的最大总线号是255（从属总线号=255）。

\5.    枚举软件必须进行深度优先（depth-first）的搜索。在继续发现Bus 0上的其他Device/Function之前，枚举软件必须先搜索Bus 1。

\6.    软件读取Bus 1—Device 0—Function 0的Vendor ID，这个目标设备在我们的示例图 3‑13中为Bridge C。这次读取将返回一个有效的Vendor ID，这表示在Bus 1上存在Device 0—Function 0。

\7.    在Header寄存器中的Header Type字段的值为1（0000001b），这表示它是又一个PCI-to-PCI Bridge。像之前一样，bit 7的值为0（Multifunction bit=0），表示这个Bridge C是一个单Function设备。

\8.    软件现在执行一系列的配置写操作来将这个Bridge C的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=1

n 次级总线号（Secondary Bus Number）=2

n 从属总线号（Subordinate Bus Number）=255

\9.    软件继续进行深度优先的搜索（depth-first search），读取Bus 2—Device 0—Function 0的Vendor ID。在本例中（图 3‑13），Bus 2上的Device 0—Function 0是Bridge D。

\10.  读请求返回了一个有效的Vendor ID，表示Bus 2—Device 0—Function 0存在。

\11.  在Header寄存器中的Header Type字段的值为1（0000001b），这表示它是又一个PCI-to-PCI Bridge，且bit 7的值为0（Multifunction bit=0），表示这个Bridge D是一个单Function设备。

\12.  软件现在执行一系列的配置写操作来将这个Bridge D的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=2

n 次级总线号（Secondary Bus Number）=3

n 从属总线号（Subordinate Bus Number）=255

\13.  继续进行深度优先的搜索（depth-first search），读取Bus 3—Device 0—Function 0的Vendor ID。在本例中（图 3‑13），Bus 2上的Device 0—Function 0是Bridge D。

\14.  返回了一个有效的Vendor ID，表示Bus 3—Device 0—Function 0存在。

\15.  这次有了不同，在Header寄存器中的Header Type字段的值为0（0000000b），这表示它是一个Endpoint Function（EP）。由于这个Function是一个EP而不是一个Bridge，它有一个Type 0 Header，且在这个EP之下就没有别的PCI兼容总线了（PCI-Compatible Bus）。这个EP的bit 7的值为1（Multifunction bit=1），表示这个EP是一个多Function设备。

\16.  枚举软件对Bus 3—Device 0上的全部可能的8个Function进行访问，访问它们各自的Vendor ID，然后软件确认除了Function 0之外只存在Function 1。Function 1也是一个EP（Type 0 Header），因此在这个设备（Bus 3—Device 0）之下并不存在一个附加的总线。

\17.  枚举软件继续在Bus 3上进行扫描，寻找Device 1到Device 31上有效的Function，然而并没有找到任何其他的Function。

\18.  枚举软件完成了寻找Bridge D之下所有Function的任务之后，就会更新Bridge D的一些信息，也就是将从属总线号从预留的255更新为真实值3。然后，枚举软件回到上一个总线层级（Bus 2），并继续在这个总线上扫描，寻找有效的Function。在本例中（图 3‑13），Bus 3上的Device 1—Function 0是Bridge E。

\19.  配置读请求返回了一个有效的Vendor ID，表示Bus 2—Device 1—Function 0存在。

\20.  在Header寄存器中的Header Type字段的值为1（0000001b），这表示它是又一个PCI-to-PCI Bridge，且bit 7的值为0（Multifunction bit=0），表示这个Bridge E是一个单Function设备。

\21.  软件现在执行一系列的配置写操作来将这个Bridge E的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=2

n 次级总线号（Secondary Bus Number）=4

n 从属总线号（Subordinate Bus Number）=255

\22.  软件继续进行深度优先的搜索（depth-first search），读取Bus 4—Device 0—Function 0的Vendor ID。

\23.  返回了一个有效的Vendor ID，表示Bus 4—Device 0—Function 0存在。

\24.  在Header寄存器中的Header Type字段的值为0（0000000b），这表示它是一个EP设备，且bit 7的值为0（Multifunction bit=0），表示这个设备是一个单Function设备。

\25.  枚举软件继续在Bus 4上进行扫描，寻找Device 1到Device 31上有效的Function，然而并没有找到任何其他的Function。

\26.  枚举软件已经到达了这个搜索分支的底部，因此它会更新当前总线之上相连的Bridge，在这里为Bridge E，将其从属总线号从预留的255更新为真实值4。然后，枚举软件回到上一个总线层级（Bus 2），并继续读取该总线上的下一个设备（Device 2）的Vendor ID。在本例中（图 3‑13），Bus 2上并没有实现Device 2到Device 31，因此枚举软件将不会发现Bus 2上还有其他的设备，仅有Device 0和Device 1。

\27.  枚举软件更新Bus 2之上相连的Bridge，在这里为Bridge C，将其从属总线号从预留的255更新为真实值4，并回到上一个总线层级（Bus 1），继续去读取该总线上的下一个设备（Device 1）的Vendor ID。在本例中（图 3‑13），Bus 1上并没有实现Device 1到Device 31，因此枚举软件将不会发现Bus 1上还有其他的设备，仅有Device 0。

\28.  枚举软件更新Bus 1之上相连的Bridge，在这里为Bridge A，将其从属总线号从预留的255更新为真实值4，并回到上一个总线层级（Bus 0），继续去读取该总线上的下一个设备（Device 1）的Vendor ID。在本例中（图 3‑13），Bus 0上的Device 1—Function 0是Bridge B。

\29.  如前面所述的相同的过程，枚举软件发现Bridge B，并执行一系列的配置写操作来将这个Bridge B的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=0

n 次级总线号（Secondary Bus Number）=5

n 从属总线号（Subordinate Bus Number）=255

\30.  然后枚举软件又在Bus 5上发现Bridge F，并执行一系列的配置写操作来将这个Bridge F的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=5

n 次级总线号（Secondary Bus Number）=6

n 从属总线号（Subordinate Bus Number）=255

\31.  接着枚举软件又在Bus 6上发现Bridge G，并执行一系列的配置写操作来将这个Bridge G的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=6

n 次级总线号（Secondary Bus Number）=7

n 从属总线号（Subordinate Bus Number）=255

\32.  当枚举软件继续在Bus 7上搜索，它发现了一个单Function的EP设备，Bus 7—Device 0—Function 0，因此将Bridge G的从属总线号更新为7。

\33.  枚举软件向上回到Bus 6继续进行搜索，然后它发现了Bridge H，并执行一系列的配置写操作来将这个Bridge H的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=6

n 次级总线号（Secondary Bus Number）=8

n 从属总线号（Subordinate Bus Number）=255

\34.  然后枚举软件又在Bus 8上发现Bridge J，并执行一系列的配置写操作来将这个Bridge J的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=8

n 次级总线号（Secondary Bus Number）=9

n 从属总线号（Subordinate Bus Number）=255

\35.  Bus 9上的所有设备以及与它们相关的Function都已经全部被搜索完成，它们都不是Bridge，因此下方也就没有附加的总线，所以将Bridge H和Bridge J的从属总线号更新为9。

\36.  枚举软件向上回到Bus 6继续进行搜索，然后它发现了Bridge I，并执行一系列的配置写操作来将这个Bridge I的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=6

n 次级总线号（Secondary Bus Number）=10

n 从属总线号（Subordinate Bus Number）=255

\37.  当枚举软件继续在Bus 10上搜索，它发现了一个单Function的EP设备，Bus 10—Device 0—Function 0。

\38.  由于枚举软件已经到达了这个搜索分支的底部，这个底部是也是整个PCIe拓扑树状结构的底部，因此Bridge B、F、I的从属总线号会被更新为10，而Host/PCI Bridge也会将从属总线寄存器更新为这个数值。

 

每个Bridge中最终的主总线号、次级总线号和从属总线号可以参阅图 3‑9。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image140.jpg)

图 3‑13单Root系统

### 3.13     多RC枚举示例（Multi-Root Enumeration Example）

#### 3.13.1    整体说明（General）

考虑如图 3‑14所示的多RC系统。在这个系统中，每个RC：

n 实现了各自的配置地址端口（Configuration Address Port）和配置数据端口（Configuration Data Port），并且各个RC中的这些相应的端口的IO地址是相同的，比如RC 0和RC 1的配置地址端口的IO地址相同。

n 都实现了增强型配置机制。

n 都包含一个Host/PCI Bridge

n 实现了各自的次级总线号寄存器和从属总线号寄存器，但是不同RC的这两个寄存器对于配置软件来说是不同的操作地址。

在图示中，每个RC都是一个芯片组成员，这其中的一个RC被设计为主RC，它的Bridge下方相连的总线为Bus 0，它被称为主RC（Primary Root Complex）。而另一个RC的Bridge下方相连的总线则是被暂时认为是Bus 255，它被称为次级RC（Secondary Root Complex）。

#### 3.13.2    多RC枚举过程（Multi-Root Enumeration Process）

在对图 3‑14中左侧的树状结构进行枚举的过程中，次级RC的Host/PCI Bridge将会忽略所有的配置访问，因为所有的目标总线号中最大的也不会超过9。需要注意，虽然Bus 8被检测到了并且也被编了号，但是这条总线上并没有连接设备。一旦左侧树状结构的枚举过程完成，枚举软件就会进行如下的这些步骤来对次级RC进行枚举：

\1.    在本例中（图 3‑14），枚举软件将会把次级RC中Host/PCI Bridge的次级总线号和从属总线号都更改为64。（64和128这两个值在多RC系统中被经常用来作为起始总线地址，但是这只是软件的一个习惯，在PCI或者PCIe的协议规范中并没有对这种配置方式有硬性规定。比如在本例中即使给次级RC的起始总线地址设置为10，也不会引起任何错误。）

\2.    枚举软件开始在Bus 64上进行搜索，并发现了与RC下方端口相连的Bridge。

\3.    然后枚举软件执行一系列的配置写操作来将这个Bridge的总线号寄存器设置为如下值：

n 主总线号寄存器（Primary Bus Number Register）=64

n 次级总线号（Secondary Bus Number）=65

n 从属总线号（Subordinate Bus Number）=255

现在，这个Bridge就认为在它下方与它直接相连的总线编号为65（次级总线号=65），并且它下方最大的总线号为255（从属总线号=255）。（这里原书有误，原书写其下方的最大总线号为65，但是应该是255，只不过之后这个号会被更新为65）

\4.    枚举软件继续在Bus65上进行搜索，并发现了Device 0，且它仅有一个Function 0。进一步搜索的结果显示，在Bus 65上并没有其他的Device了，因此搜索过程返回到上一级总线。

\5.    枚举软件回到Bus 64继续进行，然而也没有发现任何其他的设备，因此就将Host/PCI Bridge的从属总线号更新为65。

\6.    这样就完成了次级RC的枚举过程。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image142.jpg)

图 3‑14多Root系统

#### 3.13.3    热插拔注意事项（Hot-Plug Considerations）

热插拔，指的是一个插件卡（add-in card）可以在系统运行时插入或拔出。在这种热插拔环境中，对于图 3‑14中的Bus 8来说，可能带来潜在的麻烦。若系统已经完成枚举，并已经正常的开始运行了，这时将一个含有Bridge的插件卡插到Bus 8上，则有可能会引发问题。这个插件卡内的Bridge分配给自己的次级总线号和从属总线号将会比自己的主总线上的其他从属总线的编号还要大，并且将完全包含这些总线号，意思就是说，Bus 8下方若出现了新总线，那么这个新总线可能要被分配为Bus 9，而这就与已经存在的Bus 9产生了冲突。这是因为新的总线号必须在插件卡上方的Bridge的从属总线范围内，对于本例（图 3‑14）来说也就是在6-9的范围内。

一种用来分配Bus 8下方这些新增的总线号的方法是，将当前的Bus 9的编号增大，为Bus 8下的新增总线号腾出编号的空间。尽管在系统运行时调整总线号是可以做到的，但是据有经验的人说，这样的方法很难工作的很好。

还有一种简单的方法来解决这个潜在的问题：只需要在发现未填充的插件卡插槽时，简单地留出一段总线编号间隔（bus number gap）即可。例如，当Bus 8被分配了总线号8，但是在它下方发现了一个空的插件卡插槽，那么就给下一个被发现的总线一个更高的总线号，比如给它编号19而不是9，这样就为Bus 8之下以后可能的新增总线留出了编号空间。然后，如果一个带有Bridge的插件卡被插入，那么新增的总线就可以从Bus 9开始编号，而不会产生任何问题。在大多数情况下，留出这样的总线编号间隔并不会引发问题，因为系统总共可以分配256个总线号，编号资源较为充足。

### 3.14     MindShare Arbor：调试/验证/分析和学习的软件工具

#### 3.14.1    整体说明（General）

MindShare Arbor是一个计算机系统调试（Debug）、验证（Validation）、分析（Analysis）和学习的工具，它允许用户对任意的memory、IO或者配置空间的地址进行读取或者写入。读取这些地址返回回来的数据将以一个简洁明了且信息丰富的样式进行查看。

本书的作者决定不在某一个标志性的特定章节中对所有的配置寄存器都进行详细描述。相反地，各个寄存器将分散在全书中，在与它们各自相关的章节中进行详细讲述。

MindShare Arbor是一个很棒的参考学习工具，可以用它来代替书中没有的配置寄存器空间的专用描述章节。通过MindShare Arbor，我们可以快速的理解PCI、PCI-X以及PCI Express中实现的配置寄存器和结构。所有的寄存器和字段定义都是按照最新版本的PCI Express协议进行更新的。一些其他种类的结构（例如x86 MSRs、ACPI、USB、NVM Express）也可以在MindShare Arbor中进行查看（一些暂时没有的也会很快就可以支持查看）。

访问[www.mindshare.com/arbor](http://www.mindshare.com/arbor)即可免费下载MindShare Arbor的试用版。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image144.jpg)

图 3‑15 MindShare Arbor的部分截图

#### 3.14.2    MindShare Arbor功能列表

n 包含PCIe 3.0协议中的所有配置寄存器的描述内容。

n 可以扫描一个系统中所有PCI可见Function的配置空间，并以可读性极强的格式将每个寄存器的讲解都展示出来。

n 可以直接访问任何的memory或者IO地址。

n 可以写入任意的配置空间位置、memory地址或是IO地址。

n 可以在一个已经解码的格式中查看标准或非标准的结构体

o 解码信息可以给标准的PCI、PCI-X、PCI Express结构体进行解码。

o 解码信息可以给一些基于x86的结构体和设备特别的寄存器进行解码。

n 可以创建自己定义的基于XML的译码文件，来驱动Arbor的信息显示。

o 可以创建译码文件，来对配置空间、内存地址空间和IO空间内的结构体进行译码。

n 可以保存系统扫描的结果，并之后再在其他系统上查看这个结果。

o 保存的系统扫描结果是基于XML的开源格式的文件。

n 已经拥有或者即将拥有的新功能特性

o 可以检查几次扫描结果之间的差异。

o 可以为扫描中的违规或者非最佳的设置进行后期处理。

o 可以支持脚本来达到自动化。

o 可以对x86结构体进行译码（MSRs、paging、segmentation、interrupt tables等等）。

o 可以对ACPI结构体进行译码。

o 可以对USB结构体进行译码。

o 可以对NVM Express结构体进行译码。

 



## Chapter 4      Address Space & Transaction Routing//地址空间与事务路由

### 关于前一章

前一章节对PCIe环境中的配置操作进行了介绍。这包括那个用来实现Function配置寄存器的空间、一个Function是如何被搜索发现的、配置事务是如何被生成并路由转发的、PCI兼容配置空间和PCIe扩展配置空间的不同点，以及软件是如何辨认区分开EP和Bridge的。

### 关于本章

本章节将对描述一个Function通过Base Address Registers（BARs，基地址寄存器）来请求地址空间（memory地址空间或IO地址空间）的目的和方法，以及软件必须怎样设置所有的Bridge内的Base/Limit寄存器来使得TLPs能从一个源端口被路由转发至正确的目的端口。另外，也将会讨论PCIe中TLP路由的一般概念，包括基于地址的路由、基于ID的路由和隐式路由（implicit routing）。

### 关于下一章

下一章的将详细的对TLP（Transaction Layer Packet，事务层包）的内容进行描述。我们将描述它的用途、格式、TLP Type的定义以及其他一些相关字段的详细内容。

### 4.1 我需要一个地址（I Need An Address）

几乎所有的设备中都有能够让软件（或是其他潜在的设备）访问的内部寄存器或是存储空间位置。这些内部位置有可能用来控制设备的行为、报告设备的状态，或者可能用来保持住设备要进行处理的数据。这里先不管这些内部寄存器/存储的目的，我们这里更看重的一点是要可以从设备外部来对它们进行访问。这意味着这些内部位置需要是可寻址的（addressable）。软件必须要可以执行一个带有地址的读或这写操作，以此来访问目标设备内相应的内部位置。为了让这种方式可行，这些内部位置都需要被分配地址，并且所分配的地址也需要来自系统所支持的地址空间。

PCIe支持三个地址空间，它们与PCI中的三个地址空间完全相同：

n 配置空间（Configuration）

n 内存地址空间（Memory）

n IO地址空间（IO）

#### 4.1.1     配置空间（Configuration Space）

如我们在Chapter 1中所见到的，配置空间是由PCI引入的，软件通过配置空间就可以用一种标准化的方法来对设备的状态进行控制和检查。PCIe对PCI软件具有向后兼容性，所以PCIe中仍然支持配置空间，并且支持它的原因也和PCI中一样，即用一种标准化的方法来对设备的状态进行控制和检查。更多关于配置空间的信息（目的、如何访问、大小、内容等等）请参阅Chapter 3。

尽管配置空间出现的意义是来放置和保持一些标准化的结构（PCI-defined Header、Capability Structure能力结构等等），但是PCIe设备也会经常的将一些设备特定（device-specific）的寄存器映射到设备自身的配置空间中。在这种情况下，映射到配置空间的设备特定寄存器常用来作为控制寄存器（control）、状态寄存器（status）或者指针寄存器（pointer），而不是数据存储位置。

#### 4.1.2     内存和IO地址空间（Memory and IO Address Spaces）

##### 4.1.2.1   整体说明（General）

在PC的早期阶段，IO设备的内部寄存器/存储是通过IO地址空间（IO Address，它是由intel定义的）来访问的。然而，由于IO地址空间的一些限制和不良影响（在这里我们暂不讨论），这种地址空间很快就失去了软件和硬件厂商的青睐。这使得IO设备的内部寄存器/存储被映射到了memory地址空间（Memory Address Space，内存地址空间），一般被称作内存映射IO或者简称MMIO（Memory-Mapped IO）。然而，因为早期的软件是使用IO地址空间来访问IO设备的内部寄存器/存储的，所以实际中常用的做法是将一套设备特定寄存器既映射到内存地址空间，也映射到IO地址空间。这使得新的软件可以使用内存地址空间，也就是通过MMIO，来对设备的内部位置进行访问。而传统（旧）的软件也依然可以运行，因为它依然可以通过IO地址空间来访问设备的内部寄存器。

对于更加新型的设备，如果它们不再依赖老旧的传统软件并且也不需要考虑对传统操作的兼容问题，那么它们一般只要将内部寄存器/存储映射到内存地址空间（MMIO）即可，而不需要请求IO地址空间来进行映射。实际上，PCIe协议中不鼓励使用IO地址空间，它仅仅是受一些传统遗留原因的支持，并有可能在未来新版本的协议中被弃用。

如图 4‑1所示，图中展示了一种通用的内存和IO的映射。内存映射的大小是这个系统可使用地址范围（通常由CPU的可寻址范围决定）的一个函数。PCIe中IO映射的大小被限制为32bits（4GB），虽然其实很多使用Intel兼容（x86）处理器的计算机中仅有低16bit（64KB）被使用。PCIe可以支持的内存地址大小达到64bit。

图 4‑1给出的映射示例仅展示了EP所声明使用的MMIO和IO，但是这种能力并不是EP所独有的。它对于Switch和RC来说也是一种很普通的能力，Switch和RC内部也存在着可以通过MMIO和IO地址来进行访问的设备特定寄存器。

##### 4.1.2.2   可预取的与不可预取的内存空间的对比（Prefetchable vs. Non-Prefetchable Memory Space）

图 4‑1展示了被PCIe设备声明的两种不同类型的MMIO：可预取MMIO（Prefetchable MMIO，P-MMIO）和不可预取MMIO（Non-Prefetchable MMIO，NP-MMIO）。了解这两种内存空间之间的区别是十分重要的。可预取空间有两个意义十分明确的属性：

n 读操作不存在副作用。（Reads do not have side effects）

n 允许写合并（Write merging is allowed）

将MMIO的一个区域定义为可预取的，这样就可以推测性的提前获取该区域中的数据，以预测Requester在不久的将来可能需要比当前实际请求更多的数据。之所以这种小型的Caching（Minor Caching）数据操作是安全的，是因为读取这些数据并不会改变目标设备的任何状态信息，也就是说读取某个位置并不会带来副作用。例如，如果一个Requester请求从一个地址中读取128byte数据，那么Completer可以在给出此次请求的128byte之后也预取出下一个128byte，而当下一个128byte被请求时数据早已经被Completer从内存空间中预取出来了，这样就提高了性能。然而如果Requester再也没有请求额外的数据，那么Completer就需要将预取的数据清除掉，释放自身的Buffer空间。如果读取数据的行为改变了对应地址上的数值（或者有其他的什么副作用），那么就将会无法恢复被清除的数据了。然而对于可预取空间来说，读取行为并没有任何副作用，因此它总是可以回退并且在稍后也能获得原始的数据，因为原始的数据一直都不变的存放在那里。

你也许会想知道什么样的内存空间会存在读取操作的副作用。举一个例子，一个内存映射的状态寄存器，它被设计成在读取时自动清除自己，这样可以减少程序员在读取这个状态后还需要额外步骤来清除这些比特的操作。

做这种可预取和不可预取的区分，对于PCI的意义要大于PCIe，因为PCI总线协议中的事务并没有包含传输量大小的信息。如果设备都在同一条总线上交换数据，那么没有传输量大小的信息也不是什么问题，因为在同一总线上会有实时的握手信号来指示Requester什么时候收到了足够的数据完成了事务。但是如果数据的传输需要跨过一个Bridge来到另一条总线，那么情况就不像刚才一样简单了，因为对于跨到不同总线的读操作来说，Bridge在收集另一条总线上的数据时需要去猜测传输数据总量。如果猜错了传输量的大小则会增加延时而降低性能，因此在这种情况下若拥有预取的权限则对提升性能会有不小的帮助。这就是为什么将内存空间指定为可预取的概念在PCI中很有好处。由于PCIe请求中包含了传输量大小的信息，所以可预取空间并不像以前在PCI中那样引人关注了，但是为了向后兼容性，PCIe还是继承了这种概念。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image146.jpg)

图 4‑1通用的Memory与IO的地址映射

### 4.2 基地址寄存器BARs（Base Address Registers）

#### 4.2.1     整体说明（General）



一个系统中的每个设备对地址空间的数量和类型可能都有不同的要求。例如，一个设备可能有256byte大小的内部寄存器/存储需要通过IO地址空间来访问，而另一个设备中可能有16KB的内部寄存器/存储需要通过MMIO来访问。

基于PCI的设备不允许自己来决定哪些地址可以用来访问它们内部的位置，这系统软件负责的工作（例如BIOS和操作系统内核）。因此设备必须为系统软件提供一个途径用来确定设备对地址空间的需求。一旦软件知道了设备对地址空间的需求是什么样的并假设这个需求是可以被满足的，软件就会给对应的设备分配一段可用的地址范围和相应的地址空间类型（IO、NP-MMIO、P-MMIO）。

这些都是通过配置空间Header中的基地址寄存器BARs（Base Address Registers）来完成的。如图 4‑2所示，一个Type 0 Header拥有6个可用的BAR（每个大小为32bit），而一个Type 1 Header只拥有2个BAR。Type 1 Header是存在于所有的Bridge设备中的，这意味着每个Switch端口和RC端口都会拥有1个Type 1 Header。Type 0 Header只存在于非Bridge设备中，例如EP。关于这里的一个例子可以参阅图 4‑3。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image148.jpg)

图 4‑2配置空间中的BARs

系统软件必须首先确定设备所需地址空间的大小和类型。设备的设计者知道设备中西药通过IO或者MMIO访问的内部寄存器/存储的总体大小。设备的设计者还知道当这些寄存器被访问时设备将会如何工作（例如读取操作是否有副作用）。这将决定设备所需要的是可预取MMIO（读取操作无副作用）还是不可预取MMIO（读取操作有副作用）。知道了这个信息之后，设备设计者将BARs的低位bit固定为某个值，以此来指示需要请求的地址空间的大小和类型。

BARs的高位bit是软件可进行写入的。一旦系统软件通过检查BARs的低位bit确定了设备所请求的地址空间的大小和类型，系统软件就会将分配给这个设备的地址范围的基地址写入BAR中。由于一个EP（使用Type 0 Header）拥有6个BARs，它最多可以请求6个不同的地址空间。然而，一般实际中请求6个不同的地址空间并不常见。绝大多数设备会请求1到3个不同的地址范围。

并不是所有的BARs都需要被实现。如果一个设备并不需要使用所有的BAR来映射自己的内部寄存器，那么多余的BARs将会被固定为全0，以此来通知软件这些BARs并没有被实现。

一旦BARs被编程写入（programmed），设备内的内部寄存器或者本地内存（local memory）就可以通过BARs中所写入的地址范围进行访问。任何时候，当设备发现一个请求事务的地址是映射到自己的一个BAR时，它就会接收这个请求事务，因为它自己就是这个请求的目标设备。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image150.jpg)

图 4‑3 PCIe设备中对Type 0、Type 1 Header的用法

#### 4.2.2     BAR示例1——32bit内存地址空间请求



图 4‑4展示了设置建立一个BAR的基础步骤，在本例中，要请求一个4KB大小的不可预取内存（NP-MMIO，non-prefetchable memory）。在图中，展示了BAR在配置过程中的三个节点：

\1.    在图 4‑4中的（1），我们可以看到BAR处于未初始化的状态。设备的设计者已经将低位bit固定为一个数值，来指示需要的memory的大小和类型，但是高位bit（可写可读的）则仍然是用X来表示，这代表它们的值还未知。系统软件将会首先把每个BAR都通过配置写操作来将可写入的bit写为全1（当然，被固定的低位bit不会受到配置写操作的影响）。在图 4‑4的（2）中展示的BAR就是处于第二阶段的样子，除了被固定的低位bit以外，所有的bit都被写为1。

写为全1这个操作是为了确定最低位的可写入的bit（least-significant writable bit）是哪一位，这个bit的位置指示了需要被请求的地址空间的大小。在本例中，最低位的可写入的bit为bit 12，因此这个BAR需要请求212（或者说是4KB）的地址空间。如果最低位的可写入的bit为bit 20，那么这个BAR就要请求220（1MB）的地址空间。

\2.    在软件将BARs中所有可写bit都写为1后，软件将从BAR0开始，依次读取每个BAR的数值，以此来确定各个BAR要请求的地址空间的大小和类型。表 4‑1中总结了本例中对BAR0进行配置读的结果。

\3.    这个过程中的最后一步就是系统软件为BAR 0分配一个地址范围，因为对于软件来说现在已经知道了BAR 0请求的地址空间的大小和类型。图 4‑4的（3）中展示了BAR处于第三阶段的样子，此时系统软件已经将一块地址区域的起始地址写入了BAR 0中。在本例中，这个起始地址为F900_0000h。

 

到这里为止，对BAR 0的配置就完成了。一旦软件启用了命令寄存器（Command register，偏移地址04h）中的内存地址译码（memory address decoding），那么这个设备就会接受所有地址在F900_0000h-F900_0FFFh（4KB大小）范围内的memory请求。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image152.jpg)

表 4‑1对BAR 0写入全1后再读取BAR 0时的结果

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image154.jpg)

图 4‑4设置建立32bit不可预取内存的BAR

#### 4.2.3     BAR示例2——64bit内存地址空间请求

在上一个例子中，我们看到了通过BAR 0来请求不可预取内存地址空间（NP-MMIO）。在当前的例子中，如图 4‑5所示，BAR 1和BAR 2被用来请求一块64MB的可预取内存地址空间。这里使用了两个连续相连的BAR是因为这个设备支持这个内存地址空间的请求使用64bit地址，这意味着如果需要的话，软件给它分配的地址空间可以超过4GB地址边界（并非必要）。由于地址可以是64bit位宽，因此必须将两个连续相连的BAR一起使用。

跟上面一样，将BAR在配置过程中分为3个节点：

\1.    在图 4‑5中的（1），我们可以看到一对BAR都处于未初始化的状态。设备的设计者已经将低位BAR（在本例中为BAR 1）中的低位bit固定为一个数值，来指示需要的memory的大小和类型，但是高位BAR（BAR 2）中的bit则都是可读可写的，没有被固定。系统软件的第一步就是把每个BAR都通过配置写操作来将可写入的bit写为全1。在图 4‑5的（2）中展示的BAR就是处于第二阶段的样子，除了BAR 1中被固定的低位bit以外，所有的bit都被写为1。

\2.    如上一个例子所述，系统软件已经评估过BAR 0。因此软件的下一步就是读取下一个BAR（BAR 1），并对其进行评估，以此来确定设备是否正在请求更多的地址空间。一旦BAR 1被读取，软件就会发现设备正在请求更多的地址空间，并且所请求的地址空间类型为可预取内存地址空间（P-MMIO），这个地址空间可以被分配为64bit地址范围的任何一个位置。由于这个地址空间请求支持64bit地址，那么下一个连续相连的BAR（本例中为BAR 2）就会被当做是BAR 1的高32bit。因此软件也会读取BAR 2的内容。然而，软件并不会对BAR 2进行像对BAR 1一样的低位bit的评估，因为软件仅仅是将BAR 2简单的当做BAR 1发起的64bit地址请求的高32bit。表 4‑2中总结了本例中对两个BAR都写入全1后再进行配置读的读取结果。

\3.    这个过程中的最后一步就是系统软件为这一对BAR分配一个地址范围，因为对于软件来说现在已经知道了这一对BAR请求的地址空间的大小和类型。图 4‑5的（3）中展示了这两个BAR处于第三阶段的样子，软件已经通过两次配置写操作将分配的地址区域的64bit起始地址写入BAR 1与BAR 2的组合体中。在本例中，高位BAR的bit 1（BAR pair的bit 33）被置为1，低位BAR的bit 30（也是BAR pair的bit 30）也被置为1，这表示其实地址为2_4000_0000h。两个BAR中所有其他的可写bit都已被清零。

 

到这里为止，BAR Pair（BAR 1和BAR 2）的配置就已经完成了。一旦软件启用了命令寄存器（Command register，偏移地址04h）中的内存地址译码，那么这个设备就会接受所有地址在2_4000_0000h-2_4300_0000h（64MB大小）这个范围内的memory请求。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image156.jpg)

图 4‑5设置建立64bit可预取内存的BAR

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image158.jpg)

表 4‑2将两个BAR都写入为全1后的读取结果

#### 4.2.4     BAR示例3——IO地址空间请求

让我们在前面两个例子的基础上继续，我们假设当前的Function还需要请求IO空间，如图 4‑6所示。在图中，进行请求的BAR（本例中为BAR 3）在配置过程中分为3个节点：

\1.    在图 4‑6中的（1），我们可以看到BAR 3处于未初始化的状态。系统软件此前已经将每一个BAR都写入了全1（写入BAR中可写的bit，不可写的不受影响），并且已经对BAR 0、BAR 1 BAR 2组成的BAR Pair进行了评估。现在软件要继续看看设备是否还需要通过BAR 3来请求更多的地址空间。图 4‑6中的状态（2）展示了BAR 3被写入全1后的样子。

\2.    软件现在将会读取BAR 3的内容，以此来评估请求地址空间的大小和类型。表 4‑3中总结了本次配置读的读取结果。

\3.    读取BAR 3的内容之后，软件就知道了这个地址空间请求是要请求256byte大小的IO地址空间，那么软件的最后一步操作就是要为设备分配一个IO地址空间范围，并将这个地址范围的起始地址写入BAR 3。图 4‑6中的状态（3）所展示的BAR 3就是完成了写入起始地址的样子。在我们的例子中，这个设备的起始地址为16KB，所以bit 14是被置为1的，也就是说基地址为4000h，所有的高位bit都是清0的。

 

到目前为止，对BAR 3的配置已经完成了。一旦软件启用了命令寄存器（Command register，偏移地址04h）中的IO地址译码，那么这个设备就会接受并响应所有地址在4000h-40FFh（256Byte大小）这个范围内的IO事务。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image160.jpg)

图 4‑6 IO BAR的设置建立

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image162.jpg)

表 4‑3 IO BAR被写入全1后再读取的读取结果

#### 4.2.5     所有的BARs都必须按顺序被评估

在讲解了前面的三个示例之后，我们可以很明显的发现，软件必须按顺序来对BARs进行评估，即BAR 0—BAR 1—BAR 2—BAR 3这样的顺序。

大多数时候，Function并不需要启用全部的6个BAR。即使是在我们上面举的例子中，也仅仅是使用了6个BAR中的4个。如果上面例子的基础上并不需要继续请求更多的地址空间，那么设备的设计者需要将BAR 4和BAR 5的所有bit都强制固定为全0。这样的话，即使软件也对BAR 4和BAR 5用配置写来写入全1，BAR 4和BAR 5的值（全0）也不会受到配置写的影响。在对BAR 3进行完评估（evaluate）之后，软件需要继续去评估BAR 4。一旦软件检测到BAR 4中没有bit被置为1（因为BAR 4的所有bit都被强制固定为0），软件就知道这个BAR并没有被使用，然后继续去评估下一个BAR（也就是BAR 5）。

即使软件发现了一个BAR并没有被使用，所有的BAR也都必须被评估。在PCI或者PCIe中，并没有规定说一定要以BAR 0来作为第一个用来请求地址空间的BAR。如果设备的设计者需要的话，他们也可以选择比如说BAR 4来作为第一个请求地址空间的BAR，并将BAR 0、BAR 1、BAR 2、BAR 3、BAR 5的所有bit都强制固定为0。这也再一次说明了软件必须按照顺序对Header中的每一个BAR都进行评估。

#### 4.2.6     大小可变的BARs（Resizable BARs）

在PCIe协议的2.1版本中新加入的一个特性，支持改变BAR中所请求的地址空间的大小，这种特性是通过在扩展配置空间内定义一个新的能力结构（Capability Structure）来实现的。新的Capability Structure使得Function能够公布它可以操作的地址空间的大小，然后让软件根据实际可用的系统资源来选用Function所公布的地址空间大小中的某一种。例如，如果一个Function理想情况下要拥有2GB的可预取内存地址空间（Prefetchable Memory Address Space），但是它也可以只使用1GB、512MB或者256MB的P-MMIO进行操作，假如说系统软件无法容纳更大的、大于256MB的地址空间请求，那么系统软件应该就只能使Function请求256MB的地址空间。

### 4.3 Base寄存器和Limit寄存器（Base and Limit Registers）

#### 4.3.1     整体说明（General）

一旦一个Function的BAR们都被编程写入了，这个Function就知道自己所拥有的地址范围了，这意味着这个Function将会把任何目的地址在这一范围内的事务都声明为自己的，这个地址范围就是被写入它的某一个BAR的地址范围。这没什么问题，但是更重要的是要认识到，Function要“看到”它应该声明的事务的唯一办法是，它上方的Bridge需要将这些事务向下转发到这个目标Function所连接的相应的链路。因此，每个Bridge（例如Switch的端口和RC端口）都需要知道它下方所存在的可用地址范围，这样Bridge才能确定哪些请求需要从它的主接口（Primary interface，它在上方）被转发到它的次级接口（Secondary interface，它在下方）。如果一个请求的目标地址是这个Bridge下方Function的BAR中的地址，那么这个请求就应该被转发到Bridge的次级接口。

在Bridge的Type 1 Header中有Base寄存器和Limit寄存器，而Bridge下方的地址范围将会被编程写入这两种寄存器。在每个Type 1 Header中可以看到有3套Base、Limit寄存器。之所以需要三套是因为一个Bridge下的地址范围可以被分成三类：

n 可预取内存空间（P-MMIO）

n 不可预取内存空间（NP-MMIO）

n IO空间（IO）

为了解释这些Base和Limit寄存器是如何工作的，让我们在上一节的例子上继续进行讲解，并将上一节例子中那个已经完成BAR配置的EP放在一个Switch的下方，如图 4‑7所示。图中也列出了这个Function中各个BAR所拥有的地址范围。

对于每个下方存在EP的Bridge来说，它的Base和Limit寄存器都需要被编程写入，但在开始这个写入过程之前，我们先来关注一下连接到我们示例EP的Bridge，也就是Switch的Port B。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image164.jpg)

图 4‑7设置建立Base和Limit寄存器值的示例的拓扑

#### 4.3.2     P-MMIO可预取范围（Prefetchable Range）

Type 1 Header有两对可预取内存base/limit寄存器。被称为Prefetchable Memory Base/Limit的寄存器要储存可预取地址范围的低32bit地址信息。如果这个Bridge支持对64bit地址进行译码，那么另一个被称为Prefetchable Memory Base/Limit Upper 32bits寄存器就也需要被启用，要用它来储存地址范围的高32bit，也就是bit[63:32]。图 4‑8中展示了软件应该写入这些寄存器的值，以此来指示这个Bridge（Switch的Port B）下方的可预取地址范围为2_4000_0000h-2_43FF_FFFFh。这些寄存器的每个字段的含义都被总结在了表 4‑4中。

下面对表 4‑4中内容进行复述。

n Prefetchable Memory Base寄存器：

n 值为4001h。

n 用法为，寄存器的高12bit用来存放32bit BASE地址的高12bit，也就是[31:20]。Base地址中的低20bit全是0，这意味着Base地址总是在1MB边界上对齐。

寄存器的低4bit用于指示Bridge中是否支持64bit地址译码，这4bit值为1则意味着Upper Base/Limit寄存器要被启用。

n Prefetchable Memory Limit寄存器：

n 值为43F1h。

n 用法与Base寄存器相似，这个寄存器的高12bit用来存放32bit LIMIT地址的高12bit，也就是[31:20]。Limit地址的低20bit为全1（16进制下都是F）。这个寄存器的低4bit和base寄存器的低4bit含义相同。

n Prefetchable Memory Base Upper 32bits寄存器：

n 值为00000002h。

n 用法为，这个寄存器用来存放这个端口下方可预取内存的64bit BASE地址的高32bit。

n Prefetchable Memory Limit Upper 32bits寄存器：

n 值为00000002h。

n 用法为，这个寄存器用来存放这个端口下方可预取内存的64bit Limit地址的高32bit。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image166.jpg)

图 4‑8示例中Prefetchable Memory Base/Limit 寄存器的值

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image168.jpg)

表 4‑4示例中Prefetchable Memory Base/Limit 寄存器的含义

#### 4.3.3     NP-MMIO不可预取范围（Non-Prefetchable Range）

不可预取内存空间只支持32bit地址，这一点不同于可预取内存空间范围。因此对于不可预取内存空间来说，只有1个base寄存器和1个limit寄存器。按照图 4‑7的示例，Port B的Bridge中的Non-Prefetchable Memory Base/Limit寄存器应该被写入的值被展示在图 4‑9中。这些寄存器的每个字段的含义都被总结在了表 4‑5中。

下面对表 4‑5中内容进行复述。

n Non-Prefetchable Memory Base寄存器：

n 值为F900h。

n 用法为，寄存器的高12bit用来存放32bit BASE地址的高12bit，也就是[31:20]。Base地址中的低20bit全是0，这意味着Base地址总是在1MB边界上对齐。

寄存器的低4bit必须要为0。

n Non-Prefetchable Memory Limit寄存器：

n 值为F900h。

n 用法与Base寄存器相似，这个寄存器的高12bit用来存放32bit LIMIT地址的高12bit，也就是[31:20]。Limit地址的低20bit为全1（16进制下都是F）。这个寄存器的低4bit和base寄存器的低4bit含义相同。

寄存器的低4bit必须要为0。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image170.jpg)

图 4‑9示例中Non-Prefetchable Memory Base/Limit 寄存器的值

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image172.jpg)

表 4‑5示例中Non-Prefetchable Memory Base/Limit 寄存器的含义

这个例子展示了一个很有趣的情况，那就是实际上Port B下方的EP所拥有的NP-MMIO范围大小为4KB，但是Port B的配置空间却表示出了1MB的范围，这远大于4KB。这是因为Type 1 Header中的base/limit寄存器只能用来表示地址的[31:20]。低20bit也就是[19:0]，则会被隐去。因此内存空间base/limit寄存器所能标志的最小范围就是1MB。

在我们的例子中，EP请求并被授予了4KB的NP-MMIO（F900_0000h-F900_0FFFh）。而Port B中NP-MMIO的base/limit寄存器被写入的值则指示了这个Port下存在的NP-MMIO大小为1MB（F900_0000h-F90F_FFFFh），或者说是1024KB。这意味着有1020KB的内存地址空间（F900_1000h-F90F_FFFFh）被浪费了。即使这样，这些被浪费的地址空间也**一定不能**被分配给其他的EP，否则这会导致对数据包的路由机制无法工作。

#### 4.3.4     IO范围（IO Range）

类似于可预取内存范围，Type 1 Header中IO base/limit寄存器也有两对。被称为IO Base/Limit的寄存器用于存放IO地址范围的低16bit地址信息。若这个Bridge支持32bit IO地址的译码操作（现实设备中很少见），那么就要启用IO Base/Limit Upper 16Bits寄存器来存放地址信息的高16bit，也就是IO地址范围的[31:16]。按照我们的前面的示例，在图 4‑10中给出了软件要写入这些寄存器的值，以此来表示这个Bridge（Port B）之下存在的IO地址范围为4000h-4FFFh。这些寄存器的每个字段的含义都被总结在了表 4‑6中。

下面对表 4‑6中内容进行复述。

n IO Base寄存器：

n 值为40h。

n 用法为，寄存器的高4bit用来存放16bit BASE地址的高4bit，也就是[15:12]。Base地址中的低12bit全是0，这意味着Base地址总是在4KB边界上对齐。

寄存器的低4bit用于指示Bridge中是否支持32bit地址译码，这4bit值为1则意味着Upper Base/Limit寄存器要被启用。

n IO Limit寄存器：

n 值为40h。

n 用法与Base寄存器相似，这个寄存器的高4bit用来存放16bit LIMIT地址的高4bit，也就是[15:12]。Limit地址的低12bit为全1（16进制下都是F）。这个寄存器的低4bit和base寄存器的低4bit含义相同。

n IO Base Upper 32bits寄存器：

n 值为0000h。

n 用法为，这个寄存器用来存放这个端口下方IO地址空间的32bit BASE地址的高16bit。

n IO Limit Upper 32bits寄存器：

n 值为0000h。

n 用法为，这个寄存器用来存放这个端口下方IO地址空间的32bit Limit地址的高16bit。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image174.jpg)

图 4‑10示例中IO Memory Base/Limit 寄存器的值

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image176.jpg)

表 4‑6示例中IO Base/Limit 寄存器的含义

在这个例子中，我们又看到了一种情况，Bridge被编程写入的地址空间要远大于其下方的Function所拥有的的实际IO地址空间。例子中EP拥有的地址空间为256Byte（4000h-40FFh）。而Port B被编程写入的值却表示其下方存在的IO地址空间为4KB（4000h-4FFFh）。同前面讨论MMIO时一样，这也是Type 1 Header的一种局限。对于内存地址空间来说，低12bit（bit[11:0]）的值是被隐去的，因此能被指定的IO地址范围的最小值就是4KB。这种局限比内存范围中的1MB最小窗口（1MB minimum window）要更加严重。在基于x86（Intel兼容）的系统中，处理器仅支持16bit的IO地址空间，而由于IO地址信息仅有bit[15:12]是可以被指定在Bridge中，这就意味着一个系统中最多就只能有16个（24）不同的IO地址范围。

#### 4.3.5     未被使用的Base和Limit寄存器

并不是每个PCIe设备都会使用全部的3种地址空间（P-MMIO、NP-MMIO、IO）。实际上，PCIe协议中不鼓励使用IO地址空间，它仅仅是受一些传统遗留原因的支持，并有可能在未来新版本的协议中被弃用。

我们来假设这种情况，一个EP中并没有请求使用所有的3种地址空间，那么对于这样的设备上方的Bridge来说，它的base和limit寄存器应该被配成什么值呢？首先，我们肯定不能简单地就把它们写入全0，因为低位的被隐去的地址bit依然是不同的（base中低位bit认为全0，limit中低位bit认为全F），这依然会表示出一个有效的地址范围。所以为了解决这些情况，就必须令base寄存器中的写入值大于limit寄存器的写入值。例如，如果一个EP并没有请求IO地址空间，那么这个Function上方直接相连的Bridge将会把IO Limit寄存器写为00h，将IO Base寄存器写为F0h。由于Base地址大于Limit地址，Bridge会知道这是一个无效的设置，并以此来认定其下方没有Function拥有IO地址空间。（这里原书中有误，写成了将limit配置为大于base，这个错误在errata中已经列出）

这种使base和limit寄存器无效化配置的方法对于所有的3种base/limit寄存器都是可行的，而不只是针对IO base/limit寄存器。

### 4.4 完整性检查：对所有用于地址路由的寄存器（Sanity Check：Registers Used For Address Routing）

为了确保你已经理解了设置建立BARs和Base/Limit寄存器的规则和方法，请仔细查看图 4‑11，以保证你对它们的正确认知。我们已经简单的对示例系统进行了扩展，在Switch的Port A下方加入了另一个EP以及它所请求的地址空间。要记得，Type 1 Header中也含有2个BAR，它们也可以请求地址空间。而一个Bridge的Base/Limit寄存器表示的地址空间范围并不包括Bridge自身的BAR所请求的地址空间，也就是说Base/Limit寄存器只表示Bridge下方存在的地址空间。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image178.jpg)

图 4‑11最终的地址路由示例建立

### 4.5 TLP路由基础（TLP Routing Basics）

在前几个小节中所讲述的设置建立BARs和Base/Limit寄存器的目的，是为了确保流量（traffic）可以被正确的路由到它的目的Function，这样子这个目的Function就可以看见这些流量中的事务并声明对这些事务的所有权。在例如PCI的这种共享总线（Share-Bus）结构中，所有的流量对于总线上的每个设备都是可见的。只有当流量的目的地在另一条总线，也就是必须跨越一个Bridge时，才会对请求进行路由。由于PCIe链路是点对点的（point-to-point），那么在设备间发送事务时就需要更多的路由操作了。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image180.jpg)

图 4‑12多端口的PCIe设备有进行路由的职责

如图 4‑12所展现的这样，一个PCIe拓扑使用独立的、点对点的链路来将各个设备与它们的一个或多个邻设备（neighbors）相连接。当流量到达链路接口的入站端（inbound side，或称为ingress port入口端口），这个端口将会进行错误校验，然后做出如下的三个决定中的一个：

\1.    接收这个流量并在自身内部使用它。

\2.    将这个流量转发至相应的出站（outbound，或者egress出口）端口。

\3.    拒绝这个流量，因为入口端口自身既不是这个流量的预期的目标，也不是一个用来对它进行转发的接口。（注意，还存在其他可能的原因导致流量被拒绝）

#### 4.5.1     接收器检查3种类型的流量（Recievers Check For Three Types of Traffic）

假设一条链路处于完全可工作状态，那么每个设备的接收接口（入口端口ingress port）必须对到达的三种链路流量进行检测和评估：Ordered Sets（命令集）、DLLPs（Data Link Layer Packets数据链路层包）以及TLPs（Transaction Layer Packets事务层包）。Ordered Sets和DLLPs对于链路来说是本地的，因此它们不用被路由到另一条链路。TLP可以并且也有可能需要被从一条链路搬移到另一条链路，这其中所基于的路由信息存在于数据包的Header中。

#### 4.5.2     路由元件（Routing Elements）

对于像RC和Switch这样具有多个端口的设备来说，它们可以在端口之间转发TLPs，它们有时也被称作Routing Agent（路由媒介）或是Routing Element（路由元件）。它们会接受目标为自身内部资源的TLP，也会将TLP在自身入口端口和出口端口之间进行转发。

有趣的是，Switch需要支持Peer-to-Peer的路由，但是对于RC来说这项功能是可选的。Peer-to-Peer流量通常是一个EP向另一个EP发送数据包。

EP只有一条链路，并且它永远不希望看到一个目的地不是EP自己的流量，对于它来说只会简单地接受或者拒绝输入的TLPs，而不会进行转发。

#### 4.5.3     TLP路由的三种方法（Three Methods of TLP Routing）

##### 4.5.3.1   整体说明（General）

TLP可以被基于地址路由（Memory或IO），可以被基于ID路由（Bus—Device—Function），或者还可以被隐式路由（routed implicitly）。具体使用的路由办法要根据TLP类型来决定。表 4‑7总结了TLP类型和路由方法的对应关系。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image182.jpg)

表 4‑7 PCIe TLP类型对应的路由方法

可以发现，Message是唯一一种支持多种路由方法的TLP类型。PCIe协议中定义的大多数message使用的是隐式路由，然而由厂商定义的message则根据需要也可以使用地址路由或者ID路由。

##### 4.5.3.2   隐式路由和Message的目的（Purpose of Implicit Routing and Message）

在隐式路由中，不使用地址或者ID这种路由信息；相反，这个数据包是根据包头中的一个代码（code）来进行路由的，这个代码指示的目的地是拓扑结构中一个已知的位置，例如RC。这种方法简化了使用隐式路由的message路由。

###### l 为什么是Message？（Why Messages？）

Message事务在PCI和PCI-X中并没有被定义，但是在PCIe中被引入。加入Message作为一种数据包类型的主要的原因是，为了继续进行PCIe的设计目标之一，也就是大幅度地减少以前PCI中的边带信号（sideband signals）的使用，例如中断pins、错误pins、电源管理信号等等。因此，大多数的边带信号都被带内（in-band）数据包所替代，这些数据包的形式就是Message TLP。

###### l 隐式路由是怎么提供帮助的？（How Implicit Routing Helps？）

要使用带内Message来替代边带信号，就需要一种方法来将这些Message路由到正确的收件方（recipient）那里去，而Message的收件方位于一个由数量众多的点到点链路组成的拓扑结构中。隐式路由利用了这样一个事实：Switch和其他的路由元件是可以理解上行（upstream）和下行（downstream）的概念的，且RC是位于整个拓扑结构的最顶端，而EP则是位于拓扑结构的底部。因此举例来说，一个Message可以使用一个简单的代码来表示它应该被送往RC，或者是要被送往下行的所有设备。上述的这种能力能够有效的减少对定义地址范围或是ID列表的需要，而原本是需要针对不同的Message来定义特定的地址范围或是ID列表才能完成路由。

 

不同类型的隐式路由的相关内容可以参阅“Implicit Routing”一节。

#### 4.5.4     拆分事务协议（Split Transaction Protocol）

像大多数其他串行传输技术一样，PCIe使用拆分事务协议（Split Transaction Protocol），它允许一个目标设备接收一个或多个请求，然后再使用单独的完成包来对这些请求进行响应。相比于在PCI中使用等待态（wait-state）或是延迟传输（retry）来处理访问目标时出现的延迟，拆分事务协议是一种显著的进步。拆分事务协议不再使用通过试探目标是否处于ready才进行访问的高延迟传输机制，而是使用目标设备自己发起响应的方式，无论何时只要目标设备处于ready，它都能自己发起响应，而这个响应所对应的请求是此前就已经给到目标设备的了。这种方式使得每个事务的完成都至少需要两个单独的TLP——Request（请求包）和Completion（完成包），而我们后面将会讲到，对于一个单独的读请求，可能会有多个完成包返回。图 4‑13展示了一个拆分事务中的Request-Completion的组成，这个例子的场景是软件从EP读取数据。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image184.jpg)

图 4‑13 PCIe事务中的请求TLP和完成TLP

#### 4.5.5     Posted与Non-Posted

为了减少Request-Completion延迟所带来的开销和损失，MWr（Memory Write）事务是Posted的，这意味着从Requester的角度来说，当这个Posted事务刚从Requester发出去，就认为它已经完成了。如果想帮助理解，可以将术语“Posting（邮寄）”和邮政系统（postal system）联系到一起，因为“邮寄”一个MWr和邮寄一封信十分相似。一旦你将要邮寄的信放入邮局邮箱，你就会相信邮政系统会把它送出去，而不必去等待投递的确认。这种方法比等待整个Request-Completion传输完成要快的多。但是，在所有的邮寄方案中都会存在不确定性，事务是否、何时在最终的接收方被成功完成，这是不确定的。

在PCIe中，尽管所有的Posted MWr会带来不确定性，但是相比于这种方法获得的收益来说，这种不确定性是比较少量且可以接受的。相比之下，对IO和配置空间的写操作几乎都会影响设备的行为，且这种影响还是实时与这两种写操作相关联的。因此，对于这样的写请求来说，非常有必要知道它们是否完成以及什么时候完成。基于这样的原因，IO写和配置写必须要是Non-Posted的，它们总是需要收到一个返回的完成包来报告自身操作的状态。

总结来说，Non-Posted事务需要完成包，Posted事务不需要完成包而且也永远不应该收到一个完成包。表 4‑8列出了哪些PCIe事务是Posted的，哪些是Non-Posted的。

下面对表 4‑8的内容进行复述：

n Memory Write：

**所有的Memory Write****请求都是Posted****的**。不需要返回完成包（Completion）。

n Memory Read和Memory Read Lock：

**所有的Memory Read****请求都是Non-Posted****的**。需要由Completer返回一个带有数据的Completion（这个Completion由一个或多个TLP组成），Completer通过Completion来传递Requester所请求的数据以及当前Memory Read事务的状态。若发生了错误，那么返回的用于报告错误状态的Completion将不含有数据。

n AtomicOP：

**所有的AtomicOP****请求都是Non-Posted****的**。需要由Completer返回一个带有数据的Completion，这个Completion中包含了目标位置的原始值。

n IO Read和IO Write：

**所有的IO****请求都是Non-Posted****的**。对于IO Write和失败的Read来说，需要返回一个不含有数据的Completion。而对于成功的Read来说，则需要返回一个带有数据的Completion。

n Configuration Read和Configuration Write：

**所有的Configuration****请求都是Non-Posted****的**。与IO请求类似，对于Configuration Write和失败的Read来说，需要返回一个不含有数据的Completion。而对于成功的Read来说，则需要返回一个带有数据的Completion。

n Message：

**所有的Message****请求都是Posted****的**。虽然路由方法是取决于Message类型的，但是Message都被认为是posted请求。

 

简单总结来说，只有Memory Write和Message是 Posted的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image186.jpg)

表 4‑8 Posted和Non-Posted事务

#### 4.5.6     Header中定义数据包格式和类型的字段

##### 4.5.6.1   整体说明（General）

如图 4‑14所示，每个TLP都含有一个3个或4个DW（12或16byte）的Header。在TLP Header中含有Format和Type这两个字段，它们用来定义剩余的Header的内容，并决定出这个TLP在拓扑结构中游走时使用的路由方法。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image188.jpg)

图 4‑14 TLP通用的3DW和4DW Header

 

##### 4.5.6.2   Header中Format/Type字段的编码含义

下方的表 4‑9中总结了TLP Header中的Format和Type字段的编码含义。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image190.jpg)

表 4‑9 TLP Header中Format和Type字段编码含义

#### 4.5.7     TLP Header概述

当TLP在入口端口（ingress port）被接收到，首先要在物理层和数据链路层对TLP进行错误检查。如果检查无错，那么再由事务层来检查当前TLP使用的是哪种路由方法。基础的步骤为：

\1.    Format和Type字段确定了TLP Header的大小、数据包的格式和类型。

\2.    根据TLP所使用的路由方法（与Type相关），设备将确定这个TLP的预期接收者是否就是自己。如果确实就是自己，那么设备将接受（或者说是消耗）这个TLP，否则设备将会把这个TLP转发到相应的出口端口（egress port）——根据该出口的排序和流控规则。

\3.    如果设备既不是TLP的预期接收方，且设备也并不位于前往预期接收方的路径上，那么它通常会通过一个状态字段为Unsupported Request（UR）的完成包来拒绝这个TLP。

### 4.6 路由机制的应用方法（Applying Routing Mechanisms）

一旦配置完了系统地址并且也允许开始执行事务，设备将会检查输入的TLP并使用相应的配置字段域来对数据包进行路由。下面的小节将会对PCIe结构中的TLP路由机制基础特点和功能进行描述。

#### 4.6.1     ID路由（ID Routing）

ID路由是用来指向一个逻辑位置——Bus Num，Device Num，Function Num，或者一般称为BDF，这个逻辑位置一般是拓扑结构中的一个Function。ID路由这种机制对PCI和PCI-X协议中用于配置事务路由的方法是兼容的。在PCIe中，ID路由仍然会被用来路由配置包，同时它还可以用来路由完成包和一些Message。

##### 4.6.1.1   Bus号，Device号，Function号的使用上限

PCIe支持的拓扑结构的上限与PCI和PCI-X一样：

\1.    一个系统中可以存在的最大总线数量为256，因为Bus号是8bit位宽的。这个总线数量包括了Switch产生的内部总线。

\2.    每条总线上最多可以有32个设备，因为Device号是5bit位宽的。一个老式的PCI总线或是Switch、RC的内部总线的下方可以挂载不止一个设备，然而外部的PCIe链路（external PCIe Links）就必须要是点到点的（point-to-point），也就是说外部PCIe链路下方只能挂载一个设备。下行端口（Switch或RC的下行端口）会强制将外部链路的Device号定为Device 0，因此每个外部EP都总会是Device 0。除非使用了替代路由ID解释（Alternative Routing-ID Interpretation，ARI），若使用了ARI那么这个外部Device将没有Device号，关于ARI的更多内容请参阅“IDO（ID-based Ordering）”一节。

\3.    每个设备中最多可以有8个内部Function，因为Function号是3bit位宽的。

##### 4.6.1.2   TLP Header中有关ID路由的关键字段

如果一个收到的TLP中的Type字段表示了这个TLP要使用ID路由，那么TLP Header中的ID字段（Bus，Device，Function）就要用来执行路由检查（routing check）。有两种情况：使用3DW Header来进行ID路由，使用4DW Header来进行ID路由（这只可能出现在Message路由中）。图 4‑15展示的是一个用于ID路由的3DW Header，图 4‑16展示的是一个用于ID路由的4DW Header。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image192.jpg)

图 4‑15 3DW TLP Header中的ID路由字段

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image194.jpg)

图 4‑16 4DW TLP Header中的ID路由字段

##### 4.6.1.3   EP进行一次检查（Endpoint：One Check）

对于ID路由，一个EP只会简单地根据自己的BDF来检查TLP Header中的ID字段。每当Function在自己的链路上接收到一个Type 0配置写时，Function将会从配置写事务的TLP Header的byte8-9中“捕获Capture”到自己的Bus号和Device号。Function必须将Bus号和Device号存储下来，而具体是存在什么地方在协议中并没有特别指定。保存下来的Bus和Device号被可以被用作Requester ID，例如当这个EP发起请求后，请求的Completer可以将这个Requester ID放进完成包内来让完成包被顺利路由返回，Requester ID就是这个完成包用来路由的信息。

##### 4.6.1.4   Switches（Bridges）每个端口进行两次检查

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image196.jpg)

图 4‑17一个使用ID路由的入站TLP在进行Switch的路由检查

对于一个使用ID路由的TLP来说，一个Switch端口首先要检查这个TLP的预期目的地是不是端口自身，这种检查是通过将端口自身的BDF与TLP Header中的目标ID进行比较来进行的，如图 4‑17中的（1）所示。与EP一样，每个Switch端口将会从上行端口发现的每次配置写（Type 0）事务中捕获到自己的Bus号和Device号。如果TLP中的目标ID与Switch端口的ID相同，那么Switch端口就会消耗掉（consume）这个TLP，也就是不再转发到其他地方而是自己使用。如果这两个ID字段并不相同，那么Switch端口将会检查这个TLP的目标位置是否在自己的下方。这个检查的方式是，通过检查次级总线号寄存器（Secondary Bus Number Register）和从属总线号（Subordinate Bus Number Register）来查看TLP的目标总线号是否在Switch端口下方的从属总线范围内（包括范围边界）。如果在范围内，那么就将这个TLP向下转发。图 4‑17中的（2）所表示的就是这项检查。如果数据包到达Switch的上行端口，但是没有与端口的BDF相匹配，且也不在从属总线范围内，那么它就会被作为上行端口的UR（Unsupported Request）来处理。

如果上行端口确定了所接收到的TLP的目的地是其下方的一个设备（也就是这个TLP的目标总线在上行端口的从属总线范围内），那么上行端口就会将TLP向下转发，Switch所有的下行端口都会对这个TLP做跟上面一样的检查。每个下行端口都会检查自己是不是TLP的目的地，如果是，那么这个端口将会消耗掉这个TLP而其他端口将会忽略这个TLP。如果TLP的目的地并不是任何一个下行端口，那么下行端口们都会再去检查TLP的目标设备是否在自己下方的从属总线范围中。如果有一个下行端口检查到了这个TLP的目标设备在自己的下方，那么这个端口就会将TLP转发至自己的次级总线，且其他下行端口将会忽略这个TLP。

在这一节中，很重要的一点是要记住Switch中的每一个端口（Port）都是一个Bridge，因此它们都有自己的配置空间，并且它们的配置空间Header必然是Type 1 Header。尽管在中只展示除了上行端口的Type 1 Header，但是实际上每个端口（每个P2P Bridge）都有自己的Type 1 Header，并且在每个端口收到TLP时都会执行上面所述的两次检查。

#### 4.6.2     地址路由（Address Routing）

使用地址路由的TLP引用的内存（系统内存和内存映射IO）和IO地址映射和PCI/PCI-X中事务引用的是一样的。如果一个Memory请求的目标地址小于4GB（即一个32bit地址），那么这个TLP必须使用3DW Header。而如果它的目标地址大于4GB（即一个64bit地址），那么这个TLP就必须使用4DW Header。IO请求的地址被限制为32bit，并且仅当为了支持传统的旧功能时才能使用核实现IO请求。

##### 4.6.2.1   TLP Header中有关地址路由的关键字段

当TLP Header中的Type字段表示这个TLP使用地址路由时，那么就要用Header中的Address字段来执行路由检查。Address字段可以是32bit也可以是64bit。

###### l 使用32bit地址的TLP

对于IO和32bit的Memory请求，它们所使用的3DW Header如图 4‑18所示。因此，这些TLP的目标内存映射寄存器（Memory-Mapped Registers）会存在于4GB内存边界以下。

###### l 使用64bit地址的TLP

对于64bit的Memory请求，它们所使用的4DW Header如图 4‑19所示。因此，这些TLP的目标内存映射寄存器（Memory-Mapped Registers）会存在于4GB内存边界之上。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image198.jpg)

图 4‑18 3DW TLP Header中的地址路由相关字段

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image200.jpg)

图 4‑19 4DW TLP Header中的地址路由相关字段

##### 4.6.2.2   EP对地址的检查

如果一个EP收到了一个地址路由的TLP，那么EP将会用其自身配置Header中的每一个BAR（Base Address Register基地址寄存器）与TLP Header中的Address字段进行比对，如图 4‑20所示。由于EP只有一个链路接口，因此它只会接受或者拒绝数据包，而不会对其进行转发。如果TLP Header中的Address字段与EP的某一个BAR所指示的地址范围相匹配，那么EP就会接受这个TLP。更多的关于BARs是怎么样被使用的内容请参阅“基地址寄存器BARs（Base Address Registers）”一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image202.jpg)

图 4‑20 EP对输入TLP的地址进行检查

##### 4.6.2.3   Switch进行路由（Switch Routing）

对于Switch端口来说，如果一个输入的TLP使用的是地址路由，那么端口会首先检查这个TLP的目标地址是否就是端口本身，这种检查是通过将Switch端口自身Type 1 Header中的两个BAR与TLP Header中的Address进行比对来实现的，如图 4‑21中的步骤1所示。如果TLP Header中的Address字段与某个BAR的地址范围相匹配，那么就说明这个Switch端口就是TLP的目的，端口将会消耗掉这个TLP。如果并未匹配任何一个BAR的地址范围，那么Switch端口将会检查Base/Limit寄存器对，以此来确定这个TLP的目标Function是否在本端口之下。如果Request的目标是IO空间，那么端口将会检查IO Base寄存器和IO Limit寄存器，如图 4‑21中步骤2a所示。而如果Request的目标是Memory空间，那么端口将会检查不可预取Base/Limit寄存器和可预取Base/Limit寄存器，如图 4‑21中步骤2b所示。更多关于Base/Limit寄存器对是如何被评估的信息，请参阅“Base寄存器和Limit寄存器（Base and Limit Registers）”一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image204.jpg)

图 4‑21 Switch检查使用地址路由的入站TLP的路由信息

为了能理解Switch中基于地址的TLP路由，最好要记住一点，每个Switch端口自身都是一个Bridge。下面的步骤描述的是一个Bridge（也就是Switch端口）接收一个基于地址路由的TLP：

###### l TLP向下行移动（由Switch主接口接收并向下转发）

\1.    如果TLP的目标地址与Switch端口的其中一个BAR地址范围相匹配，那么这个Bridge（Switch端口）就会消耗掉这个TLP，因为其自身就是这个TLP的目的地。

\2.    如果TLP的目标地址落在端口的某种Base/Limit寄存器所表示的范围内，那么这个TLP就会被转发到次级接口（Secondary Interface），也就是向下转发。

\3.    若以上两个条件都不符，且主接口（Primary Interface）上没有其他的Bridge声明拥有这个TLP，那么这个TLP将被Switch主接口当做UR（Unsupported Request）来处理。

###### l TLP向上行移动（由次级接口接收并向上转发）

\1.    如果TLP的目标地址与当前Switch次级接口（Secondary Interface）的其中一个BAR地址范围相匹配，那么这个Bridge（Switch端口）就会消耗掉这个TLP，因为其自身就是这个TLP的目的地。

\2.    如果TLP的目标地址落在端口的某种Base/Limit寄存器所表示的范围内，那么这个TLP将被Switch次级接口当做UR（Unsupported Request）来处理（除非这个端口是Switch的上行端口，在这种情况下，这个数据包可能是一个Peer-to-Peer的事务，它将会被转发至下行的另一个端口上，而不是之前接收这个TLP的端口）。

\3.    若以上两个条件都不符，那么这个TLP将会被转发至主接口（上行端口），因为这个TLP目的地既不是当前的端口Bridge，也不是这个Bridge之下的任何一个Function。

##### 4.6.2.4   多播能力（Multicast Capabilities）

在PCIe 2.1版本中，新加入了通过指定一个地址范围来进行多播的能力。任何接收到的TLP，只要其地址落在被指定为多播范围的地址范围之内，它的路由或者接受（routed/accepted）就要根据多播规则来处理。虽然这个多播的地址范围不一定会被保留在一个Function的BARs中，也不一定会存在于Bridge的Base/Limit寄存器对中，但是它也需要通过合适的方式被接受或转发。更多的关于多播功能的信息请参阅“Multicast Capability Register”一节。

#### 4.6.3     隐式路由（Implicit Routing）

隐式路由通常被一些Message数据包所使用，它是基于路由元件的感知能力，即路由元件知道拓扑结构具有上行和下行方向，并且拓扑的最顶部是RC。这使得一些简单的路由方法可以不需要去分配目标地址或是ID就能进行路由。由于RC一般都集成了电源管理（power management）、中断（interrupt）和错误处理逻辑（error handling logic），这使得RC基本上是大多数PCIe Message的发送者或者接收者。

##### 4.6.3.1   仅为Message所使用（Only for Messages）

有些Messages使用地址或者ID路由，而不是隐式路由，那么对于这些Message来说，它们的路由机制就跟前面几节描述的ID路由和地址路由相同。然而，对于绝大多数Message来说，使用的路由机制是隐式路由。隐式路由的目的是为了模仿边带信号（side-band signal）的行为，之所以要这样是因为PCIe的设计目标之一就是要尽可能的把原本PCI中的边带信号都清除干净。PCI中的这些边带信号一般是用于Host将一个事件通知给所有的设备，或者是设备将一个事件通知给Host。在PCIe中，我们使用Message TLPs来传输这些事件，而不是通过边带信号。在PCIe中被定义了Message的事件类型有：

n 电源管理（Power Management）

n INTx传统中断信号（INTx legacy interrupt signaling）

n 错误信号（Error signaling）

n 锁定事务的支持（Locked Transaction support）

n 热插拔信号（Hot Plug signaling）

n 供应商指定信号（Vendor-specific signaling）

n 插入卡插槽功耗限制设置（Slot Power Limit settings）

##### 4.6.3.2   TLP Header中有关隐式路由的关键字段

对于隐式路由来说，使用TLP Header中的子字段（sub-field）来确定Message的目的地。图 4‑22展示了一个使用隐式路由的Message TLP Header。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image206.jpg)

图 4‑22 4DW Message TLP Header中的隐式路由相关的字段

##### 4.6.3.3   Message的Type字段总结

表 4‑10展示了一个Message的TLP Header中Type字段的含义。如表中所示，最高的2bit用于指示这个数据包是一个Message，剩下的低3bit用于指示这个Message使用的路由方法。需要注意，Message TLP不管使用哪种路由操作，都只会使用4DW Header。若使用地址路由，那么就用byte8-15来组成64bit地址；若使用ID路由，那么就用byte8和9来组成目标BDF。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image208.jpg)

表 4‑10 Message请求Header中Type字段的用法

##### 4.6.3.4   EP的处理

对于隐式路由，EP会简单的检查路由子字段（sub-field）是否适合它。例如，EP会接受一个广播Message或是一个终止于EP接收端的Message，但是不会接受隐式目的地为RC的Message。

##### 4.6.3.5   Switch的处理

像Switch这样的路由元件（Routing Element）需要考虑TLP到达了哪个端口，以及TLP的路由子字段是否对这个端口来说是合适的。举例来说：

\1.    一个Switch的上行端口可以合法的接收一个广播Message。上行端口将会把这个广播Message复制一份，并向它的所有下行端口都转发。如果一个Switch的下行端口接收了一个隐式路由的广播Message（这意味着这个Message将会向上行移动），这是一种错误，它将被当做畸形TLP（Malformed TLP）来处理。

\2.    一个Switch的下行端口可以接收隐式路由目的地为RC的Message，下行端口将会把这个Message转发给上行端口，因为路由元件知道RC永远在上行。而Switch的上行端口并不会从外界接受一个隐式路由目的地为RC的Message，因为这意味着它会向下转发，但是RC却在拓扑结构的上方。

\3.    如果隐式路由Message表示它会终止在及接收者的位置，那么接收它的Switch端口将会消耗掉这个Message，而不是将其转发。

\4.    对于使用地址路由或者ID路由的Message来说，Switch会按照一般对地址或ID的检查方式来决定是接收它们还是转发它们。

### 4.7 DLLP和Ordered Set不会被路由

DLLP和Ordered Set的流量并不会被从Switch或者RC的入口端口路由到出口端口。这些数据包只会通过物理层到物理层的链路进行传输，从一个端口移动到另一个端口。

DLLP起源于PCIe端口的数据链路层，先经过物理层，接着离开端口并在链路上传输，然后到达对端端口。在对端端口上，DLLP先经过物理层，然后最终到达数据链路层，DLLP在这里被处理和消耗。DLLP不会继续沿着端口向上到达事务层，因此它并不存在路由。

相似的，Ordered-Set数据包起源于物理层，然后离开端口并通过链路传输，最终到达对端端口。在对端端口上，Ordered-Set在物理层就被处理和消耗。由于Ordered-Set也不会继续沿着端口向上到达数据链路层或者事务层，因此它也不存在路由。

正如本章所讨论的那样，只有TLP会被Switch和RC进行路由，它们起源于源端口的事务层，结束于目的端口的事务层。

 



#           第二部分    Part Two：Transaction Layer事务层

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



## Chapter 6      Flow Control //流量控制

### 关于上一章

上一章节讨论了主要的三种类型的数据包：TLP事务层包（Transaction Layer Packets）、DLLP数据链路层包（Data Link Layer Packets）、Ordered Sets命令集。并且在上一章中主要讲述TLP的用法、格式和不同种类的定义，并将详细的讲解TLP的相关字段含义。关于DLLP的内容将会在Chapter 9 “DLLP Element”中进行单独讲解。

### 关于本章

本章节将讨论流量控制协议（Flow Control）的目的以及细节操作。流量控制是用来确保在接收者无法接收TLP时，发送方不会再继续发送TLP。这避免了接收Buffer溢出，也消除了原本PCI工作方式中的一些低效行为，比如断开（disconnect）、重试（retry）和等待态（wait-state）。

### 关于下一章

下一章节将会讨论用于支持QoS（Quality of Service）的机制，并描述对网络结构中传输的不同数据包的传输时间和带宽进行控制的意义。这些机制包括，特定应用的软件将会给每个数据包都分配优先级，以及每个设备内构建可选的硬件来启用事务优先级管理。

### 6.1 流量控制概念（Flow Control Concept）

每条PCIe链路两端的端口都必须实现流量控制机制。在一个数据包允许被发出之前，流量控制机制必须确认接收端口拥有足够的可用Buffer空间来接收数据包。而在像PCI这种并行总线中，尽管不知道目标方是否准备好了处理数据，事务也会尝试去发送执行，如果因为没有足够的可用Buffer空间，请求被拒绝了，那么事务就会一直重试（Retry）直至完成。这就是PCI的“Delayed Transaction Model延迟的事务模型”，然而这种方式的效率很低。

流量控制机制可以改善当多个虚拟通道（Virtual Channel，VC）被占用时的传输效率。每个虚拟通道自己的事务是与其他VC的事务流量相独立的，因为流量控制Buffer是VC各自单独维护的。因此，若一个VC的流控Buffer满了，它也不会阻塞对其他VC Buffer的访问。PCIe支持最多8条虚拟通道。

流量控制机制使用一种基于信用（Credit-based）的机制，使得发送端口可以知道接收端口有多少可用的Buffer空间。作为它们初始化的一部分，每个接收者都要将自己的Buffer大小报告给链路对端的发送者，并在运行过程中使用DLLP来定期地更新这个“信用值”。技术上来说，明显地，DLLP是一种开销，因为它并不携带任何数据荷载，但是DLLP始终很小（始终为8个符号大小），这样可以使其对性能的影响最小。

流量控制逻辑实际上是两个层级的共同职责：事务层包含了计数器用于对可用Buffer空间进行计数，但是数据链路层也要负责发送和接收这些包含了可用Buffer空间信息的DLLP。如图 6‑1展示了这种共同职责。在流量控制的的工作过程中：

n **设备报告可用的Buffer****空间**：接收者的每个端口都要报告自己的流量控制Buffer的大小，单位为credit（信用）。一个Buffer中credit的数量会从自身接收侧事务层发送至发送侧数据链路层（如图 6‑1）。在合适的时间，数据链路层将会产生一个流量控制DLLP来将这个credit信息转发至每个流量控制Buffer的链路对端的接收者的。

n **接收者寄存Credit**：接收者收到流量控制DLLP，并将credit值传输至自身发送侧事务层。这就完成了credit从链路的一端到对端的传输。这些操作是双向进行的，直到所有的流量控制信息都完成了交换。

n **发送者检查Credit**：在发送者可以发送一个TLP之前，它需要检查流量控制计数器（Flow Control Counter），以此来知道对方是否有足够的credit。如果credit数量足够，那么就将TLP转发至数据链路层，但是如果credit不足，那么TLP的发送操作就会被阻塞直至获知的更多的足够的流量控制credit。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image258.jpg)

图 6‑1流量控制逻辑所处的位置

### 6.2 流量控制Buffer和Credit（Flow Control Buffer and Credits）

一个端口所支持的每个VC（Virtual Channel）资源都会实现流量控制Buffer。回想一下，链路两端的端口可能支持的VC数量是不同的，因此由软件进行配置和启用的VC的最大数量就是两个端口间的最大公共VC数量。

#### 6.2.1     VC流量控制Buffer的组成（VC Flow Control Buffer Organization）

接收者的每个VC流控buffer是按照通过VC的每种事务类别来进行管理的。这些类别为：

n Posted事务——MWr（Memory Write）和Message

n Non-Posted事务——MRd（Memory Read）、配置读/写、IO读/写

n 完成包——读和写完成包

除此之外，每个既有Header也有数据荷载的事务类别，都会被分成Header和数据两部分。这将产出6种不同的Buffer，每个Buffer都实现自己的流控。（见图 6‑2）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image260.jpg)

图 6‑2流控Buffer组成

对于有些事务，例如读请求，它只由Header组成，而像写请求就既有Header也有数据荷载。发送者必须确认Header的Buffer空间和数据荷载的Buffer空间都满足了事务传输的需求，才能发送事务。注意，当事务被转发至软件或者是在Switch中被转发至出口端口时，必须在VC流控Buffer中保持事务顺序。因此，接收者也biubiu跟踪Buffer内的Header和数据荷载的顺序。

#### 6.2.2     流控Credit（Flow Control Credits）

接收者需要报告Buffer空间的大小，使用的单位是Credit，或者叫流量控制Credit。Header和数据荷载Buffer的一个流控Credit（FCC，Flow Control Credit）的大小值分别为：

n Header Credit：Header大小的最大值+digest

—  完成包为4DW

—  请求包为5DW

n 数据Credit：4DW（16byte对齐）

流量控制DLLP会传达这个信息，而DLLP自身的传输是不需要流控Credit的。这是因为DLLP产生和终结的地方都是在数据链路层，并不需要使用到事务层Buffer。

### 6.3 初始流量控制通告（Initial Flow Control Advertisement）

在流控的初始化过程中，PCIe设备们会通过对Buffer空间的流控Credit进行“advertising通告”的方式，来相互传达各自的Buffer大小。PCIe也专门定义了一些Buffer需要的流控Credit初始值。一个通告过自己的初始Buffer空间的接受者可以有效的保证它的Buffer空间永远不会溢出。

#### 6.3.1     流控通告的最大值和最小值（Minimum and Maximum Flow Control Advertisement）

协议规范定义了各种不同的流控Buffer类型所报告的Credit数量的最小值，表 6‑1列出了这些Buffer类型对应的Credit最小值。然而，设备一般通告的Credit数量会远大于最小值，表 6‑2列出了协议所允许的Credit数量通告的最大值。

| Credit类型                                                | 通告（Advertisement）最小值                                  |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| Posted  Request Header  （PH）  Posted请求Header          | 1单位：  Credit值= 1个4DW Header+Digest= 5DW                 |
| Posted  Request Data  （PD）  Posted请求数据              | 这个值是Max_Payload_Size的最大值用Credit的表示。  例如：如果支持的最大的Max_Payload_Size为1024Byte，那么允许的最小的初始Credit值就是040h（64*16byte）。 |
| Non-Posted  Request Header  （NPH）  Non-Posted请求Header | 1单位：  Credit值= 1个4DW Header+Digest= 5DW                 |
| Non-Posted  Request Data  （NPD）  Non-Posted请求数据     | 1单位：  Credit值= 1个4DW Header+Digest= 5DW  2单位：  如果接收方支持AtomicOP路由或者具有作为AtomicOP完成者的能力，那么Credit值可以为02h。 |
| Completion  Header  （CplH）  完成包Header                | 1单位：  Credit值= 1个3DW Header+Digest= 4DW。这种情况是对于支持Peer-to-Peer的RC和Switch。  无限数量：初始Credit值=全0。应用于不支持Peer-to-Peer的RC和EP。 |
| Completion  Data  （CplD）  完成包数据                    | n单位：  这个值是Max_Payload_Size或者是读请求size这二者可能的最大值（一般读请求size会比前者小）除以FC单位大小（4DW）。这种情况是对于支持Peer-to-Peer的RC和Switch。  无限数量：初始Credit值=全0。应用于不支持Peer-to-Peer的RC和EP。 |

表 6‑1流控通告最小值

| Credit类型                                                | 通告（Advertisement）最小值                                  |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| Posted  Request Header  （PH）  Posted请求Header          | 128单位：  128 Credits=128*5DW=2560 Bytes                    |
| Posted  Request Data  （PD）  Posted请求数据              | 2048单位：  值为设备支持多有Function（8个）的Max_Payload_Size（4096 bytes）总和,总和为8*4096=32768,除以Credit大小（4DW）。  2048 Credits=2048*4DW=32768Bytes |
| Non-Posted  Request Header  （NPH）  Non-Posted请求Header | 128单位：  128 Credits=128*5DW=2560 Bytes                    |
| Non-Posted  Request Data  （NPD）  Non-Posted请求数据     | 作者无法给Non-Posted数据找到一个准确的Credit数量最大值。给出的单纯数据的Credit数量最大值为2048。然而，一种更合理的计算的方法是使用Non-Posted Header来限制在128 Credits内，因为Non-Posted数据总是与Non-Posted Header相关联。 |
| Completion  Header  （CplH）  完成包Header                | 128单位：  128 Credits=128*4DW=2048 Bytes。这是对不发起事务的端口的限制（例如支持Peer-to-Peer的RC和Switch）。  无限数量：初始Credit值=全0。应用于发起事务的端口（例如不支持Peer-to-Peer的RC和EP）。 |
| Completion  Data  （CplD）  完成包数据                    | 2048单位：  值为设备支持多有Function（8个）的Max_Payload_Size（4096 bytes）总和,总和为8*4096=32768,除以Credit大小（4DW）。  2048 Credits=2048*4DW=32768Bytes  无限数量：初始Credit值=全0。应用于发起事务的端口（例如不支持Peer-to-Peer的RC和EP）。 |

表 6‑2流控通告最大值

#### 6.3.2     无限Credit（Infinite Credits）

可以注意到，如果流控Credit值为00h，那么其意思就是拥有无限多的Credit。在流控初始化之后，不会再进行任何的通告（advertisement）。发起事务的设备必须要保留足够的buffer空间，这些buffer空间用来存放执行拆分事务（split transaction）的过程中返回的数据以及状态信息。这些事务组合包括：

n Non-Posted读请求和返回的完成包数据

n Non-Posted读请求和返回的完成包状态

n Non-Posted写请求和返回的完成包状态

#### 6.3.3     无限Credit通告的特殊用法

协议规范指出，当一个设备只实现了VC0时，需要有一些特别的考虑。例如，仅有的两种Posted写请求：IO Write与Configuration Write，二者仅允许使用VC0。因此，Non-Posted数据buffer并不会被VC1-VC7所使用，那么就可以为VC1-VC7宣告一个无限Credit值。然而Non-Posted Header仍然需要进行操作，所以Header Credit的值需要继续更新，而并非保持无限。

### 6.4 流量控制初始化（Flow Control Initialization）

#### 6.4.1     整体说明（General）

在发送任何事务之前，都需要进行流控初始化（flow control initialization）。事实上，在流控初始化成功完成之前，TLP是不能在链路上进行发送的。流控初始化会发生于系统中的每一个链路，其过程主要为链路两端设备之间的一次握手。这一过程会在物理层的链路训练（link training）完成后就开始进行。链路层从国观察LinkUp信号是否有效就可以知道物理层何时处于Ready状态，如图 6‑3所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image262.jpg)

图 6‑3物理层报告自己已经Ready

一旦流量控制初始化开始，那么这个过程从根本上来说在各个虚拟通道（VC）上都是一样的，当一个VC被启用时，硬件就负责控制它的流量控制初始化过程。VC0默认总是启用，因此它的初始化过程是自动的。这使得配置事务可以遍历整个拓扑结构，并进行枚举过程。其他的VC只有在配置软件对链路两端都进行了设置并且启用它们之后才会进行初始化。

#### 6.4.2     流控初始化流程（The FC Initialization Sequence）

流控初始化过程涉及到链路层的DLCMSM（Data Link Control and Management State Machine，数据链路控制与管理状态机）。如图 6‑4所示，复位后状态机将处于DL_Inactive状态。在DL_Inactive状态中，会发出DL_Down信号，这个信号会作用于链路层和事务层。与此同时，也会在此状态中等待物理层的LinkUp信号，此信号是用于表示LTSSM（Link Training and Status State Machine，链路训练和状态状态机）已经完成了工作，也就是物理层已经处于Ready状态。当物理层Ready后，将会使得DLCMSM跳转至DL_Init状态，它包含两级子状态用于处理流控初始化：FC_INIT1和FC_INIT2。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image264.jpg)

图 6‑4 DLCMSM数据链路控制与管理状态机

#### 6.4.3     FC_Init1详细说明（FC_Init1 Details）



在FC_Init1状态中，设备会连续的发送一系列的3种InitFC1流控DLLP来通告它的接收Buffer大小（见图 6‑5）。根据协议规范，数据包必须按照这样的顺序来进行发送：Posted，Non-Posted，Completion完成包，如图 6‑6所示。协议规范中强烈建议频繁的重复这些DLLP的发送，这会使接收设备能够更容易的发现它们，特别是在当前没有TLP或者DLLP需要发送的情况下。每个设备都需要从对端设备接收到这一系列DLLP，这样它才可以将对端设备的Buffer空间大小寄存下来。一旦设备已经发送出了自身的Buffer空间大小的值，并且接收到了足够次数的完整的DLLP序列，所谓足够次数就是设备足以相信接收到的对端Buffer大小的值是正确的，那么此时设备就已经可以跳出FC_INIT1状态了。要进行跳出的操作，设备需要将接收到的值记录在传输计数器（transmit counter）中，置起一个内部的flag信号（FL1），然后就可以跳转到FC_INIT2状态来进行初始化操作中的第二步。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image266.jpg)

图 6‑5 INIT1流控DLLP格式及内容

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image268.jpg)

图 6‑6设备在DL_Init状态下发送InitFC1 DLLP

#### 6.4.4     FC_Init2详细说明（FC_Init2 Details）

在FC_Init2这个状态中，设备需要连续的发送InitFC2 DLLP。发送DLLP的顺序和FC_Init1中一样，并且其中也包含了相同的有关Credit的信息，但是这些DLLP还有一个作用就是发送方用来确认流控初始化（FC initialization）已经成功。由于设备在FC_Init1状态中就已经寄存了对端的可用Buffer值，因此它不在需要任何关于Credit数量的信息，它在等待InitFC2 DLLP时会忽略传来的所有InitFC1 DLLP。尽管对于链路另一端设备来说此时并没有完成初始化，初始化的完成与否是通过DL_Up信号来对事务层进行指示的，但是设备可以在此时发送TLP。（见图 6‑7）

你可能会有疑问，初始化过程为什么还需要这个第二步呢？一个简单的回答是：链路两端的设备在完成流控初始化（FC Initialization）时需要的时间可能是不一样的，而这样两步的方法可以确保在较快的设备完成初始化后，较慢的那一端依然能够继续接收到需要的流控信息（FC Information）。一旦一个设备接收到了任意Buffer种类的FC_INIT2数据包，那么它就会置起一个内部的flag（FL2）。（这个操作不需要等待接收到所有种类的FC_INIT2数据包）注意，FL2也是在接收到一个UpdateFC包或者TLP之后才被置起的。当链路两端的设备都完成了这些步骤并且都发送出了InitFC2 DLLP之后，DLCMSM将会跳转至DL_Active状态，这也就意味着链路层已经准备好了执行常规的操作，链路层初始化完成。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image270.jpg)

图 6‑7 FC值已经寄存-接下来发送InitFC2并报告DL_Up

#### 6.4.5     FC_INIT1和FC_INIT2的传输速率（Rate of FC_INIT1 and FC_INIT2 Transmission）

在协议规范中定义了发送FC_INIT DLLP之间的延时，如下：

n VC0：基于硬件进行流控的VC0要求FC_INIT1包和FC_INIT2包要 “连续地以尽可能大的速率”进行传输。也就是说重发计时器（resend timer）设置的重发间隔值为0。

n VC1-VC7：当软件启动了其他VC（VC0之外）的流控初始化时，FC_INIT DLLP序列需要在“当没有其他TLP或者DLLP需要传输时”被重复发送。然而需要注意，两组DLLP序列发送的起始时刻最多只能间隔17us。

#### 6.4.6     流控初始化协议违例

设备对流控初始化协议违例的检测是可选的。检测到的错误可以作为数据链路层协议错误进行报告。

### 6.5 流控机制的介绍（Introduction to Flow Control Mechanism）

#### 6.5.1     整体说明（General）

协议规范定义了流量控制机制所要求的寄存器、计数器，以及一系列的机制用于报告（reporting）、追踪（tracking）和计算（calculating）一个事务是否可以被发出。这些元素并不是必需的，它们的实际实现是由设备的设计者来决定的。本节将介绍协议规范模型，并解释相关概念和对需求进行定义。

#### 6.5.2     流控相关元素（The Flow Control Elements）

图 6‑8说明了用于管理流量控制的元素。图中展示了在连路上单向传输的事务，而另一组这些流控元素则支持相反方向的传输。每个元素的主要功能都会再下面的内容中列出。虽然对于全部的6种接收buffer都重复有这些流控元素，但是为了简单起见，在本例子中仅考虑Non-Posted Header流控。

与管理流控相关的最后一个元素是流控更新DLLP（Flow Control Update），它也是在链路正常传输期间唯一可使用的流控包。FC Update包的格式如图 6‑9所示

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image272.jpg)

图 6‑8流控元素

##### 6.5.2.1   发送方流控元素（Transmitter Elements）

n 事务等待缓冲区（Transactions Pending Buffer）：把将要发送进统一虚拟通道的事务暂存起来。

n Credit消耗计数器（Credits Consumed counter）：该计数器是记录发送给对端当前类型Buffer的所有事务的Credit总和（也就是消耗了接收端多少Credit），这个计数值也可以被缩写成“CC”。

n Credit额度计数器（Credit Limit count）：该计数器的初始值由对端接收者相应类型的Buffer大小而决定。在初始化之后，对端接收者会定期的发送流控更新包（FC Update），以便在接收方Buffer可用时更新流控Credit。这个值可以被缩写为“CL”。

n 流控门控逻辑（Flow Control Gating Logic）：它用于进行计算来确定对端接受者是否有足够的流控Credit来接收等待发送中的TLP（Pending TLP，PTLP）。本质上来说，这个逻辑就是检查CREDITS_CONSUMED（CC）加上下一个PTLP所需要消耗的Credit是否大于CREDIT_LIMIT（CL）。协议规范中定义了下面的方程，以此来完成上述的检查，方程中所有的值的单位都是Credit。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image274.jpg)

关于应用此方程式的例子，请参阅“Stage 1 – Flow Control Following Initialization”一节。

##### 6.5.2.2   接收方流控元素（Receiver Elements）

n 流控缓冲区（Flow Control Buffer）：储存输入的Header或者数据。

n Credit分配计数（Credit Allocated）：记录已分配（可用）的全部的流控Credit数量。它是由硬件进行初始化的，反映了已分配的流控Buffer的大小。这些Buffer会在事务到达时被填充，但是填充进去的事务最终都会被接收端取出，也就是从Buffer中移除。当它们被移除时，这部分的Buffer空间Credit数量将会被加到CREDIT_ALLOCATED计数值上。因此这个计数器会记录并表征当前可用的Credit数量。

n 已接收Credit计数器（Credit Received counter，optional）：这是一个可选项，它用于记录进入流控Buffer的所有TLP的总Credit数量。当流控功能正常时，CREDIT_RECEIVED计数值应该小于等于CREDIT_ALLOCTED计数值。如果并未满足这二者的“小于等于”条件，那么就会发生buffer溢出并检测到出错。协议规范推荐设计者实现这一可选的机制，并且需要注意的是这里若出现违例则应该将其视作是一个Fatal Error，即致命的错误（也就是非常严重的错误）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image276.jpg)

图 6‑9流控DLLP的类型和包格式

### 6.6 流控示例（Flow Control Example）

接下来将会以Non-Posted Header流控Buffer作为例子进行讲解，并尝试捕捉出在多种情况下流控实现的细微差别。对流控的讨论将会分为如下几个基本阶段：

n **阶段一**：在初始化之后紧跟着传输一个事务，并对其进行追踪，以此来解释流控元素中的计数器和寄存器的一些基本操作。

n **阶段二**：发送方发送事务的速度大于接收方能处理事务的速度，使得接收方Buffer被填满。

n **阶段三**：当计数器翻转到0（持续增大后回到0），流控机制依然可以正常工作，但是需要考虑几个问题。

n **阶段四**：接收方可以对Buffer溢出错误进行检查，这是一个可选项。

#### 6.6.1     阶段一——初始化后的流控（Flow Control Following Initialization）

一旦流控初始化完成后，设备就已经准备好进行正常工作了。我们的示例中的流控Buffer大小为2KB，且种类为Non-Posted Header Buffer，因此1个Credit代表5dwords大小，也就是20Bytes。这意味着可用的流控单位数量为102d（66h）。图 6‑10显示出了涉及到的流控元素，包括流控初始化后各计数器和寄存器的值。

当发送方准备好要发送一个TLP时，它必须首先检查流控Credit。我们给出的例子会比较简单，因为发出的数据包有且仅有一个Non-Posted Header，它永远只会需要1个FC Credit即可，并且我们也事先假设这个事务中不存在数据荷载。

关于Header Credit的检查使用的是无符号算术（2的补码，2’s complement），它需要满足以下公式：

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image278.jpg)

代入图 6‑10中所示的值得到：

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image280.jpg)

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image282.jpg)

图 6‑10初始化后的流控元素

在本例中，将当前CREDITS_CONSUMED计数值（CC）加上PTLP所需的 Credit（Header需要1 Credit，也就是PTLP所需的Credit数量=01h），以此来确定CREDITS_REQUIRED（CR），也就是00h+01h=01h。然后再用CREDIT_LIMIT计数值（CL）减去CREDITS_REQUIRED（CR），这样就能确定是否有足够的可用Credit。

下面的描述包含了对利用2的补码（就是计算机网络中熟知的补码，另一个1的补码其实是反码）进行减法的简要回顾。当使用2的补码进行减法计算时，将要减去的数先转换为反码（1的补码）再加1，这要就得到了减数的补码（2的补码）。然后再将被减数与减数的补码（2的补码）相加，并丢弃最高位进位。

Credit检查：

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image284.jpg)

CR转换为2的补码：

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image286.jpg)

CR的2的补码加上CL：

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image288.jpg)

结果是否小于等于80h呢？是的。减法运算后的结果小于等于计数器最大值的一半，这里是用模256的计数器进行记录的，因此最大值的一半就是128，满足此条件我们就认为接收Buffer有足够的空间，可以发送当前的数据包。这其中只使用一半的计数器值的做法可以避免潜在的计数错误的问题，参阅“Stage3—Counter Roll Over”一节。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image290.jpg)

图 6‑11第一个TLP发送后的流控元素

#### 6.6.2     阶段二——流控Buffer已满（Flow Control Buffer Fills Up）

假设现在接收方已经有一段时间没有从流控Buffer中移除事务了，或许是Device Core繁忙，无法处理事务。最终流控Buffer完全被填满，如图 6‑12所示。此时如果发送方又希望发送一个TLP，那么它先进行流控Credit检查：

Credit Limit（CL）=66h

Credits Required（CR）=67h

进行CL-CR无符号减法运算，也就是减数转换为2的补码的加法运算：

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image292.jpg)

不难发现FFh<=80h并不成立，因此不能发送数据包。

这个通道会一直被阻塞，直到发送方接收到Update Flow Control DLLP（更新流控DLLP），这个DLLP将CREDIT_LIMIT（CL）的值更新为67h或者更大的值。当新的值存入CL寄存器时，发送方的Credit检查就可以通过了，也就可以发送下一个TLP了（Header只占1个Credit）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image294.jpg)

00h<=80h成立，可以发送数据包。这里之所以CREDIT_LIMIT值加1就够用是因为我们是使用Non-Posted Header流控Buffer，Header只占1个Credit。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image296.jpg)

图 6‑12流控Buffer填满后的流控元素

#### 6.6.3     阶段三——计数器翻转为0（Counters Roll Over）

由于Credit Limit（CL）和Credit Required（CR）计数只会向上增加，它们最终会翻转为0。当CL翻转而CR还没翻转时，Credit检查（CL-CR）就会出现较小值的CL减去较大值的CR的情况。然而，在使用无符号算术时，这种情况并不会引发问题。如前面的例子所描述的那样，当使用2的补码进行减法运算时，是可以得到正确的处理结果的。图 6‑13展示了CL翻转前后，CL与CR的值，以及CL-CR的2的补码减法运算结果。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image298.jpg)

图 6‑13流控计数器翻转问题

#### 6.6.4     阶段四——流控Buffer溢出错误检查（FC Buffer Overflow Error Check）

尽管这是一个可选操作，但是协议规范还是推荐实现这个FC Buffer溢出错误检查机制。图 6‑14展示了和溢出错误检查相关联的流控元素，包括：

n Credits Received（CR）计数器（注意这里CR不是前面的Credit Required）

n Credits Allocated（CA）计数器

n 错误检查逻辑（Error Check Logic）

这些元素使得接收方可以使用跟发送方相同的方式来记录流控Credit。如果流控机制正确运行，那么发送方的Credits Consumed计数值（CC）永远不会超过它的Credit Limit（CL），并且接收方的Credits Received（CR）也永远不会超过它的Credits Allocated计数值（CA）。

若检测到下面的公式成立，那么就说明出现了溢出情况。注意公式中的Field Size对Header来说为8，对数据来说为12。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image300.jpg)

当公式成立时，说明发送至FC Buffer的Credits数量已经超过了可用的数量，也就是说发生了溢出。注意在PCIe 1.0a版本中定义的公式中使用的是≥而不是＞。这是一个错误，因为CA=CR（Credits Received）时并没有发生溢出。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image302.jpg)

图 6‑14 Buffer溢出错误检查

### 6.7 流控更新（Flow Control Update）

当接收方的Buffer内有事务被移除时，也就说明这一部分Buffer空间又变为可用了，那么接收方就需要定期的用最新的流控Credit数量来更新对端设备。图 6‑15给出了一个例子，示例中发送方原本由于Buffer已满的原因阻塞了Header事务的发送，而后接收方从流控Buffer中移除了3个Header，这使得现在有可用的Buffer空间了，但是此时对端设备并不知道这件事。当3个Header从Buffer中移除时，CREDITS_ALLOCATED计数值（CA）会从66h增加至69h，这个新的计数值被报告给对端设备（发送方）的CREDIT_LIMIT寄存器，报告的方式是使用流控更新包（FC Update packet）。一旦CREDIT_LIMIT被更新，发送方就可以继续发送TLP了。

需要特别指出的一点是，这里更新报告的是CREDITS_ALLOCATED寄存器的实际值。其实我们本可以只报告寄存器值的变化量，比如“Non-Posted Header +3 Credits”，但是这种表示会存在一个潜在的问题。为了理解这其中所含的风险，我们需要考虑如果包含了寄存器变化增量信息的DLLP因为某些原因造成了丢失，此时会发生什么。对于DLLP来说是没有replay机制的，如果出现了错误那么处理方式就是简单的把DLLP丢弃。在这种情况下，增量信息将会丢失，并且没有方法恢复它。

反过来说，如果报告的是寄存器的实际值而不是增量，那么在DLLP传输失败时，下一个DLLP依然可以完成计数值的同步更新。虽然这种情况有时会浪费一些时间，比如发送方需要在一个DLLP发送失败时等待下一个DLLP来更新流控Credit，但是并不会有信息的丢失出现。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image304.jpg)

图 6‑15流控更新示例

#### 6.7.1     流控更新DLLP格式和内容（FC_Update DLLP Format and Content）

回想一下，流控更新包与流控初始化包一样，包含两个Credit字段，一个是Header Credit字段，一个是Data Credit字段，如图 6‑16所示。接收方报告在HdrFC和DataFC字段的Credit值可能已经更新了很多次，也有可能自从上次发送更新包后就没有变过。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image306.jpg)

图 6‑16流控更新包的格式和内容

#### 6.7.2     流控更新频率（Flow Control Update Frequency）

针对流控更新DLLP应该如何发送，协议规范定义了各种规则以及建议的实现方式。它们是为了：

n 尽可能早的将最新的CREDITS_ALLOCATED（CA）告知给发送方，特别是当有事务已经被阻塞的时候。

n 确立FC包之间的延时最坏情况。

n 平衡各相关的流控操作的需求，例如：

—  需要足够频繁的报告Credit信息，以此来避免事务阻塞

—  希望减少流控更新DLLP所需要的链路带宽

—  选择最优的Buffer大小

—  选择最大的数据荷载大小

n 检测流控包之间关于最大时延的违例情况。

流控更新仅在链路处于活跃状态（active state，L0 or L0s）时才能使用。所有其他的链路状态表示更加积极的电源管理，它具有更长的恢复时延。

##### 6.7.2.1   立即通告CREDITS_ALLOCATED（Immediate Notification of CA）

当一个流控Buffer已经满到无法接收最大的数据包时，协议规范要求要在Buffer出现更多可用空间时立即发送一个FC_Update DLLP。存在两种情况：

n **最大数据包大小=1Credit**。当数据包的发送由于非无限的NPH（Non-Posted Header）、NPD（Non-Posted Data）、PH（Posted Header）和CplH（Completion Header）这些种类的Buffer已满而被阻塞时，若此时有1个或多个的Credit变为可用（已分配），那么流控更新包（UpdataFC Packet）必须要被安排传输。

n **最大数据包大小=Max_Payload_Size**。对于非无限的PD和CplD的Credit类型，流控Buffer空间可能会减小到无法发送最大数据包的程度。在这种情况下，当1个或多个的Credit变为可用（已分配），流控更新包必须要被安排传输。

##### 6.7.2.2   流控更新DLLP间最大延时（Maximum Latency Between UpdateFC）

每种FC Credit类型（非无限）的流控更新包安排发送的频率至少要达到每30us（-0%/+50%）发送一次。如果设置了控制链路寄存器（Control Link Register）内的扩展同步位（Extended Sync bit），那么安排更新的频率必须不低于120us（-0%/+50%）一次。注意，实际安排发送流控更新包的频率可能要比协议规范中要求的更频繁。

##### 6.7.2.3   基于Payload Size和链路宽度计算更新频率

协议规范中提供了一个公式用于计算更新频率，这个计算是基于最大Payload Size以及链路宽度的。在下面展示的公式中，使用符号时间（symbol time）来定义流控更新的发送间隔。作为参考，1个符号时间的定义为传输1个symbol所需要的时间，Gen1为4ns，Gen2为2ns，Gen3为1ns。表 6‑3、表 6‑4、表 6‑5展示了每种速率下的原生未调整的流控更新参数值。（UF意为UpdateFactor，FC应该是写错了，应该也是UF）

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image308.jpg)

n **最大荷载大小（MaxPayloadSize****）**：这个值在设备的控制寄存器（Control Register）中的Max_Payload_Size字段。

n **TLP****开销（TLP Overhead****）**：常数值（28 symbol）表示消耗链路带宽的附加TLP组件（TLP前缀Prefix、序列号Sequence Number、包头Header、LCRC、组帧符号Framing Symbol）。

n **更新因子（UpdateFactor****）**：在两个相邻流控更新包被接收到的中间间隔的这段时间里，所能发送的最大大小TLP的数量。这个数字是为了平衡链路带宽效率和接收Buffer大小——因为这个值会随Max_Payload_Size和链路宽度而发生变化。

n **链路宽度（LinkWidth****）**：链路所使用的通道（Lane）数量。

n **内部延时（InternalDelay****）**：是一个常数值，为19个符号时间（symbol time），表示接收到TLP并发送DLLP所需要的内部处理延迟。

公式中定义的关系表明，发送流控更新包的频率随链路宽度增加而减少，建议使用计时器来触发流控更新包的调度发送。需要注意，这个公式不考虑接收方或是发送方在L0s电源管理状态时相关的延时（前面提到过仅在active状态才可以使用流控更新）。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image310.jpg)

表 6‑3 PCIe Gen1原生未调整的AckNak_LATENCY_TIMER值（单位为Symbol time）

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image312.jpg)

表 6‑4 PCIe Gen2原生未调整的AckNak_LATENCY_TIMER值（单位为Symbol time）

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image314.jpg)

表 6‑5 PCIe Gen3原生未调整的AckNak_LATENCY_TIMER值（单位为Symbol time）

*译者对上面三个表格进行举例说明，选取PCIe Gen1，Max Payload=256，x2 Link，代入公式得到((256+28)*1.4/2)+19=217.8，不难发现就是对应表中217这个值。而对于Gen2、Gen3可能发现不符合公式，其实不难发现Gen2=Gen1+51，Gen3=Gen2+45，这应该是书中没有提供一些常数参数的原因，总之Gen2表格内的值都等于Gen1对应的值加上51；Gen3表格内的值都等于Gen2对应的值加上45。

协议规范认识到这个公式其实对于许多应用场景的使用是不恰当的，比如那些使用大块数据的流的应用。这些应用可能会要求实际Buffer的大小要大于规范中给出的大小，以及使用更复杂的更新策略，以此来优化性能和降低功耗。由于一个给定的解决方案是依赖于具体应用的特定需求的，因此协议规范中并没有提供此类策略的定义。

#### 6.7.3     错误检测计时器——一个软要求（Error Detection Timer——A Pseudo Requirement）

协议规范中为流控包定义了一个可选的超时（time-out）机制，并且强烈建议实现它，目前它只是一个软要求，但是在未来版本的协议中它可能会成为一个硬性的要求。对于一种给定的Credit类型，它的两个流控包之间的最大延时间隔为120us，而对于超时机制来说超时界限为200us。对于每种流控Credit类型（Posted、Non-Posted、Completion），都各自有单独的超时计时器，且每个计时器都在收到相应类型的流控更新DLLP时进行复位。需要注意，与无限流控Credit相关联的超时计时器一定不能报告错误。

除开无限Credit的情况之外，其他情况下的超时意味着链路存在一个严重的问题。如果超时发生了，那么物理层就会被通知进入恢复状态（Recovery state），并重新进行链路训练，以期望清除错误状态。超时计时器的特性包括：

n 仅在链路处于active状态（L0 or L0s）才进行工作。

n 最大时间限为200us（-0%/+50%）

n 当接收到初始化（Init）或者流控更新包（Update FCP）时，计时器就会复位。或者可选的，也可以在接收到任何DLLP时都复位计时器。

n 超时（Timeout）会强制物理层进入LTSSM（链路训练和状态状态机，Link Training and Status State Machine）的恢复状态（Recovery State）。

 

 



## Chapter 7      Quality of Service //QoS服务质量

### 关于上一章

上一章节讨论了流量控制协议（Flow Control）的目的以及细节操作。流量控制是用来确保在接收者无法接收TLP时，发送方不会再继续发送TLP。这避免了接收Buffer溢出，也消除了原本PCI工作方式中的一些低效行为，比如断开（disconnect）、重试（retry）和等待态（wait-state）。

### 关于本章

本章节会讨论用于支持QoS（Quality of Service）的机制，并描述对网络结构中传输的不同数据包的传输时间和带宽进行控制的意义。这些机制包括，特定应用的软件将会给每个数据包都分配优先级，以及每个设备内构建可选的硬件来启用事务优先级管理。

### 关于下一章

下一章节将会讨论PCIe拓扑结构中的事务对排序的要求。这些规则是继承自PCI的，它们许多都受到“生产者/消费者（Producer/Consumer）”程序设计模型的推动，因此在下一章内会描述这个模型的机制原理。原始的规则中还考虑了必须要避免的那些死锁情况。

### 7.1 QoS的出发点（Motivation）

如今的许多计算机系统并不含有用于管理外设流量的机制，但是有一些应用是需要这种机制的。举一个例子，流媒体视频通过一个通用数据总线（General-Purpose data bus）进行传输，这要求数据要在正确的时间进行传输。在嵌入式引导控制系统中，及时的传输视频数据对于系统操作来说也是极为关键的。由于预见到了这些需求，原始的PCIe协议规范中就包含了QoS（服务质量）机制，它可以使得某些流量更优先被传输或处理。更广义的说法是，这是一种差异化的服务（Differentiated Service），因为数据包会根据被分配到的优先级不同而受到不同的处理对待，并且这种机制允许有一个范围较广的服务偏好（也就是允许的优先级比较多）。在这个范围的高的一端，QoS机制可以为这个优先级的应用提供可预测的（Predictable）以及可保证的（Guaranteed）的性能。这种级别的服务支持被称为“Isochronous（同步的）”服务（可以类比同步以太网、TTE，这里的Isochronous就是这个同步的含义），这个术语来源于两个希腊单词“isos”（相等）和“chronos”（时间），合起来就表示某个事情以相同的时间间隔发生。要使得这样的机制在PCIe中能正常工作，这要求硬件元素和软件元素的共同作用。

### 7.2 基本元素（Basic Elements）

支持高级别的服务给系统性能提出了一些要求。例如，传输速率需要足够的高，这样才能满足应用在一个时间范围内传输足够多的数据的需求，同时这也使得应用能够适应与其他流量之间的竞争。另外，传输时延也需要足够低，这样才能确认数据包及时传输到达，避免出现延迟问题。最后，必须要对错误处理进行管理，这样它才不会影响到需要及时传输的数据包的传输。要实现上述这些目标，需要有一些特殊的硬件元素，其中一种就是一组称为Virtual Channel Capability Block（虚拟通道能力块）的配置寄存器组，如图 7‑1所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image316.jpg)

图 7‑1虚拟通道能力寄存器

#### 7.2.1     流量类型TC（Traffic Class）

我们所需要的第一件事情是要用一种方式来区分各种流量，也就是需要用一些东西来区分哪些数据包具有高优先级。这种需求是通过为数据包标明流量类型（TC）来完成的，流量类型（TC）中定义了8个优先级，它通过TLP Header中的3bit TC字段来表示（TC 0-7，数字越大优先级越高）。图 7‑2所示的32位memory请求的TLP Header中标识出了TC字段的位置。在初始化过程中，设备驱动程序会与同步管理软件（Isochronous Management software）沟通服务的级别，同步管理软件会返回相应的TC值来使用各种流量类型的数据包。然后驱动程序就会给数据包分配正确的TC优先级。TC值默认为0，这样就可以让不需要高优先级服务的数据包不会干扰到那些需要高优先级服务的数据包。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image318.jpg)

图 7‑2 TLP Header中的流量类型（TC）字段

不识别PCIe的配置软件（指传统PCI的配置软件）是无法识别出这些新的寄存器的，它就会为所有的事务都分配默认的TC0/VC0。另外，也有一些数据包一直被要求要使用TC0/VC0，比如配置（Configuration）、I/O以及Message事务。如果这些数据包被认为是维护包流量，那么就有必要将它们限制在VC0中，并且要让它们原理高优先级数据包的传输路径，避免影响到高优先级包。

#### 7.2.2     虚拟通道VC（Virtual Channels）

VC（虚拟通道）实际上是硬件Buffer，它用来作为输出数据包的队列。每个端口都必须要包含默认的VC0，并且最多可以有8个VC（VC0-VC7）。每个通道都表示可供输出数据包使用的不同传输路径。使用多通道路径的想法类似于收费公路，如果司机购买了某种无线电标签，那么收费站就允许他们在高优先级车道行驶，而对于那些没有购买无线电标签的司机来说，虽然他们仍然可以在这条收费公路行驶，但是他们必须在每个收费站都停下来，且每次通过都要付钱，这使得这些司机通过收费站需要更长的时间。如果只有一条路径，那么要经过的每个人所花费的时间将受到最慢的那个司机的影响和延误，而如果有多条路径的话就意味着有高优先级的司机不会被延误（可以使用高优先级通道）。

##### 7.2.2.1   为每个VC分配TCs——TC/VC映射

分配给每个数据包的TC值在整个传输过程中是不会改变的，而且必须在每个服务点都有TC对VC的映射，也就是某种TC的数据包要使用哪条路径传输到目的地。VC映射是链路自己指定的，从一个链路到另一个链路后VC映射是可以改变的。配置软件会在初始化时建立这种映射关系，它是通过VC资源控制寄存器（VC Resource Control Register，图 7‑1）中的TC/VC Map字段来实现的。这个8bit的字段使得多个TC值可以被映射到某一个VC，因为它的每个bit都代表了一个对应的TC值（bit 0 = TC0，bit 1 = TC1，etc.）。将一个VC的8bit TC/VC Map字段中的某一位置为1，就表示将相应的TC分配给了这个VC。图 7‑3展示了一个TC-VC的映射例子，这个例子中TC0和TC1被映射到了VC0，而TC2、TC3、TC4则是映射到了VC3。

软件在分配VC ID和映射TC的方面有很大的灵活性，但是在进行TC/VC映射时也要遵循一些规则：

n 对于相同的一条链路的两端的两个端口来说，它们的TC/VC映射必须是相同的。

n TC0自动映射到VC0。

n 其他的TC（除了TC0）可以映射到任何一个VC。

n 一个TC只能映射到一个VC，而可以有多个TC映射到同一个VC。

所使用的虚拟通道的数量取决于链路两端设备所能共享的最大虚拟通道资源容量。软件会为每个VC分配ID，并且会将TC映射至VC。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image320.jpg)

图 7‑3 TC到VC的映射举例

##### 7.2.2.2   确定要使用的VC的数量

软件会检查连接在公共链路上的设备所能支持的VC的数量，并且一般会选择链路两端设备所能支持的最大数量作为VC的实际使用数量。考虑如图 7‑4所示的拓扑示例，在这里，Switch上的每个端口（port）都支持全部的8个VC，而设备A仅支持默认的VC0，设备B支持4个VC，设备C支持8个VC。需要注意，虽然Switch的端口A能够支持8个VC，但是由于设备A仅能支持VC0，因此端口A的其余7个VC是无法被使用的。相似地，Switch的端口B实际也仅能使用4个VC。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image322.jpg)

图 7‑4一个设备支持多个虚拟通道

配置软件要确定每个端口所能支持的最大VC数量，它是通过读取虚拟通道能力寄存器组（Virtual Channel Capability register，图 7‑1）中的Extended VC Count字段来完成的，这个字段如图 7‑5所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image324.jpg)

图 7‑5扩展VC支持字段

##### 7.2.2.3   分配VC ID

配置软件需要给每个VC都分配一个ID号，除了VC0（因为它是固定存在的）。如图 7‑3所示，VC能力寄存器空间中，每个VC都有自己12Byte的配置寄存器组。第一组寄存器总是由VC0使用。扩展VC计数（Extended VC Count）字段定义了当前端口实现的额外VC的数量（除VC0之外），每个实现的VC都有一组配置寄存器。注意，字段的值“n”表示的是额外实现的VC的数量，例如扩展VC计数字段为3，那么就说明除了VC0以外还有3个VC，以及它们每个都有一组自己的配置寄存器。

软件通过使用VC ID字段为每个额外的VC分配一个编号（见图 7‑3）。这些ID号并不一定要是连续的，但是必须只能出现一次而不能重复。

 

### 7.3 VC仲裁（VC Arbitration）

#### 7.3.1     整体说明（General）

当一个设备有多个VC，且每个VC都有待发送的数据包，那么就需要VC仲裁机制来决定数据包传输的顺序。软件可以从硬件所实现的几种选择方案中选择任意的一种。设计的目标是要实现所需的服务策略，并且确保所有的事务都在向前推进，以防止发生意外超时。另外，VC仲裁也会受到流量控制（flow control）和事务排序（Transaction Ordering）的相关需求的影响。这些主题内容将在其他章节中讨论，但是要注意它们也会影响仲裁，因为：

n 每个受支持的VC都有它自己的Buffer和流控处理

n 映射到同一个VC的多个事务一般会按照严格的顺序进行传输（尽管也有例外，比如一个数据包的“宽松排序Relaxed Ordering”这一属性标志位被置为1时。）

n 事务排序（Transaction Ordering）仅应用在一个VC内，因此对于分配给不同的VC的几个数据包之间是不存在排序关系的。

图 7‑6中的例子中含有两个VC（VC0和VC1），VC1：VC0的传输优先级是3:1的比率，也就是说每传输3个VC1数据包才会发送1个VC0数据包。Device Core向TC/VC映射逻辑发送请求（请求包中包含TC值）。基于已经规划好的映射，数据包被放入相应的VC Buffer中等待传输。最终，VC仲裁器决定这个由VC优先发送数据包。这个例子展示了一个单向的流程，但是同样的逻辑也适用于同时进行相反方向的传输。

VC能力寄存器提供了三种基本的VC仲裁方式：

\1.    **严格优先级仲裁（Strict Priority Arbitration****）**：在所有含有数据包的VC中，拥有最大编号的VC永远赢得仲裁。

\2.    **分组仲裁（Group Arbitration****）**:VC们被硬件分成1个低优先级组和1个高优先级组。低优先级组使用的仲裁方法是由软件从可选方法中来选取的，而高优先级组则是使用严格优先级仲裁（Strict Priority Arbitration）。

\3.    **硬件固定仲裁（Hardware Fixed arbitration****）**：仲裁方案构建在硬件中。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image326.jpg)

图 7‑6 VC仲裁示例

#### 7.3.2     严格优先级VC仲裁（Strict Priority VC Arbitration）

默认的优先级方案是基于VC ID的固有优先级（VC0=最低优先级，VC7=最高优先级）。这种机制是自动的，并不需要任何额外配置。图 7‑7展示了一个严格优先级仲裁的例子，它包含了所有的8个VC，这其中使用VC ID号来管理事务的发送顺序。使用严格优先级仲裁的VC的最大编号不能大于扩展VC计数（Extended VC Count）字段（如图 7‑5）。此外，如果设计者选择让所有的VC都使用严格优先级仲裁，那么图 7‑8中的Port VC Capability Register 1（端口VC能力寄存器1）的Low Priority Extended VC Count字段需要固定为0。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image328.jpg)

图 7‑7严格优先级仲裁

严格优先级方案要求高编号的VC永远要优先于低编号的VC。举例来说，如果所有的8个VC都使用严格优先级方案来管理，那么VC0就只能在其他所有VC都不需要发送数据包时才能发送数据包。这样的方式可以使得最高优先级的数据包得到很高的带宽以及最低的延时。但是，严格优先级方案可能会使得低优先级的通道分配不到带宽（一直有高优先级通道占用所有可用带宽），所以必须要小心地确保不会发生这种情况。协议规范中要求要对高优先级流量进行管理，以免出现低优先级通道的带宽饥饿，并给出了两种对高优先级流量进行管理的方法：

n 发起事务的端口可以限制高优先级数据包的注入速率，从而就可以为低优先级事务提供更多带宽。

n Switch可以在出口端口（egress port）调节多个流量。这种方法可能会限制某些应用和设备的吞吐量，通常这些应用设备是一些试图超过可用带宽限制的高带宽应用和设备。

设备的设计者可能也需要限制使用严格优先级的VC的数量，可以通过将VC们分为1个低优先级组合1个高优先级组来实现这一目标，在下一小节我们将会讨论这种分组方法。

#### 7.3.3     分组仲裁（Group Arbitration）

图 7‑8展示了Port VC Capability Register 1（端口VC能力寄存器1 ）的Low Priority Extended VC Count字段。这个只读（read-only）字段指定了该设备低优先级仲裁组的VC ID上限。例如，如果这个字段的值是4，那么就说明VC0-VC4都属于低优先级组，而剩下的VC5-VC7则是高优先级组。注意，若Low Priority Extended VC Count的值为7，则说明没有VC使用严格优先级。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image330.jpg)

图 7‑8低优先级扩展VC计数

如图 7‑10所展示的那样，高优先级组的VC会继续使用严格优先级仲裁，但是低优先级仲裁组则会使用设备支持的其他的一种仲裁方法。如图 7‑9所示，Port VC Capability Register 2（端口VC能力寄存器2）用来表示设备可以支持低优先级组使用哪些替代仲裁方法，而VC Control Register（VC控制寄存器）会允许具体选择了哪种仲裁方法。低优先级仲裁方案包括：

n 基于硬件的固定仲裁（Hardware Based Fixed Arbitration）

n 加权轮询仲裁WRR（Weighted Round Robin Arbitration）

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image332.jpg)

图 7‑9 VC仲裁能力字段

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image334.jpg)

图 7‑10 VC仲裁优先级（分组仲裁）

 

##### 7.3.3.1   硬件固定仲裁方案（Hardware Fixed Arbitration Scheme）

这个方案定义了一种基于纯硬件的仲裁方法，并且不需要额外的软件设置。这种纯硬件的仲裁方法可以是硬件设计者实现的任何一种方法，而且这种方法还可以基于预期的设备负载或带宽来进行设计。举一个简单的例子，一个普通的轮询调度中，每个VC都会得到相同的次数来循环轮流发送数据包。

##### 7.3.3.2   加权轮询仲裁方案（Weighted Round Robin Arbitration Scheme）

在这种方法中，某些VC可以被给予比其他VC更大的权重（更高的优先级），这些高权重VC会比其他VC得到更多的entries（条目），也就是更多的发送数据包的机会。协议规范定义了三种WRR选项，每种选项都具有不同数量的entries（称为phase“阶段”）。表项的大小通过往Port VC Control Register（端口VC控制寄存器，图 7‑9）中的VC Arbitration Select（VC仲裁选择）字段写入相应的值来进行选择。表项中的每个条目都代表一个软件使用低优先级VC的阶段。VC仲裁器会以连续的方式反复的扫描表项的所有条目，然后会通过表项条目内指定的VC来发送数据包。一旦一个数据包被发出，那么仲裁器就会马上去扫描下一个条目（entry），也就是进入下一个阶段（phase）。图 7‑11展示了一个拥有64个条目的WRR仲裁表。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image336.jpg)

图 7‑11 WRR VC仲裁表

##### 7.3.3.3   建立虚拟通道仲裁表

VC Arbitration Table（VAT，VC仲裁表）位于配置空间中，它的具体的位置的表示方法是一个偏移量，其基址为VC能力结构（VC Capability Structure）的地址，如图 7‑12所示。 

如图 7‑13所示，VAT（VC Arbitration Table）中的每个条目都是一个4bit字段，它用来指示这个阶段中需要发送数据的VC的ID号。整个VAT表的长度（有多少条目）是通过图 7‑9中的仲裁选项来进行选择的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image338.jpg)

图 7‑12 VC仲裁表偏移字段以及加载VC仲裁表字段

这个表由配置软件加载，以实现虚拟通道所需要的优先级顺序。当表发生了任何变化时，硬件会设置VC Arbitration Table Statue bit（VC仲裁表状态位），这为软件提供了一个途径来验证表项虽然发生的改变但是还没有真正作用在硬件上。一旦表被加载完成，软件就会设置端口VC控制寄存器（图 7‑9）中的Load VC Arbitration Table bit（加载VC仲裁表位）。这会使得硬件对新的表项进行加载，将新的值应用在VC仲裁器中。当表项被加载完成并应用后，硬件就会清除VC Arbitration Table Statue bit（VC仲裁表状态位），这也就通知了软件关于表项加载的操作已经完成。这种表项加载的方式的动机可能是为了在系统运行中也能更改表内容而不中断系统的运行。但是问题是配置写操作一次仅能更新1DW的表内容，它是一种相对慢速的事务，这也就意味着需要一段比较长的时间才能完成整个表内容的更新，而在这段时间里这个表仅仅进行了部分更新。换句话说，这有可能会导致设备在这段时间里继续运行时出现意料之外的行为。为了避免这种情况，这种机制允许软件完成对整个表的更改之后，在统一的一次将它们全部应用在硬件仲裁器中。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image340.jpg)

图 7‑13加载VC仲裁表条目

### 7.4 端口仲裁（Port Arbitration）

#### 7.4.1     整体说明（General）

Switch端口和Root端口经常接收到一些需要转发至其他端口的数据包。这些数据包虽然由多个端口进行接收，但是它们可能会都需要转发至同一个出口端口的同一个VC，那么这个时候就需要仲裁机制来决定这些不同端口输入的数据包如何依次的放入同一个目标VC。类似于VC仲裁，端口仲裁也有多种可选方案供配置软件选择。TC、VC和仲裁方法的多种组合可以支持一系列的服务级别，这些服务级别分为两个大类：

\1.    **异步类型（Asynchronous****）**：数据包会得到“尽力而为Best Effort”的服务，并且可能本身也没有收到任何的偏好需要考虑。许多设备和应用，例如大容量存储设备，对带宽或者延时并没有很严格的要求，并不需要特殊的定时机制。从另一方面说，通过为不同的数据包建立流量类型（Traffic Class）这种层次结构，那些需求更多的应用所产生的数据包可以仍然划分优先级并且保持其优先级地位。在服务的级别需要保证之前，即使是差异化服务也会被认为是异步的。当然，异步服务将总是可用的，而不需要任何特别的软件或者硬件选项。

\2.    **同步等时类型（Isochronous****）**：当服务需要保证延时和带宽时，我们将它移动到同步等时类型（Isochronous）这一大类中。当两个设备之间通常需要同步连接（Synchronous connection）时，这将会非常有用。例如，当耳机直接插入驱动器时，它与从音乐CD中获取数据的CD-ROM之间使用的是同步连接。然而，当音频数据必须要通过一个像PCIe一样的通用总线之后才能到达外部扬声器，那么这个时候的连接就不是同步的了，因为其他的流量也有可能要使用相同的数据流路径。为了达到相同的效果，同步等时服务必须保证在不阻止其他流量使用链路的同时，能够及时正确的传输这些时间敏感（time-sensitive）的音频数据信息。毫无疑问，这就必须要有专门的软件和硬件设计来支持它。

关于端口仲裁的概念如图 7‑14所示。需要注意，端口仲裁在系统中的多个位置都会出现：

n Switch的出口端口（egress）

n 支持Peer-to-Peer事务时的RC端口

n 某些RC出口端口，它们通向的目的地是主存

端口仲裁通常需要对Switch或者Root的出口端口的每个虚拟通道进行软件配置。在下面的例子中，Root端口2支持来自Root端口1和3的Peer-to-Peer传输，因此这里需要端口仲裁。但是需要注意的是，对于Root端口间支持Peer-to-Peer的传输是可选的，因此并不是每个Root出口端口都需要端口仲裁。

与系统内存相连接的路径是一条有趣的路径。这里可能会有来自多个入口端口（ingress）的数据包在同一时间都想要访问这一个出口端口，因此它需要支持端口仲裁。然而，它所使用的并不是一个PCIe端口，因此它并不需要通过配置我们接下来要讨论的PCIe寄存器来对端口仲裁进行支持。相反地，RC需要提供供应商指定的（vendor-specific）的一组寄存器来提供这方面的功能，它被称为Root Complex Register Block（RCRB，RC寄存器块）

因为出口端口的每个VC的端口仲裁都是独立管理的，所以每个支持可编程的端口仲裁的VC都需要有一个单独的仲裁表，如图 7‑15所示。只有Switch和RC端口才支持端口仲裁表，EP内是不允许使用端口仲裁表的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image342.jpg)

图 7‑14端口仲裁概念

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image344.jpg)

图 7‑15每个VC的端口仲裁表

尽管在协议规范中并没有说明，但是实际上针对不同的数据包流之间的仲裁需要在出口端口中使用额外的Buffer来积攒来自多个端口的流量，如图 7‑16所示。这个例子中有两个入口端口（端口1和2），它们的事务都被路由至同一个出口端口（端口3）。Switch所进行的操作包括：

\1.    到达入口端口的数据包直接根据TC/VC映射被放入相应的流控Buffer（VC）。

\2.    数据包从流控Buffer中被转发至路由逻辑，路有逻辑将决定数据包被路由至哪个出口端口，并完成路由操作。

\3.    被路由至出口端口（端口3）的数据包通过TC/VC映射，以此来确定他们应该被放入出口端口的哪一个VC。

\4.    每个入口端口都存在一组Buffer与之相关联，允许在端口仲裁完成前都可以追踪入口端口号。

\5.    端口仲裁逻辑决定每组入口Buffer发送事务的顺序。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image346.jpg)

图 7‑16端口仲裁缓冲

#### 7.4.2     端口仲裁机制（Port Arbitration Mechanisms）

实际上端口仲裁的机制的定义与VC仲裁所使用的模型比较相似。配置软件通过读取图 7‑17所示的寄存器来确定端口的能力，并为每个VC选择端口仲裁方案。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image348.jpg)

图 7‑17软件选择端口仲裁方案

##### 7.4.2.1   硬件固定仲裁（Hardware-Fixed Arbitration）

这种仲裁机制不需要软件设置。一旦它被选择使用，那么就仅由硬件来进行管理。实际的仲裁方案由硬件的设计者选择，可能会基于设备的预期需求来进行选择。它也许只是简单的保证了仲裁的公平性，或者是优化了设计的某些方面，但是它无法支持差异化或同步服务。

##### 7.4.2.2   加权轮询仲裁（WRR Arbitration）

就像VC仲裁中的加权轮询仲裁机制类似，软件可以设置端口仲裁表，以此来让一些端口在仲裁中有更多的被选中的机会。这种方法为来自不同端口的流量分配了不同的仲裁权重。

在扫描仲裁表时，其中的每个阶段（Phase）都会指定下一个被接收的数据包的入口端口号。一旦当前阶段的数据包被发送，仲裁逻辑就会立即跳转至下一个阶段。如果并没有数据包需要发送给被选中的端口，仲裁器就将会立即推进到下一个阶段去。对于仲裁表中的这些条目并没有与它们相关的时间值。

对于WRR端口仲裁，有四种仲裁表长度可以使用，仲裁表长度确定了它包含的阶段数量。若仲裁表中的条目数量越多，那么也就可以使用更复杂更特别的仲裁选择比率。而从另一方面来说，仲裁表条目数量越少则会使用更少的存储空间以及更低的成本。

##### 7.4.2.3   基于时间的加权轮询仲裁（Time-Based WRR Arbitration，TBWRR）

要对同步类型的服务进行支持，这种机制是必需的。像它名字所述的那样，基于时间的加权轮询（TBWRR）为每个仲裁阶段都加入了时间元素。就像在WRR中，端口仲裁器从仲裁表中获得当前阶段所指示的端口号，然后根据这个端口号去从相应的入口端口VC Buffer中取出数据包用于传输。而对于现在的TBWRR来说，基于时间的仲裁器并不会立即推进到下一个阶段，而是要等到当前的虚拟时隙（timeslot）过去之后才推进到下一个阶段。这确保了从入口端口接收事务的时间间隔是确定的。如果被选中的端口并没有需要发送的事务，那么也会等待至下一个时隙，而在这中间的这段等待时间不会有任何信息被发送。注意，时隙的并不控制整个传输所持续的时间，而是控制两次传输之间的间隔。这里的事务传输的最大持续时间是完成轮询并且返回到初始时隙中所花费的时间。时隙的长度也许在未来会发生变化，但是目前它的值为100ns。

基于时间的WRR仲裁所支持的最大长度的仲裁表含有128个阶段，但是一个具体的VC可用的仲裁表条目实际数量可能会低于这个数值。这个值是由硬件初始化，并通过每个支持TBWRR的VC的Maximum Time Slots字段来进行表示，这个字段如图 7‑18所示。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image350.jpg)

图 7‑18最大时隙寄存器

#### 7.4.3     载入端口仲裁表（Loading the Port Arbitration Tables）

端口仲裁表的实际大小和格式，是一个和阶段数量和入口端口数量有关的函数，这里的入口端口可以是Switch端口、RCRB（Root Complex Register Block）或者是支持Peer-to-Peer传输的RC端口。端口仲裁表所支持的最大入口端口数量为256个。实际的仲裁表中每个条目的bit数量，其设计考虑和管理都依赖于可以将事务传输至此出口端口的入口端口数量。每个仲裁表条目的大小表示在端口VC能力寄存器1（Port VC Capability Register 1）中的Port Arbitration Table Entry Size（端口仲裁表条目大小）字段，这个字段的长度为2bit。其中各个数值含义为：

n 00b：1bit（也就是在两个端口之间进行选择）

n 01b：2bits（4个端口）

n 10b：4bits（16个端口）

n 11b：8bits（256个端口）

配置软件通过将各端口号填入仲裁表来完成仲裁表的加载，以实现支持的每个VC所需的端口优先级功能。如图 7‑19所示，仲裁表的格式取决于每个条目的实际大小以及该设计中所支持的阶段的数量。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image352.jpg)

图 7‑19端口仲裁表格式

#### 7.4.4     Switch仲裁示例（Switch Arbitration Example）

现在让我们考虑一个3端口Switch的例子，以此来对端口仲裁和VC仲裁进行举例讲解。我们在示例中假设，由入口端口0和入口端口1接收的数据包会向上行移动，而端口2则是面向上行的出口端口（也就是面向RC）。下面的关于这个示例的讨论可以参照图 7‑20。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image354.jpg)

图 7‑20 Switch中的仲裁示例

\1.    到达入口端口0的数据包根据TC/VC映射关系，被端口0放入相应的接收VC中。如图中展示的一样，流量类型为TC0或是TC1的TLP会被放入端口0的VC0 Buffer；流量类型为TC3或是TC5的TLP会被放入端口0的VC1 Buffer。在这个端口的链路上，其他的TC都是不允许出现的，如果出现了一个数据包而它的TC无法正确的映射到任何一个存在的VC，那么就会被视作发生了一个错误。

\2.    到达入口端口1的数据包也会根据TC/VC映射关系被放入相应的VC中，但是端口1的TC/VC映射关系和端口0的不同。在端口1中，流量类型为TC0的TLP会被放入端口1的VC0 Buffer；流量类型为TC2-4（TC2、TC3、TC4）的TLP会被放入端口1的VC3 Buffer。链路上不允许出现其他TC的数据包。

\3.    对于端口0和端口1这两个端口来说，它们接收的数据包的目的出口端口是由每个数据包的路由信息来决定的。例如，Memory或是IO请求TLP会使用地址路由方式。

\4.    所有目的出口端口为端口2的数据包，都会被呈交给端口2的TC/VC映射逻辑。如图中所示的那样，流量类型为TC0-2（TC0、TC1、TC2）的TLP会被放入端口2中被标识有TLP各自入口端口号的VC0 Buffer；流量类型为TC3-7（TC3、TC4、TC5、TC6、TC7）的TLP会被放入端口2中被标识有TLP各自入口端口号的VC1 Buffer。

\5.    端口仲裁是端口2中的每个VC独立进行的，各个VC会将自己的一组带有各入口端口号标识的Buffer中的数据包进行排队，以此来决定哪个入口端口传来的TLP将要成为下一个真正被放入真实VC的数据包。

\6.    最后，由VC仲裁来确定在各真实的VC Buffer实际在链路上传输事务的顺序是怎样的。

\7.    注意，VC仲裁仅会在有足够的流控Credit时，才回去选择数据包用于传输。

### 7.5 多功能 EP的仲裁（Arbitration in Multi-Function Endpoints）

有一组寄存器称为Multi-Function Virtual Channel（MFVC）Capability（多功能虚拟通道能力寄存器组），这组寄存器是为了在具有多种Function的EP设备中实现QoS的这种特定情况而定义的。

在协议规范中描述了这种仲裁的两种情况。第一种情况，如图 7‑21所示，EP中虽然有两个Function，但是只有Function 0中含有VC能力寄存器（VC Capability Registers），并且所有的Function的任务分配都是相同的。对于这种情况，Function之间的仲裁方式将按照一些厂商指定（Vendor-Specific）的方法去进行。这是最简单的方法，但是这无法在内部包含一个标准结构用于定义不同Function的请求的优先级，因此它无法支持QoS。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image356.jpg)

图 7‑21简单的多Function仲裁

如果希望支持QoS，那么就需要在VC0中实现一个MFVC（Multi-Function Virtual Channel）寄存器组，并且每个Function都要有自己独立的一组VC能力寄存器。为了保持软件的向后兼容性，协议规范中声明，所有不适用MFVC的设备的VC Capability ID（VC能力ID）都必须为0002h，而所有内部实现了MFVC的设备的VC Capability ID都必须为0009h。

图 7‑22展示了MFVC寄存器组，以及一个示例框图，示例中的EP内含有两个Function，这个EP的端口支持2个VC。每个Function都有自己的事务层以及VC能力寄存器组，但是并没有实现更低的层级。相反地，它们都连接到了二者共享的端口的事务层，这个共享端口具有所有的层级。这种共享硬件接口的方式可以降低成本和开销，并且各Function事务层种添加的MFVC寄存器组使得Function可以处理同步等时流量（Isochronous Traffic）。

在图 7‑22中可以看到，MFVC寄存器仅存在于Function 0中，并定义了此接口使用的VC以及仲裁方法。MFVC寄存器组看起来与VC能力寄存器组十分相似，它可以支持VC仲裁与Function仲裁。由于来自多个不同Function的数据包有可能在同时访问同一个VC，因此就需要Function仲裁来决定不同Function数据包之间的优先级。这种方式在现在看来应该是非常熟悉的，因为它与此前的端口仲裁其实是相同的概念，甚至连仲裁方法选项都是一样的，包括TBWRR。VC仲裁选项也与单Function VC寄存器中的仲裁选项相同。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image358.jpg)

图 7‑22多Function仲裁中的QoS支持

### 7.6 同步等时服务支持（Isochronous Support）

前面的内容中提到，并不是每个设备或者应用都需要支持同步等时服务，但是也有一些是必须要支持同步服务的。由于PCIe从设计之初就想要支持同步等时服务，因此让我们来考虑一下要支持它的话会有什么样的要求。

#### 7.6.1     时间规划就是一切（Timing is Everything）

考虑图 7‑23中展示的例子，例子中的同步连接（synchronous connection）使我们所期望的，但是这是无法实际实现的。相反地，我们通过通过同步等时机制来模拟一个同步连接。在这个例子中，等时性（Isochrony）定义了在服务间隔内需要被传输的用来完成服务需求的数据量。下面对操作流程进行了描述：

\1.    同步源（摄像机和PCIe接口）在第一个等时服务间隔（Service Interval,SI 1）中在Buffer A积累数据。

\2.    摄像机在下一个服务间隔（SI 2）中通过通用总线发送完Buffer A中所有被缓存积累的数据，同时也将下一块新数据积累在Buffer B中。

明显地，系统必须要能保证Buffer A中的全部内容要在这个服务间隔内发送，不管链路上是否还有其他流量在传输。要做到这一点，可以给这些时间敏感（time-sensitive）数据包分配高优先级，并且按照需要规划仲裁方案使得这些数据包即使在有其他流量竞争的情况下也能被优先处理。还需要注意，只要所有的数据在时间窗口内发送出去即可，而至于它们具体什么时间到达在这里并不关心，它们可能会在服务间隔中很分散的发送，也有可能集中在服务间隔的某一时刻紧密的发送。只要它们都在服务间隔中被发送出去，那么就依然可以做到需要的保证。

\3.    在SI 2中，录音机接收到数据，并将这些输入数据缓存起来，随后这些数据可以在SI 3被传递到存储介质来完成录制。摄像机也会在SI 3期间将Buffer B中的数据卸到链路上，同时也在SI 3内在Buffer A中积累新的数据，以此循环进行。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image360.jpg)

图 7‑23同步事务的示例应用

##### 7.6.1.1   如何定义时间规划（How Timing Defined）

PCIe使用TBWRR端口仲裁机制中的时隙（time slot）来定义同步等时时长。目前，每个时隙是100ns，它代表TBWRR仲裁表的128个条目中的1个。一旦设置完成，仲裁器就会每12.8us循环重复一次这个仲裁表，这也代表全部的服务间隔。

要想让一个同步等时路径像预期的一样工作，这需要有一定的考虑。首先，数据包必须在可以测的时间点、以固定的间隔被发送。第二，同步等时数据需要被传输的最大数据量必须是要提前可以获知的，并且数据包不允许超过这个最大值上限。第三，链路必须要有足够的带宽来支持一个给定时隙内所要传输的数据量。

考虑接下来这个例子。一个单通道链路工作在2.5Gbps，每4ns发送一个符号（10bit符号）。也就是说每个100ns的时隙可以发送25个符号，但是这足够吗？在很多情况下这是不够的，因为1个TLP需要28byte组成 Header、序列号（Sequence Number）、LCRC等等的常规开销。这意味着100ns根本不足以完成这些常规开销的发送，更不用说发送数据荷载了。如果我们需要发送128byte的数据，那么需要的带宽就将是128+常规开销=156bytes。解决这个问题的一个选项是将链路宽度增加至8通道，这使得一次可以传输的数据量变为原来的8倍。这种改变会使链路在100ns内可以传输200bytes，那么也就可以在1个时隙中发送所有的同步等时数据。另一种解决方法是依然使用单通道链路，但是可以给予端口更多的时隙用于发送数据，比如将服务间隔由1个时隙增加到8个时隙，那么就也可以在1个服务间隔中传输200byte数据了。具体的选择哪种解决方法取决于成本和性能的权衡，但是系统的设计者必须知道同步等时路径对时间规划和带宽的要求，这样才能正确的建立同步等时路径。

##### 7.6.1.2   如何实施时间规划（How Timing is Enforced）

像前面的例子中所描述的，为了要使一个设计进行正确的操作，时间规划已经成了必不可少的一部分，它是由到目前为止我们所讨论的多种元素的组合来完成执行的。首先，软件中必须选择高优先级TC，在硬件中配置VC，并定义TC到VC的映射关系，以此来让正确的数据包放入高优先级VC中。然后，期望的的时间规划其实是一个仲裁方案规划问题，通过这个规划来适配特定时间内所需要的带宽。例如，VC仲裁可能会使用严格优先级仲裁，因为这是唯一可以确保高优先级数据包不会被其他数据包阻塞延迟的仲裁方式。而对于端口仲裁则必须是TBWRR，这样才能执行确定好的时间规划。

#### 7.6.2     软件支持（Software Support）

要支持同步等时服务，还需要系统中各个软件元素之间的协调合作。在一个PC系统中，设备驱动会向OS（操作系统）报告同步等时服务的需求和能力（Capabilities），OS会根据这些信息对整个系统的需求进行评估，并为系统分配合适的资源。嵌入式系统会有所不同，因为所有的这些各部分的信息在一开始就是已知的，所以这里的软件可以更简单一些。在接下来的讨论中，我们将会描述PC系统中的情况，因为嵌入式系统的情况只是PC情况中更简单的一个子集。

 

 

##### 7.6.2.1   设备驱动程序（Device Drivers）

一个设备驱动必须能够将时间规划的需求报告给用于监督同步等时操作的软件，并在真正尝试发送同步等时服务数据包之前，先要获得这个软件的许可。需要注意的重要的一点是，驱动级软件不应该自己直接改变硬件配置或者是仲裁策略，即使它有这个能力也不行，因为这将会引起混乱。如果多个驱动程序都各自独立的尝试更改硬件配置或仲裁策略，那么最后进行更改操作的驱动程序将会把之前的更改都覆盖掉。为了避免这种情况，一种被称为同步等时代理（Isochronous Broker）的OS级程序将会接收来自系统设备的时间规划请求，并以一种协调的方式来分配系统资源，以此来适配所有的这些请求。

##### 7.6.2.2   同步等时代理（Isochronous Broker）

这种程序用来管理端到端的同步等时数据包流。它从设备驱动接收到同步等时时间规划请求，用一种能适配目标路径上的各个请求的方式，来协调分配系统资源。在协议规范中，这被称为“在一对Requester/Completer（请求者/完成者）和PCIe结构之间，建立了一个同步等时契约（Isochronous Contract）”。要做到这些，需要确定期望的路径上确实可以支持同步等时流量，然后就编程写入合适的仲裁方案，以确保它在指定的时间规划要求下工作。

#### 7.6.3     将一切结合起来（Bringing it all together）

到目前为止，我们应该已经相当清楚需要做些什么来支持系统中的同步等时流量，但是让我们来看最后一个例子，在这个例子里将会把之前的各个内容部分全都结合起来。如果我们继续对此前的视频捕捉示例进行展开详述，以此来展示一个更加复杂的系统，如图 7‑24所示的系统，当摄像机可以把捕捉到的视频数据传送至系统存储时，我们就可以基于它来讨论这个系统中所必需的每个部分。这对于同步等时服务来说是一个比较困难的系统环境，因为系统中存在许多需要竞争路径上的带宽的设备，但是这也使得这个例子非常适合用来讲解这些过程中需要考虑的方方面面。

##### 7.6.3.1   Endpoints

让我们从系统的最底部开始，对于录像EP设备本身，它的PCIe接口中需要有什么？在硬件中，如果我们想要区别不同的数据包，那么就需要1个以上的VC。为了简化例子，我们假设这个设备是一个单Function设备。设备驱动程序需要向OS级的同步等时代理程序报告设备的能力（Capabilities）和同步等时时间规划需求，同步等时代理程序将会根据这些信息对系统进行评估，并向设备驱动报告是否可能建立同步等时契约以及软件应该使用哪些TC。

驱动程序则将会将对VC的编号和数量进行编程写入，并将相应的TC映射至各VC。它也很有可能会将高优先级通道的VC仲裁编程为严格优先级仲裁（Strict Priority）。这里需要注意的一点是，仲裁必须要是“公平的”，意思是那些低优先级通道不能出现访问机会匮乏（Starve）。这意味着那些高优先级VC不能一直占用仲裁让自己的流量处于待发送状态，而是要随时间分批的传输数据包。

在结束对EP的讨论之前，还需要注意关于链路操作的另一个问题，那就是流量控制。设备在同步等时路径上的接收Buffer必须要足够大，只要数据包是按照同步等时契约（Isochronous Contract）均匀的输入，那么这个Buffer就要大到在不需要任何反压的情况下能够处理预期的输入数据包流。此外，流控更新DLLP（Flow Control Update）的返回速度必须足够快，以此来避免传输停滞。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image362.jpg)

图 7‑24同步等时系统示例

##### 7.6.3.2   Switches

在讨论了EP之后，接下来让我们考虑一下在EP与RC之间的Switch中需要提供什么。Switch通常是没有自己的设备驱动程序的，所以只能由OS级软件（如同步等时代理Isochronous Broker）来读取它们的配置信息，以此来确定这些Switch能支持怎样的服务。首先，在同步等时路径上的所有端口都必须要支持至少2个VC，且链路两端的TC/VC映射关系也必须是相匹配的。要记住，一旦数据包到达了Switch端口的事务层，那么数据包里只有TC信息而没有VC信息，TC具体对应哪个VC是每个端口自己指定的而不会将这个信息携带在TLP中。Switch 1的下行端口与它下方连接的EP的各自TC/VC映射关系必须要相匹配，但是至于其他的Switch端口可以是不同的以此来匹配它们各自链路上的EP。

###### l 仲裁问题（Arbitration Issues）

仲裁方法的选择是很直接的。为了简单起见，在我们的例子中的同步等时路径上承载的流量是单向的。对于在存储器读取操作中，其实是有可能出现双向的同步等时流量的，但是在我们的例子中选取的是类似视频流的情况。

同步等时出口端口使用的VC仲裁通常是严格优先级方案，原因与EP相同。端口仲裁则将会使用TBWRR方案，这意味着软件必须了解适当的访问比率，并将端口仲裁表（Port Arbitration Table）编程写入，以此来实现这种端口的访问比率。如果路径中有多个Switch，那么要做到这一系列事情可能并不是听起来的那么简单，因为即使他们使用的都是相同的TBWRR仲裁方案，也无法很清晰的知道它们各自的服务间隔（service interval，SI）是如何协同工作的。如果几个Switch的SI之间并未对齐，那么就意味着等时的时间规划更加难以实现，这将取决于链路的繁忙程度。但是协议规范中并没有考虑SI之间的协调合作，因此这里就将再次涉及到一个非标准方法。显然，如果在一个同步等时路径上并不存在多个Switch而只有1个，那么这个问题就将会被大大简化。

###### l 时间规划问题（Timing Issues）

图 7‑25展示了我们的例子中数据包在两个EP间传送的时间点。粗线条的大箭头是来自视频设备的数据包，它具有已知确定的大小，并按照周期性的、确定性间隔的进行发送。细线条的小箭头则是代表来自SCSI驱动器的数据包，它具有较低的优先级且发送时间点是不可预知的。在EP中，数据包只需要简单的含有TC即可，但是Switch还需要确认执行了合适的时间规划方案。这是通过使用TBWRR来完成的，它将会指定在一个给定的时间点上，哪个端口可以进行访问，以及访问时间有多长。由于知道同步等时数据包的大小以及传送频率，这使得软件可以正确的安排时间规划，但是什么样的时间规划才是我们需要的呢？

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image364.jpg)

图 7‑25同步等时数据包的注入

首先，让我们通过一个简单的示例来回顾一下需要涉及到的参数。回忆一下，PCIe中基于参考时钟周期定义了时隙（time slot），而时隙的长度是由端口能力寄存器1（Port Capability Register 1）中的参考时钟字段（Reference Clock）所给定的。目前，这个字段的值只能是100ns，并且TBWRR仲裁表只能是128个条目的大小。服务间隔（Service Interval，SI）的长度就是它们的乘积，也就是12.8us。一个给定设备的带宽可以用下面给出的公式来表达，其中Y表示1个时隙中将要发送的数据量（协议规范中声明，必须要将配置过程中编程写入的Max Payload Size值用于这个带宽的计算中，也就是先假设1个时隙就能传输1个数据包），M表示的是时隙的数量，T表示整个SI长度。举例来说，如果我们选择Payload为128Byte，又已知SI为12.8us，那么对于分配的每个时隙来说，BW=10MB/s。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image366.jpg)

现在让我们考虑一个更贴近实际的例子。假设链路工作在Gen2的速率，视频设备需要保证有100MB/s的带宽，且它将会发送数据荷载为512Byte的数据包，我们先假设1个时隙就能发送1个数据包，代入公式中得到M=2.5，也就是在这种情况下要在1个SI中发送2.5个512Byte数据包。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image368.jpg)

但是实际上1个时隙中可以传输多少数据呢？这个问题的答案当然与链路速率以及链路宽度有关。在5.0Gb/s的速率下，每发送1个10bit符号就需要2ns，因此在每个同道中100ns则可以传输50个符号。如果数据包的大小是512Byte再加上28Byte左右的Header，那么使用x1链路传输大小为550个符号的数据包则需要11个时隙。在有需要的时候，可以让同一个端口得到连续的多个时隙，这是一种解决办法。由于将要发送的数据包的大小是确定一致的，我们不能真的就将M定为2.5个512Byte，而是应该将其定为3。对于我们的公式来说，在1个SI中发送3个512Byte的带宽实际上是120MB/s。这虽然高于我们的需求带宽，但是确实解决了我们的问题。由于刚才计算过传输550个符号需要11个时隙，那么传输3个就需要11×3=33个时隙，在1个SI中还留有95个时隙用作它用。在1个SI中的3个时隙组（每组11个）中，每一组内的11个时隙必须是连续相邻的，但是组与组之间可以是分开的。

另外一个解决办法就是增加链路宽度。尽管这样的解决办法会增加硬件开销，但是如果使用11通道的链路则可以真正的在1个时隙内完成需要的数据的发送但是在CEM协议规范中（PCIe Card Electromechanical Spec ，PCIe卡Spec），并没有x11的选项，但是可以使用其中的x12选项来应用在我们的例子中。使用更宽的链路意味着软件只需要为每个数据包分配1个时隙，因此只需要在1个SI中分配3个时隙，即可满足本例子中设备的同步等时流量带宽需求。与x1的情况不同，现在我们不需要让分配的时隙连续相邻。相反地，它们可以以某种更优的方式分散在SI中。

###### l 带宽分配问题（Bandwidth Allocation Problems）

对于同步等时流量来说，必须要设计好TBWRR仲裁表来保证有足够的及时的带宽，并且这部分带宽不会被其他流量所影响。如图 7‑25中所示，SCSI控制器在SI 1中发送了1个数据包，在SI 3中发送了另一个数据包。如果时间规划就按照这样，允许EP每个SI发送1个数据包，那么这样的方式是可行的。

现在我们假设SCSI控制器尝试发送更多的数据包，其数量已经超过了SI 1对它的许可数量，如图 7‑26所示。这是协议规范中提到的两个带宽分配问题中的第一个，它被称为“过载（oversubscription）”。这将会影响到同步等时流量，但是这个问题可以通过合理的规划TBWRR仲裁表就能比较容易的避免，因为在某个特定的时间点下，只有被仲裁选中的端口可以发送数据包。如果这个端口在发送完仲裁许可的数据包后依然有很多数据包在队列中等待发送，那么它也需要等到下一次被仲裁选中才能继续发送后续的部分数据包，对于这个例子中也就是SCSI控制器想在SI 1中多发送的那一个数据包实际上可能会放到SI 2中去发送。最终，这种“过载”会导致发送处的流控反压。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image370.jpg)

图 7‑26超额使用带宽

第二个问题被称为“拥塞（congestion）”，当在一个给定的时间窗口内存在了太多同步等时服务请求时，就有可能会发生这种问题，如图 7‑27所示。这个问题和第一个有点相似，但是它是无法那么简单就解决的。与之前的情况不同，这里不能将高优先级数据包推迟到下一个时隙发送，因此系统必须要有办法来处理全部的这些请求。要纠正拥塞的问题，软件需要改变发送数据包时间点的分布，以便它们都能够得到可用硬件带宽的支持。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image372.jpg)

图 7‑27带宽拥塞

###### l 延时问题（Latency Issues）

管理数据包发送的延时也是等时性很重要的一部分，这个延时包括网络结构传播延时与Completer延时。网络结构传播延时取决于系统中各种组件之间的链路的全部特性，特别是链路宽度和操作频率。一种将这个网络结构传播延时值最小化的简单方法，就是对PCIe拓扑结构的同步等时路径上的复杂度进行约束。Completer延时取决于目的EP内部特性，例如存储速率以及内部仲裁方式等等。

 

##### 7.6.3.3   Root Complex

RC的仲裁和时间规划需求和Switch是一样的。它会从多个下行端口接收数据包，并通过符合前面所描述的等时性规则的方法将它们转发至目的地。然而，这些事情的完成方法大都是Vendor特定的，因为协议规范中并没有定义同步等时服务的RC工作方式或者是它应该如何被编程。

###### l 问题：窥探（Problem：Snooping）

在影响RC的时间规划和延时的各种因素里，我们还有一个没有进行讨论，那就是窥探过程（process of snooping）。一般情况下，对系统内存的访问会发生在处理器认为可缓存（cacheable）的位置，这意味着这个位置的内存可以在它的本地Cache中保存一个自己的临时副本。如果外部设备尝试访问这个内存区域，那么芯片组必须先检查处理器Cache，然后才能允许此次内存访问，因为Cache中缓存的副本可能已经被更改过，与原内存区域中的数据已经不相同了。如果Cache中缓存的副本确实被更改过，那么在设备访问这个内存区域之前，先要让Cache中的数据写回内存（Write Back）。虽然对内存Cache一致性的确认工作是有必要的，但是问题就是这个窥探过程是需要花费时间的。它需要花费的时间通常是在一个范围内有限的，但是是不可预测的，因为它取决于CPU在当时具体正在执行什么工作。结合上时间规划的时间确定性需求，窥探时间的不确定性很有可能会破坏同步等时数据流。

###### l 窥探问题解决方法（Snooping Solution）

对于外部设备来说，一种避免窥探的方法就是仅访问不可缓存（Uncacheable）的内存区域。另一种方法是由软件设置高优先级数据包Header中的“No Snoop（无窥探）”属性位。这种方式将会强制芯片组跳过窥探步骤，不管内存是否是uncacheable都直接去访问内存，这需要软件用一些方法来保证这样做不会引发问题。为了将这种方式作为同步等时路径的一个需求来执行，对于高优先级VC来说，硬件可以对RC端口中的另一个bit进行初始化配置，它叫做“拒绝窥探事务（Reject Snoop Transaction）”，如图 7‑17中所示的VC资源能力寄存器。这样做的目的是，让这个VC只允许传输No Snoop的事务。任何输入的没有设置No Snoop属性位的数据包豆浆杯丢弃，以此来确保时间规划的时间确定性不会被窥探过程花费的不确定时间所破坏。

##### 7.6.3.4   电源管理（Power Management）

电源管理是一个简单的监测过程，但是当PCIe中的一段路径是时间敏感路径的时候，那么这个路径上设备的电源管理机制PM（Power Management）就需要仔细的处理。配置软件可以读取到每种PM条件下造成的延时，并从中选择一种PM条件使得它的延时可以满足时间规划要求。最简单的办法就是在同步等时路径上禁用PM。设备可以进入设备状态D0然后就停留在这个状态，同时禁用硬件控制的链路电源管理机制（更多关于PM的内容请参阅Chapter 16“Power Management”）。

##### 7.6.3.5   错误处理（Error Handling）

最后，还剩下最后一个问题：当链路上出现错误时应该怎么办。ACK/NAK协议提供了一个自动地、基于硬件的重试机制（Retry）来纠正出现传输问题的数据包。这个原本还不错的特性会给等时性带来一个问题，因为它需要花费时间来完成，并且它用来解决几个传输错误花费的时间也会有较大差异，这取决于具体检测到了什么样的错误。

要解决这个问题，我们需要知道系统能容忍多少不确定的时间，使得系统仍能提供同步等时数据传输。如果延时时间预算太紧张，那么显然就没有时间可以用来重试传输失败的数据包，也就需要禁用ACK/NAK协议。有趣的是，PCIe的协议规范制定者显然没有考虑到这一点，因为并没有设计用来禁用ACK/NAK协议的配置bit，也没有设计配置bit用来决定如何处理那些本来会被重传而现在不会被重传的数据包。因此，禁用ACK/NAK协议需要使用一些非标准机制，例如Vendor-specific寄存器。

如果没有足够的时间用于重传，那么目的方可能就会简单的将损坏的数据包丢弃。另一个选择是就直接使用损坏、错误的所有数据包。对于一些使用同步等时服务支持的应用来说，使用损坏数据包的这种操作并没有听起来这么不可接受。例如，视频流中的错误可能会导致显示器偶尔出现故障闪烁，但是这个风险被认为是可以接受的。

不同于上面的情况，如果在服务间隔（Service Interval）中，有足够的时间用于重传，那么就可以通过添加一个计时器的方式来给重试延时设置一个上限，这个计时器会一直记录时间直到当前SI结束，可以使用它来决定是否可以进行重试操作。当然，错误情况不应该很频繁的出现，所以这种方式对于偶尔出现的传输错误来说是足够的，并且同时也可以保持同步等时的时间规划。

 

 



## Chapter 8      Transaction Ordering //事务排序

### 关于上一章

上一章节讨论了用于支持QoS（Quality of Service）的机制，并描述了对网络结构中传输的不同数据包的传输时间和带宽进行控制的意义。这些机制包括，特定应用的软件将会给每个数据包都分配优先级，以及每个设备内构建可选的硬件来启用事务优先级管理。

### 关于本章

本章节将会讨论PCIe拓扑结构中事务对排序的要求。这些规则是继承自PCI的，它们许多都受到“生产者/消费者（Producer/Consumer）”程序设计模型的推动，因此在本章内会描述这个模型的机制原理。原始的规则中还考虑了必须要避免的那些死锁情况。

### 关于下一章

下一章节将会描述Data Link Layer Packet（DLLP，数据链路层包）。我们将会描述其用途、格式以及DLLP数据包类型的定义，也将会对其相关字段的细节进行描述介绍。DLLP可以被用来支持Ack/Nak协议、电源管理、流控机制，还可以被用来做一些vendor自定义的事情。

### 8.1 介绍（Introduction）

和其他协议一样，对于同时要在网络结构中进行传输的、拥有相同TC（Traffic Class，流量类型）事务，PCIe规定了它们之间的排序规则。而对于拥有不同TC的事务之间则并不存在排序关系。对相同TC的事务指定排序规则的原因有：

n 维持与传统遗留总线的兼容性，例如PCI、PCI-X和AGP。

n 保证事务的完成是确定的（deterministic），且是按照程序员想要的顺序号进行的。

n 避免死锁情况。

n 通过将读取延时最小化并管理读写的顺序，来将性能和吞吐量最大化。

具体的PCI/PCIe事务排序的实现基于以下的特性：

\1.    生产者/消费者编程模型（Producer/Consumer），它是基本排序规则的基础。

\2.    宽松排序（Relaxed Ordering），当请求者知道某个事务与先前的事务没有任何依赖关系时，则允许出现宽松排序。

\3.    ID排序（ID Ordering），在ID排序规则下，Switch可以允许来自某一个设备的请求先于另一个设备的请求，因为这两个设备正在执行不相关的线程。

\4.    一些避免死锁情况的方法以及支持PCI传统设备实现的方法。

### 8.2 定义（Definitions）

在数据流中有三种通用模型用于事务排序：

\1.    **强排序（Strong Ordering****）**：对于在PCIe结构中传输的拥有相同TC的多个事务流，PCIe要求使用强排序规则。拥有相同TC的事务们也会被映射到同一个给定VC，因此每个VC中也要应用强排序规则。因此，当多个TC都被映射到同一个VC时，这些TC的所有事务都会被当做同一个TC来进行处理，即使不同的TC之间不存在排序关系。

\2.    **弱排序（Weak Ordering****）**：除非事务需要重新排序，否则它们将维持原来的顺序。由于某些给定的事务模型（例如Producer/Consumer模型）相关的依赖性，维持事物间的强排序关系可能会导致所有的事务都被阻塞。而被阻塞的事务中，有些与依赖关系并不相关联，它们就可以安全地被重新排在阻塞的事务之前。

\3.    **宽松排序（Relaxed Ordering****）**：在且仅在某些可控的情况下，事务可以被重新排序，它的好处是可以像弱排序模型一样提升性能，但是它仅在软件指定是才会这样做，以此来避免依赖问题。而它的缺点则是仅有部分事务可以得到性能上的优化。软件要启用宽松排序（RO，Relaxed Ordering）也需要一些额外开销。

### 8.3 简化后的排序规则（Simplified Ordering Rules）

PCIe 2.1版本中介绍了一种简化版的排序规则表，如所示。这个表格可以按主题划分如下：

n 生产者/消费者（Producer/Consumer）规则

n 宽松排序（RO）规则

n 弱排序规则

n ID排序规则

n 死锁的避免

这些小节中会给出与各个排序模型、操作、基本原理、情况以及要求相关的细节内容。

#### 8.3.1     排序规则与流量类型（Ordering Rules and Traffic Classes）

PCIe中的排序规则会应用于拥有相同TC（流量类型）的事务之间。对于那些在网络结构中传输的具有不同TC的事务来说，并没有针对它们的排序要求，可以认为它们各自的应用程序之间是没有关联的。因此对于不同TC的数据包来说，也就不存在由于事务排序而带来的性能降低问题。

对于拥有相同TC的数据包来说，当它们在PCIe网络结构中传输时，有可能会经历性能降低。这是因为Switch和设备们必须要支持一些排序规则，而这可能会要求将数据包延迟，或者在先发送来的数据包之前将后发送来的数据包转发出去。

像Chapter 7“Quality of Service服务质量”一章中讨论的那样，拥有不同TC的事务可能会被映射到同一个VC。TC-to-VC映射关系的配置决定了一个给定了TC的数据包将会被映射到哪一个指定的VC去。尽管事务排序规则仅应用于相同TC的数据包之间，但是可能也可以更简单的设计EP/Switch/RC令它们将事务排序规则应用在同一个VC中的所有数据包之间，即使在一个VC中可能含有多个TC的数据包。

显然，映射到不同VC的数据包之间没有排序关系。

#### 8.3.2     基于包类型的排序规则（Ordering Rules Based On Packet Type）

PCIe协议规范中定义的排序关系是基于TLP类型的。TLP被分为三个种类：

\1)   Posted

\2)   Completion

\3)   Non-Posted

Posted种类的TLP包括MWr请求（Memory Write request）与Message（Msg/MsgD）。Completion种类的TLP包括Cpl与CplD。Non-Posted种类的TLP包括MRd、IORd、IOWr、CfgRd0、CfgRd1、CfgWr0以及CFGWr1。

在下一小节中使用了一个表格（表 8‑1）来描述事务的排序规则，你将会看到这个表格中列出了上面提到的三个种类的TLP以及它们所定义的排序关系。

#### 8.3.3     简化后的排序规则表（The Simplified Ordering Rules Table）

该表按照Row Pass Column（行传递列）的方式对内容进行组织。所有的排序规则都在简化后的排序表（表 8‑1）之后进行总结概括。每种规则，或者每组规则，都定义了它们要求的操作内容。

在表 8‑1中，列2-5代表先前已经被PCIe设备发送的事务，而行A-D则代表1个刚刚到达的新的事务。对于出站事务（outbound transaction），该表指定了行（A-D）所代表的事务是否可以超过一个由列（2-5）所代表的先前的事务。表中的“No”表示行事务不允许超过列事务，“Yes”表示必须允许行事务超过列事务以此来避免死锁，而“Yes/No”则表示行事务可以超过列事务但是并不要求一定要这么做。表中各个条目均有其含义。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image374.jpg)

表 8‑1简化后的排序规则表

n **A2a,B2a,C2a,D2a**：为了执行生产者/消费者模型，后来的事务不允许超过超过一个先前的Posted请求。

n **D2b**：如果设置了RO（Relaxed Ordering），那么一个Read Completion是可以超过一个先前已排队的MWr或是Message请求的。

n **A2b,B2b,C2b,D2b**：如果使用了可选的IDO（ID Ordering），那么后来的事务是允许超过一个Posted请求的，只要它们两个的Requester ID不同。

n **A3,A4**：一个Mwr或者Message请求必须要可以超过Non-Posted请求，以此来避免死锁。

n **A5a**：Posted请求可以但不要不超过Completion。

n **A5b**：避免死锁的情况。在一个PCIe-to-PCI/PCI-X Bridge中，为了让事务从PCIe传到PCI或PCI-X，Posted请求必须要能超过Completion，否则将有可能发生死锁。

n **B3,B4,B5,C3,C4,C5**：这些情况实现弱排序，且不会有任何与排序相关问题的风险。

n **D3,D4**：Completion必须要能超过Read、IO或者是配置写请求（Non-Posted请求），以此来避免死锁。

n **D5a**：拥有不同事务ID的两个Completion都可以超过对方。

n **D5b**：拥有相同事务ID的Completion们不能超过对方，要保持原排序。这是为了保证在给单个请求返回多个Completion时，仍然可以保持地址的递增顺序。

### 8.4 生产者/消费者模型（Producer/Consumer Model）

这一节将会描述Producer/Consumer（生产者/消费者）模型的操作方式，以及为了正确执行这个模型所需要的排序规则。图 8‑1中简单的展示了一个用于示例的拓扑结构。在接下来的使用这个拓扑结构的第一个示例中将会描述Producer/Consumer模型在使用合适的排序规则时的操作方式，然后会再给出一个例子用于描述Producer/Consumer模型在使用不合适的排序规则时导致操作错误的失败情况。

Producer/Consumer模型是PCI和PCIe中数据传输时的常用方法。这个模型由图 8‑1中所展示的5个元素所组成：

n 数据的Producer（生产者）

n 内存数据Buffer

n 用于指示数据已经由生产者发送的Flag semaphore（标志信号量）

n 数据的Consumer（消费者）

n 用于指示消费者已经读取了数据的Status semaphore（状态信号量）

协议规范中指出，无论所有的相关的元素是如何排列放置的，Producer/Consumer模型都会执行工作。在本例中，Flag和Status元素都位于同一个物理实体设备，但是其实它们也可以位于不同的设备中。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image376.jpg)

图 8‑1生产者/消费者拓扑示例

#### 8.4.1     正确的生产者/消费者流程（Producer/Consumer Sequence-No Errors）

下列的讨论请参照图 8‑2。该示例中假设从一开始Flag和Status元素就已经清除复位了。这两种信号量在本例中都位于同一个设备中。下面的描述中带有序号的事件流程，以及图 8‑2所示的流程，都反映了正确排序规则流程的第一部分。

\1.    在示例中，一个被称为**Producer****（生产者）**的设备将会执行一个或多个Mwr事务（Memory Write，Posted请求），要写入的位置为内存（Memory）中的一个**数据****Buffer**。在数据流通过Posted Buffer时可能会产生一些时间延迟。

\2.    Consumer（消费者）会周期性的通过发起MRd事务（Memory Read，Non-Posted请求）来检查Flag信号量，以此来确定数据是否已经被Producer发出。

\3.    Flag信号量被Consumer设备读取，并以MRd Completion的形式返回给Consumer，用于告知Consumer数据还没有被Producer发送出（因为此时Flag=0）。

\4.    Producer通过发送一个MWr（Posted请求）来将Flag的值更新为1.

\5.    又一次地，Consumer发起步骤2中相同的事务（MRd，Non-Posted）来读取Flag的值。

\6.    这一次，Consumer读取到的Flag的值为1，这也就是说Consumer通过这次MRd Completion得知了所有的数据都已经被Producer发往内存（Memory）了。

\7.    然后，Consumer发起一个MWr事务（Posted请求）来将Flag信号量清除为0。

 

图 8‑3继续展示了示例的第二部分流程。

\8.    Producer（生产者）现在有更多的数据需要发送，它通过发起MRd事务（Non-Posted请求）的方式周期性的检查Status信号量。

\9.    Status信号量被Producer设备读取，并以MRd Completion的形式返回给Producer，用于告知Consumer还没有读取内存中的数据Buffer并且也没有更新Status信号量，因为Status=0。

\10.  对于Consumer，它现在已经知道了内存Buffer中有可用数据，所以它会发起一个或多个MRd（Non-Posted请求）来从Buffer中获取这些数据。

\11.  内存Buffer的内容被读取并返回给Consumer。

\12.  在完成了内存到Consumer的数据传输后，Consumer发起一个MWr（Posted请求）将Status信号量置为1。

\13.  又一次地，Producer通过发起MRd（Non-Posted请求）来检查Status信号量。

\14.  Producer设备读取Status，而此时Status=1。MRd Completion返回给Producer，从而Producer也就知道了内存Buffer中的数据已经被Consumer读出，那么Producer就可以继续向内存Buffer发送数据了。

\15.  Producer通过发起MWr来将Status信号量清除为0。

\16.  Producer重复以上按顺序从步骤1开始的一系列事件。

 

 

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image378.jpg)

图 8‑2生产者/消费者流程示例——第一部分

 

 

 

 

 

​                

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image380.jpg)

图 8‑3生产者/消费者流程示例——第二部分

 

#### 8.4.2     出错的生产者/消费者排序（Producer/Consumer Sequence-Errors）

前面的例子在没有讨论排序规则的情况下正确的执行了Producer/Consumer模型，但是它有可能会受到竞争情况的影响而导致Producer/Consumer模型执行流程失败。图 8‑4中展示了一个简单的流程用于演示在没有执行排序规则的情况下可能出现的多种问题中的一种。在进行下列讨论时请参照图 8‑4。

\1.    Producer向内存Buffer发起MWr事务（Posted请求）。我们假设要写入的数据被暂时地阻塞在了Switch上行端口的Posted流控Buffer中。

\2.    Producer发送一个MWr事务（Posted请求）来将Flag更新为1。

\3.    Consumer发起MRd请求（Non-Posted请求）来检查Flag是否被置为1。

\4.    Flag的值通过一个Completion返回给Consumer。

\5.    由于Flag=1，Consumer认为数据已经从Producer发送给内存Buffer了，因此它就发起MRd请求来获取内存Buffer中的数据。然而Consumer并不知道，由于Switch上行端口和RC之间链路的流控Credit不足，导致数据被暂时地阻塞在Switch上行端口的Posted流控Buffer中。结果，Consumer从返回的MRd Completion中所获取到的数据是内存Buffer中原有的旧数据而不是Producer此次发出的新数据。

 

这个问题可以通过拓扑结构中的Virtual PCI Bridge所支持的排序规则来避免。在本例中，当Consumer在执行步骤3、4的MRd事务时，Switch上行端口的Virtual PCI Bridge不能让含有Flag值的Completion超前于此前的Posted MWr的数据被转发（这个Completion和MWr事务都要经过Switch的上行端口转发）。这种情况就是表 8‑1中的D2a。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image382.jpg)

图 8‑4出错的生产者/消费者流程

### 8.5 宽松排序（Relaxed Ordering）

PCIe中支持PCI-X引入的宽松排序机制（Relaxed Ordering，RO）。RO机制允许Requester和Completer之间路径上的Switch对一些事务进行重新排序，以此来提高性能。

用来支持Producer/Consumer模型的排序规则可能会导致事务在某些情况下被阻塞，即使这些被阻塞的事务与Producer/Consumer模型的工作流程毫无关系。为了减少这种问题，可以将一个事务的RO属性位置为1，这表示软件确定这个事务与其他事务没有任何关联，允许它重新排序在其他事物之前。例如，如果一个Posted的写事务因为目的Buffer空间不足而被延迟阻塞，那么必须要等到这个写事务的阻塞问题解决并且写事务发送完成后，才能发送后续的任何事务。如果在后续到来的事务中，有一个事务是软件确定它与其他此前的事务没有关联，且它的RO bit也被置为1以此来表示这种无关联性，那么在Posted写请求解决阻塞问题之前就可以发送这个事务，而不用让它也等待写请求，且这样做并不会引发任何问题。

当设备驱动支持并启用RO bit（如图 8‑5所示，它在TLP Header中DW0的Byte2中的Bit5），设备才有可能会使用它。然后，当软件请求发送一个数据包时，这个请求包要根据软件的指示来使用RO这个属性位。当Switch或者RC发现一个数据包的RO bit是置为1的，那么它们就可以对这个数据包进行重排序，但是并没有强制要求它们一定要这样做。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image384.jpg)

图 8‑5宽松排序标志位在32bit Header中的位置

#### 8.5.1     RO对MWr和Message的影响（RO Effects on MWr and Message）

Switch和RC必须观察事务的RO属性位的值。MWr和Message都是Posted写事务，都由同一个Posted Buffer来接收，也都遵从同一个排序要求。当RO bit为1时，Switch处理这些事务的方式如下：

n 允许Switch对刚下发的MWr事务（RO bit=1）进行重新排序，使其可以排在先前下发的Posted MWr或是Message事务之前。类似地，刚下发的Message事务（RO bit=1）也可以可以重新排在先前下发的Posted MWr或是Message事务之前。Switch在转发事务时也必须保持RO bit不改变。PCI-X Bridge会忽略RO bit，它总是按照原顺序转发写事务（PCI-X中让写事务改变顺序并没有太大作用，如果一个写事务因为某些原因被阻塞了，那么即使更改了顺序，被提前的那一个写事务也还是会被阻塞）。另一个不同就是PCI-X中并没有定义Message事务。

n 允许RC对Posted写事务进行重新排序（PCI-X中这也会起到作用，因为RC可以向不同区域的内存进行写入，因此即使一个区域正忙RC也可以写入另一个不同的内存区域）。同样地，当RC接收到的写事务RO bit=1时，它可以按照任何地址顺序来写入每一个Byte。

#### 8.5.2     RO对MRd事务的影响（RO Effects on MRd Transactions）

PCIe中所有的读事务都采用拆分事务（Split Transaction）的方式来处理。当一个设备发出一个RO bit=1的MRd请求，Completer将会通过一系列的一个或多个Split Completion事务（拆分完成包）来返回Requester设备所请求的数据，且在完成包中使用和当初请求包相同的RO bit值。这种情况下Switch的工作行为如下：

\1.    当Switch接收到一个RO bit=1的MRd时，它会将MRd请求按照接收时的顺序来进行转发，一定不能让其重新排在先前下发的MWr事务之前。这保证了所有和读请求同方向传输的写事务都会被推到读请求之前。这是之前的Producer/Consumer例子的一部分，软件可能要依赖这个类似刷新的举动来进行正确的操作。Switch一定不能更改RO bit的值。

\2.    当Completer接收到MRd请求，它将收集被请求的数据，然后以一个或多个Completion完成包的形式进行发送，这些Completion都具有跟请求包中相同的RO属性。

\3.    Switch接收到RO bit=1的Completion完成包时，可以将它们进行重新排序，将这些Completion排在先前下发的与Completion同方向传输的MWr之前。如果写事务被阻塞了（例如被流控阻塞），那么Completion就可以被排在写事务之前发送，不用等待写事务阻塞结束。这种情况下的宽松排序（RO）keyi 改进读操作性能。中总结了Switch允许的宽松排序行为。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image386.jpg)

表 8‑2宽松排序中可以被重新排序的事务

### 8.6 弱排序（Weak Ordering）

当严格执行强排序规则时可能会出现暂时的事务阻塞问题。在不违反Producer/Consumer编程模型的前提下可以对强排序模型进行一定的修改，使其可以消除一些阻塞事务的情况，以此来提升链路效率。也就是说，实现弱排序模型（Weakly-Ordered model）是可以缓解强排序模型中阻塞事务的问题的。

#### 8.6.1     事务的排序与流控（Transaction Ordering and Flow Control）

之所以要将给定数量的VC Buffer拆分成流控子Buffer（Posted、Non-Posted和Cpl），是因为将TLP被解析或归入各自的Buffer后可以简化对事务排序规则的处理过程。事务排序规则处理逻辑之后就只需要在这三个子Buffer之间或是每个子Buffer去应用排序规则即可。

因为TLP被归入各自类别的三个子Buffer来应用事务排序规则，那么就必须要定义链路两端端口之间每种VC子Buffer（P、NP、Cpl）的流控机制。实际上，你可能会回想起来对于每种VC子Buffer大类（P、NP、Cpl）来说，它们各自的更第一层级的Header（Hdr）和Data（D）这两种子Buffer之间拥有一个独立的流控机制。

#### 8.6.2     事务暂停（Transaction Stalls）

强排序规则可能会出现由于单个接收Buffer满而阻塞所有事务的情况。例如，Producer/Consumer模型相关事务的排序要求不能变，但是与模型不相关的事务的排序规则是可以改变的，若都完全使用强排序规则那么就有可能出现阻塞其他不相关事务的情况。为了改善性能，我们可以考虑一种弱排序方案，这是一种对事务排序施加最低要求的方案。

下面的例子中（图 8‑6）展示了在一个VC中单向传输的事务相关的发送和接收Buffer。回忆一下，在同一VC中每种事务类型（P、NP、Cpl）都有独立的流控机制。发送Buffer内的数字表示这些事务被分配的发送顺序，并且此时Non-Posted接收Buffer已经满了。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image388.jpg)

图 8‑6强排序规则导致的事务临时暂停

考虑下面的流程：

\1.    事务1（MRd）是下一个要被发送的事务，但是此时并没有足够的Non-Posted流控Credit，因此它必须要等待。

\2.    事务2（Posted MWr）是紧跟事务1之后的待发送事务。如果执行的是强排序规则，那么这个MWr一定不能重排在事务1之前。

\3.    这种强排序的约束也会应用在后面的所有事务上，所以就会导致他们都暂停了下来，要等待事务1传输完成。

#### 8.6.3     VC Buffer提供了一个优点（VC Buffers Offer an Advantage）

事务的顺序是在VC Buffer内进行管理的。这些Buffer被按照事务类型Posted、Non-Posted和Completion进行分类，对于每个类型的Buffer来说，它的流控是与其他类型相独立的。这使得弱排序规则更加有用，比如在我们上面的例子中，即使一个Buffer满了，但是其他的Buffer还有空间，那么就可以在规则内让其他事务先传输。

### 8.7 基于ID的排序（ID Based Ordering，IDO）

另一种优化排序并提升性能的方法与流的性质有关。来自不同Requester的数据包通常并没有依赖关系，毕竟一个设备很难根据传输顺序就知道另一个设备在什么时候完成了某些步骤，因为它们可能有各自不同的路径来去往它们共享的资源。考虑到这一点，PCIe的2.1版本协议规范中引入了一种被称为基于ID的排序规则（ID-Based Ordering）来提升性能。

#### 8.7.1     解决方案（The Solution）

如果数据包的来源并没有考虑事务顺序，那么性能就会受到影响，如图 8‑7所示。在图中，事务1从自己的事务源被传输到Switch的上行端口，但是在它想继续传输至RC时却被阻塞了，这是因为RC端口中与之对应的Buffer满了（可以通过流控Buffer不足来表示这一点）。这里我们使用协议规范中的术语，来自同一个Requester的多个数据包称为一个TLP流（TLP Stream）。在本例中，事务1所示的传输路径可能会要传输TLP流中的一系列TLP。事务2对吼也到达了同一个出口端口（Switch的上行端口），并且它也被阻塞了，因为它需要保持与事务1的相对顺序。因为这两个数据包来自不同的事务源（属于不同的TLP流），因此这种延迟等待基本上是没必要的，很大概率这二者并没有相互依赖关系，但是在一般的排序规则中并没有进行这样的考虑。为了提高性能，我们需要一种不同的解决方法。

这种解决办法很简单：如果数据包各自的Requester ID不同（对于Completion来说是Completer ID），那么就可以将它们进行重新排序。这种可选能力使得软件可以让一个设备使用IDO，这样Switch端口就可以识别出一些数据包属于不同的TLP流。这是通过置位设备控制2寄存器（Device Control 2 Register）的使能位来实现的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image390.jpg)

图 8‑7不同数据源之间一般没有相互依赖关系

#### 8.7.2     何时使用IDO（When to use IDO）

软件可以让来自一个给定端口的请求或完成包应用IDO，这是通过设置Device Control 2 Register中相应的位来实现的。跟RO的情况一样，设备中并没有能力位（Capability bit）来告诉软件这个设备支持什么，而是只存在使能位，因此软件需要通过其他的一些方法来知道这个设备可以支持IDO（否则即使置位了使能位，但是设备并不支持IDO，那么也无法使用IDO）。这些bit使得这些类型的数据包可以使用IDO，但是软件还需要决定是否每个单独的数据包都要将它的IDO bit置位。TLP Header中的一个新的属性位可以用来指示这个TLP是否使用IDO，如图 8‑8所示。者带来了另一个相关的点：Completion一般会直接继承对应请求包中所有的属性位，但是对于IDO来说可能就不是这样了，因为Completer可以独立地启用IDO。换句话说，即使请求包中没有使用IDO，对应的Completion也是可以使用IDO的。

![img](file:///C:/Users/FANLI~1/AppData/Local/Temp/msohtmlclip1/01/clip_image392.jpg)

图 8‑8 64bit Header中的IDO属性位

### 8.8 死锁的避免（Deadlock Avoidance）

因为PCI总线采用延迟事务，也因为PCIe的MRd请求可能会因为缺少流控Credit而被阻塞，在这些原因下产生出了一系列的会出现死锁的场景。这些避免死锁的规则被包含在了PCIe的排序规则中，以此来确保不管拓扑结构如何都不会发生死锁。遵守排序规则可以防止由于出现了意料之外的拓扑结构而产生边界条件时出现的问题（例如，两个PCIe-to-PCI Bridge跨PCIe网络结构相连）。参考MindShare的名为《PCI System Architecture，Fourth Edition》的书籍，里面有对各种场景的详细解释，它们是PCIe中与避免死锁相关的排序规则的基础。表 8‑1中列出了用来避免死锁的排序规则，它们为A3、A4、D3、D4和A5b。注意，表中这五种为“Yes”的条目都是用来避免死锁的。如果表中列3或4相关的Non-Posted Buffer由于缺少流控Credit而出现了阻塞现象，那么行A相关联的Posted请求或是行D相关联的Completion必须要移动到Non-Posted请求之前，不难发现它们在表中的项都是“Yes”。还需要注意，A5b中的“Yes”项仅引用于PCIe-to-PCI/PCI-X Bridge中。

本质上来说，这些用来避免死锁的规则可以被总结为“必须要允许后到来的MWr请求或是Completion在顺序上超过先前到来的但被阻塞的Non-Posted请求，否则就有可能会发生死锁。”

 

 

 

 

 

 

 

 

 

 

 

 

 