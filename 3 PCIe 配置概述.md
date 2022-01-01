## Chapter 3      Configuration Overview //PCIe配置概述

### 关于前一章

前一章节对PCIe体系结构进行了全面介绍，我们将其看作是一种“执行层（executive level）”概述。它对协议中描述PCIe端口分层设计方法进行了介绍。在介绍事务协议时也一并介绍了各种数据包的种类。

### 关于本章

本章节将对如何对PCIe环境进行配置来展开介绍。内容包括了实现Function配置寄存器的空间、如何发现一个Function、如何产生并路由转发配置事务、 PCI兼容配置空间（PCI-compatible configuration space）与PCIe扩展配置空间的不同点（PCIe extended configuration space），以及软件是如何区分开EP和Bridge的。

### 关于下一章

下一章的内容将描述Function如何通过基址寄存器（Base Address Register，BARs）来请求访问memory或者IO地址空间，以及这种请求的目的与作用，并且将介绍软件是如何初始化这两种地址空间的。另外在下一章还会描述Bridge的Base/Limit寄存器（基/边界寄存器）是如何被初始化的，因为只有当Base/Limit寄存器初始化后Switch才能在PCIe网络中路由转发TLP。

### 3.1 总线/设备/功能/的定义（Definition of Bus,Device and Function）

正如PCI一样，每个PCIe功能（Function）的标识在其所在的设备内，以及这个设备所连接的总线内，都是唯一的。其标识符一般被称为“BDF”。对于任意一个 PCIe 拓扑结构，配置软件负责检测出拓扑中的每个Bus、Device和Function，缩写为BDF。接下来的几节将会结合一个PCIe拓扑的示例，来讨论BDF的主要特征。图 3‑1展示了一个PCIe拓扑结构，图中着重标识了示例系统中的Buses、Devices和Functions。本章后续内容将解释总线编号和设备编号分配的过程。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image114.jpg)

图 3‑1示例系统

### 3.2 PCIe总线（PCIe Buses）

软件总共可以分配256个总线编号。第一个总线号，Bus 0，通常由硬件分配给RC（Root Complex）。Bus 0由一个集成有EP的虚拟PCI总线，一到多个虚拟PCI-to-PCI Bridges（P2P）组成。其中的P2P Bridges拥有不可更改、硬件编码（hard-coded）的设备号和功能号。每个P2P Bridge都会产生一个新的总线，其他PCIe设备可以连接在到这些新产生的总线上去。每个总线都必须被分配一个唯一的总线号。配置软件分配总线号的过程中，首先从Bus 0/Device 0/Function 0开始搜索其他的Bridges。当找到一个Bridge之后，软件就给这个Bridge产生的新总线分配一个与上一级总线的总线号不同的、数字更大的编号。一旦新总线被分配了一个总线号之后，软件就会从新总线继续搜索更新的Bridges，而不是在上一级总线上继续搜索。这被称为“深度优先搜索（depth first search）”，关于这种搜索的细节内容，请参阅“Enumeration – Discovering the Topology”一节。

### 3.3 PCIe设备（PCIe Devices）

PCIe允许在单个PCI总线上最多挂载32个设备，然而PCIe点对点（point-to-point）的性质意味着只有一个设备可以直接连接在PCIe链路上，也就是Device 0。但是，通过RC和Switches包含的虚拟PCI总线。更多的设备可以“连接”到总线上。

每个设备都必须实现Function 0，其最多可以有8个功能（Function）。当一个设备拥有2个或以上的Function时，称之为多功能设备（Multi-Function Device）。

### 3.4 PCIe功能（PCIe Functions）

正如此前所讨论的一样，Function被设计为每个设备中之内的一个逻辑层次。这些Function可能包含硬盘驱动接口、显示控制器、以太网控制器、USB控制器等等。多Function的设备不需要依次按照编号逐个实现 Function。例如，一个设备可以只实现Function 0、2、7。因此，当配置软件检测到了一个多Function设备时，必须检查所有可能的Function，以了解当前Device存在哪些Function。每个Function都有它们自己的配置地址空间，这个配置地址空间用于设置与Function相关的资源。

### 3.5 配置地址空间（Configuration Address Space）

初期的PC需要用户设置开关和跳线来给每个安装上去的板卡分配资源，这样的方法经常会导致memory、IO和中断的设置出现冲突。这之后出现的两种IO架构——扩展ISA（EISA，Extended ISA）和IBM PS2系统，是初次实现了即插即用（plug and play）的架构。在这些架构中，配置文件随每个插件卡一起被提供，允许系统软件分配基本资源。PCI扩展了这种配置特性，它通过实现标准化的配置寄存器，允许通用的Shrink-Wrapped（压缩包装）操作系统管理几乎所有的系统资源。PCI 在拥有了一个标准化的方式来进行错误报告的开启与关闭、传送中断、进行地址映射以及更多其他的操作之后，就可以通过配置软件这一单一模块，来进行系统资源的分配和配置，这消除了几乎所有的资源冲突。

PCI为每个Function都定义了一个专用的配置地址空间块。映射在这个配置空间中的寄存器们使得软件可以发现这个Function的存在，并对这个Function进行一般操作和状态检查。大多数需要标准化的基本功能都存在于配置寄存器块的Header中，但是PCI架构师意识到若将可选功能（option feature，区别于前面的基本功能）也标准化可以带来很多好处，这些可选功能标准化后的结构被称为“能力结构”，（capability structures，例如电源管理、热插拔等等）。

对于每个Function，都含有256byte的PCI兼容配置空间（PCI-Compatible configuration space）。

#### 3.5.1     PCI兼容空间（PCI-Compatible Space）

在阅读下面的讨论内容时，请同时参阅图 3‑2。之所以将这256Byte命名为PCI-compatible configuration space（PCI兼容配置空间），是因为这些配置空间原本就是为PCI所设计的。这个配置空间的前16DW（64bytes）是配置头部（header），有两种类型的Header，分别为Type 0和Type 1。Type 0 Header对于每个Function都是必须含有的，除了Bridge，对于Bridge Function来说它使用的是Type 1 Header。剩余的48DW是一些可选寄存器，包括PCI能力结构（capability structure）。对于PCIe Functions而言，一部分 capability structure也是必须的。例如PCIe Function就必须实现如下的能力结构：


- PCI Express能力（PCI Express Capability）

- 电源管理

- MSI、MSI-X

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image116.jpg)

图 3‑2 PCI兼容配置寄存器空间

#### 3.5.2     扩展配置空间（Extended Configuration Space）

在阅读下面的讨论内容时，请同时参阅图 3‑3。当引入PCIe之后，最初始的256byte配置空间已经不足以放下所有新需要的Capability Structure了。因此配置空间的大小从原先的每个Function 256Byte扩展至了每个Function 4KByte。新增加出来的960DW扩展配置空间只能通过增强配置机制（Enhanced configuration mechanism）来进行访问，因为传统的PCI软件无法发现这个区域并进行访问，所以这部分区域对于 PCI 是不可见的。在扩展配置空间内包含了新增加的PCIe可选扩展能力寄存器（Extended Capability register），图 3‑3罗列出了一部分扩展能力寄存器。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image118.jpg)

图 3‑3每个PCIe Function所拥有的4KB配置空间

### 3.6 Host-to-PCI Bridge配置寄存器

#### 3.6.1     整体说明（General）

对Host-to-Bridge配置寄存器的访问不必使用前面的章节所提到的配置机制，在内存地址空间中，它通常映射为设备特定寄存器，这对平台固件是已知的。然而，它的配置寄存器格式排布和用法都必须遵从PCI 2.3协议规范中所定义的Type 0模板。

#### 3.6.2     只有RC发送配置请求（Only the Root Sends Configuration Request）

在协议规范中声明了，只有RC可以发起配置请求。RC作为 CPU 与 PCIe 拓扑的联络员，其传入 CPU 的 PCIe 请求包并在 PCIe 事务完成后向处理器报告。之所以限制 CPU 只能通过RC发起配置事务，是因为要是其他设备也有这种能力，那么他们可以任意改变配置内容，这样就会带来混乱。

由于只有RC能发起这些配置请求，所以这些配置请求只能在拓扑中向下转发，意味着Peer-to-Peer的配置请求是被无法实现的。请求包使用目的设备ID作为路由信息进行路由转发，这个目的设备ID就是它的BDF（拓扑中的Bus编号，Bus上的Device编号，Device中的Function编号）。

### 3.7 生成配置事务（Generating Configuration Transactions）

处理器一般无法直接进行配置读写请求，因为他们只能产生memory请求和IO请求。这意味着RC需要将 CPU 的其他类型访问转换成配置请求，这样才能进行配置的操作过程。配置空间可以通过以下两种机制进行访问：

- 传统的PCI配置机制，使用IO间接访问（IO-indirect access）

- 增强型配置机制，使用内存映射访问（memory-mapped access）

#### 3.7.1     传统PCI机制（Legacy PCI Mechanism）

在PCI协议规范中定义了IO-indirect方法，用来指示系统（RC或其等效的组件）进行PCI配置访问。在当时的历史背景下，占主导地位的PC处理器（Intel x86）被设计为仅能寻址64KB的IO地址空间。在 PCI 协议产生的时候，这个有限的IO空间已经变得非常混乱，只有少数几个可用的地址范围：0800h-08FFh，和0C00h-0CFFh。因此，将所有可能的Function的配置寄存器都映射到IO空间是不可行的。与此同时，内存地址空间也十分有限，将这些配置空间都映射进内存地址空间也不是个好方法。于是，协议规范的作者使用了一种常用的方法来解决这个问题，使用间接地址映射（indirect address mapping）。为此，需要使用一个寄存器保存目标地址，同时用第二个寄存器保存来自或是发往目标的数据。一次对目标Function的一次读写事务，需要先将待访问的地址写入地址寄存器，随后再读写数据寄存器。这很好的解决了地址空间有限的问题，但是这意味着产生一次配置访问需要两次IO访问。

PCI兼容机制使用RC的Host Bridge中的两个32bit的IO端口。它们分别是**配置地址端口**（Configuration Address Port），位于IO地址区域0CF8h-0CFBh，和**配置数据端口**（Configuration Data Port），位于IO地址区域0CFCh-0CFFh。

要访问一个Function的PCI兼容配置寄存器，首先要将目标的Bus、Device、Function和DW号写入配置地址端口，并将其使能bit置为有效。然后第二步，一个1或2或4Byte的IO读或写将会发送到配置数据端口。RC的Host Bridge将对给定的目标总线和在Bridge下现存的总线范围进行比较。若目标总线在这个范围内，这个Bridge则会发起配置读或写请求（这取决于对配置数据端口的IO访问是读操作还是写操作）。

##### 3.7.1.1   配置地址端口（Configuration Address Port）

配置地址端口仅在处理器对其完成一个完整的32bit写操作时，锁存住写入的信息，如图 3‑4，若对这个端口进行读操作则会返回它的这些内容。写入配置地址空间的信息必须遵照下面所描述的格式（图 3‑4）。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image120.jpg)

图 3‑4地址位于0CF8h的配置地址端口

- Bits[1:0]固定为0不变，且只读的，在读取时只能返回**0**。它的位置是DW对齐的，不允许指定字节（byte-specific）偏移量。

- Bits[7:2]用于标识目标Function的PCI兼容配置空间内的**目标DW**（target dword，其也被称作寄存器号），也就是用来标识Function内的寄存器号，要求这个寄存器必须位于兼容配置空间中。这种机制仅限于兼容 PCI 的配置空间中使用。（例如一个Function配置空间的前64DW）。

- Bits[10:8]用于标识目标Device内的目标Function号(0-7)。

- Bits[15:11]用于标识目标Device号(0-31)。

- Bits[23:16]用于标识目标Bus号(0-255)。

- Bits[30:24]为保留字段，必须为0。

- Bit[31]为使能位，若要将随后的对配置数据端口的IO访问转换为配置访问则必须要将该bit置为1。当该bit为0时，若有IO读或者IO写被发送到配置数据端口，那么这些事务都会被当成普通的IO请求来处理。

##### 3.7.1.2   总线比较和数据端口的使用（Bus Compare and Data Port Usage）

如图 3‑5，RC内的Host Bridge实现了一个次级总线号（Secondary Bus Number）寄存器和一个从属总线号（Subordinate Bus Number）寄存器。次级总线号（图 3-5 中的 Sec）指的是当前Bridge下直接连接的总线的编号，例如图中RC的Host/PCI Bridge产生了Bus 0，因此Host/PCI Bridge的次级总线号就是0。从属总线号（图 3-5 中的 Sub）指的是Bridge之下的可作为目标总线的总线号，例如图中Device 1的Sub=9，那么就说明在Device 1下游最大的总线号是 Bus 9，而它的Sec=5则说明其下游的总线号编号从 Bus 5 开始，简单来说就是Sec和Sub共同指定出了这个设备下可访问总线的范围（对于Device 1来说就是5-9）。

在一个单RC的系统中，Host-Bridge的次级总线号应该被固定为0，也就是它的可读可写的次级总线号寄存器从一复位就被强制置为0，或者说，Host-Bridge知道它访问到的第一个总线一定是Bus 0。若配置地址端口（Configuration Address Port，见图 3‑4）的bit 31被置为1，那么Bridge就会将目标总线号与Bridge之下的从属总线范围进行比较，以此来检查这个目标总线是否从属于当前Bridge之下。

当Bridge收到一个请求，它将会评估目标总线号是否在其下的从属总线号范围内，这个范围大于等于从次级总线号（Sec），小于等于从属总线号（Sub）。若目标总线号与当前的次级总线号相匹配，那么说明当前的次级总线就是目标总线，这个请求就会被作为一个**Type 0配置请求**来传输。当设备们收到一个Type 0请求，它们就知道当前一级本地总线上的某个设备就是这个请求的目标设备（而不是在更下一级的总线所属的设备）。

若目标总线号要大于Bridge的次级总线号（Sec），但是小于或者等于Bridge的从属总线号（Sub），那么这个请求将会作为**Type 1配置请求**被转发到Bridge的次级总线上。对于一个Type 1配置请求，可以这样理解它：尽管这个请求需要经过这条总线，但是它的目标设备并不在这一级总线上，相反地，这个请求将会由当前总线上的Bridge们向下转发到各自的下一级总线上，当然转发该Type 1配置请求的Bridge必须是从属总线范围包含了目标总线的。所有，Type 1配置请求只对Bridge设备有作用。更多关于Type 0和Type 1配置请求的信息，请参阅“Configuration Request”一节。

##### 3.7.1.3   单Host系统（Single Host System）

RC中的Host/PCI Bridge会将写入配置地址端口（Configuration Address Port）的信息锁存起来，如图 3‑1。若bit 31被置为1且目标总线在当前Bridge下方从属总线范围内，那么Bridge将把接下来的处理器对配置数据端口（Configuration Data Port）的访问转换成针对Bus 0的配置请求。处理器将会向配置数据端口（0CFCh）发起一个IO读请求或者一个IO写请求。这促使Bridge生成一个配置请求，这个配置请求是读请求还是写请求取决于IO访问是读还是写。若目标总线为Bus 0，那么它将是一个Type 0配置请求。若目标总线是从属总线范围内的其他总线，那么它将是一个Type 1配置请求。若目标总线不在从属总线范围内，那么这个Bridge将不会对这个请求进行转发操作。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image122.jpg)

图 3‑5 单Root系统

  

##### 3.7.1.4   多Host系统（Multi-Host System）

若在一个系统中存在多个RC，如图 3‑6所示，那么配置地址端口和配置数据端口将被这两个RC各自的Host/PCI Bridge复用，且两种配置端口各自的IO地址在两个Host/PCI Bridge中相同，也就是两个RC使用相同的一个IO地址来访问各自配置地址寄存器，同理，访问配置数据寄存器也是。为了防止争用，在两个Host/PCI Bridge中同时只能有一个响应处理器对配置端口的访问。

1. 当处理器发起对配置地址端口的IO写操作时，通过配置两个Host Bridge（ the host bridges are configured, //TODO），仅有一个Host Bridge会参与到此次事务中来。

2. 在枚举过程中（Enumeration），软件将搜索所有活跃（active）Bridge之下的所有总线，并对这些总线进行编号。当这一总线搜索过程完成后（不是枚举过程完成），软件将使能那个不活跃的Host Bridge，并给它赋予一个总线编号，这个总线编号会在之前那些活跃Bridge的总线编号范围之外，然后软件继续进行枚举过程。这两个Host Bridge都会看见请求，但是由于它们二者拥有相互不重叠的总线编号范围，而它们各自又只会对自己的范围内的请求作出响应，因此并不会存在冲突的情况。

3. 在上面的过程之后，两个Host Bridge都会收到对各自配置地址端口的访问，而随后进行的的对配置数据端口的读访问或写访问，则仅会被包含目标总线的Host/PCI Bridge所接收。接收访问的Bridge将会响应处理器的事务，而另一个Bridge则会忽略这个事务。
   - 若目标总线就是次级总线，那么Bridge将把这个访问转换成Type 0配置访问。
   - 否则，这个访问将被转换成Type 1配置访问。

   > 编者注：上文中的两个 RC 是对应于图 3-6 的情况，更一般的情况中会存在 1-N 个 RC。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image124.jpg)

图 3‑6多Root系统

#### 3.7.2     增强型配置访问机制（Enhanced Configuration Access Mechanism）

##### 3.7.2.1   整体说明（General）

当协议制定者在选择PCI-X和之后的PCIe该如何访问配置空间时，有两个考虑。第一个，每个Function的256Byte空间限制了那些想要在这个空间内放一些专有信息的厂商，而且未来的协议制定者可能也需要更多的空间来放置更多的能力结构（capability structure）。为了解决这个问题，这个空间从每个Function 256Bytes扩展至4Kbytes。第二个要考虑的是，PCI协议应用的时代多处理器系统还很少。对于旧的模型来说，当系统中仅有一个CPU且其只有一个线程时，生成一次访问需要两步并不会有什么问题。但是对于使用多核多线程CPU的计算机来说，使用IO间接访问模型就会产生一些问题，因为没有机制能够阻止多线程同一时间访问配置空间。因此，在没有线程锁机制（lock semantics）时不再使用“2步”模型（IO间接访问）。在没有线程锁机制的情况下，当线程A往配置地址端口（CF8h）写入一个值后，在它继续对配置数据端口（CFCh）做相应的操作之前，并没有一个机制来防止线程B将这个值覆盖掉。

为了解决这个新问题，协议制定者决定采用一个不同于IO间接访问的方法。他们不再尝试去保留地址空间，而是通过将所有配置空间都映射到内存地址，以此来创造出一个单步（single-step）、不可中断（uninterruptable）的访问过程。由于一个针对特定地址范围的memory请求会在总线上产生一个配置请求，所以这种访问方式只需要一个命令序列即可。这种方式带来的开销考量（trade-off）在于地址大小。每个Function映射地址空间需要4KB，实现最大数量的 function 需要256MB的memory（内存）地址空间。由于现代体系结构一般支持36到48位的物理内存地址空间，所以相较于现在的内存地址空间的总大小，256MB的空间占用很少。

为了处理地址映射，每个Function的4KB配置空间都以一个4KB对齐的地址作为起始地址，且这些4KB配置空间所映射的地址都要位于那段为了配置访问而预留的256MB内存地址空间内。并且现在的地址中的bit还携带了识别信息（identifying information），用于表示哪一个Function是访问目标（见表 3‑1）。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image126.jpg)

表 3‑1增强型配置机制的内存映射地址范围

##### 3.7.2.2   一些规则（Some Rules）

如果一个访问跨了dword地址边界（跨越了相邻的两个内存DW的边界，即操作长度大于了一个DW，或者是操作长度等于一个DW但是起始地址没有位于DW对齐的地址），那么RC可以不支持它访问增强型配置内存空间。RC们也不需要支持某些总线锁定协议（bus locking protocol），一些处理器使用这些总线锁定协议来实现原子操作或是不可中断的一系列命令。软件在访问配置空间时，需要避免上述这两种情况，除非软件知道RC可以支持它们。

### 3.8 配置请求（Configuration Requests）

Bridge为了响应配置请求，将会产生两种类型的配置请求，分别为Type 0和Type 1。具体产生哪一种类型的配置请求，取决于目标总线号是否与当前Bridge的次级总线号（Secondary Bus Number）相匹配。下面将进行讲解。

#### 3.8.1     Type 0配置请求（Type 0 Configuration Request）

如果目标总线号与次级总线号相匹配，Bridge将产生一个Type 0配置读/写，并转发到它的次级总线上，并：

1. 总线上的设备们检查配置请求中的Device Number（设备号），看谁才是这个配置请求的目标设备。需要注意，外部链路（external Link）上的EP总是Device 0。

2. 被选中的设备检查Function Number，看看设备内的哪个Function被选中。

3. 被选中的Function使用Register Number字段域来选择它配置空间中的DW，这个DW就是目标DW，并使用First Dword Byte Enable字段域来选出这个DW中要被进行读写操作的字节们。

如图 3‑7中给出了Type 0配置写请求和读请求的Header格式。在读写两种情况下，Type字段都是00100，而Format（Fmt）字段则用于指示是读请求还是写请求。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image128.jpg)

图 3‑7 Type 0配置读请求和写请求的Header

#### 3.8.2     Type 1配置请求（Type 1 Configuration Request）

当Bridge得到的配置请求的目标总线号并不是自己的次级总线号，但是目标总线号又在从属总线（Subordinate Bus）的范围内，那么Bridge将向次级总线转发一个Type 1请求包。次级总线上的非Bridge的设备（EP）将会忽略Type 1请求，因为这个请求的目标总线并不是当前总线。但是次级总线上的Bridge看到Type 1请求则将会进行与上一级Bridge相同的比对操作，当目标总线在从属总线范围内时：

- 如果目标总线就是当前Bridge的次级总线，这个请求包将从Type 1被转换成Type 0，并转发到次级总线上。次级总线上的本地设备将会像此前描述的一样检查请求包的Header。

- 若目标总线号不是当前Bridge的次级总线，但是在从属总线范围内，请求包将会继续以Type 1请求的格式被转发到当前Bridge的次级总线上。

如图 3-8 展示了Type 1配置读请求和写请求的Header格式。在读写两种情况下，Type字段域都是00101，而Format（Fmt）字段则用来指示这是读还是写。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image130.jpg)

图 3‑8 Type 1配置读请求和写请求的Header

### 3.9 PCI兼容配置访问示例（Example PCI-Compatible Configuration Access）

请参阅图 3‑9。

> 译者：在此图中纠正了原书版本的一个错误（该错误为errata中的第1条），Bus 6的中间的Bridge的Sub应该为9而不是8，原图中为8。

为了更好说明传统的CF8h/CFCh机制来产生配置请求，请参考下面的x86汇编代码，这段代码使得RC执行一个2byte的读操作，操作目标为Bus 4，Device 0，Function 0，Register 0（这个Register 0就是厂商Vendor ID）。

 `mov  dx,0CF8h   ;将dx设置为配置地址端口的地址`

`mov  eax,8004000h ;enable bit=1，bus 4，dev 0，func 0，DW 0`

`out  dx,eax    ;IO写来设置地址端口`

`mov  dx,0CFCh   ;将dx设置为配置数据端口的地址`

`in   ax,dx    ;从配置数据端口读出2byte数据`

 代码中的out指令将产生一个IO写操作，从处理器写入配置地址端口，这个配置地址端口位于RC的Host Bridge（0CF8h），如图 3‑4。

2. Host Bridge将配置地址端口中的目标总线号（这里是4）与Bridge下方的从属总线范围（这里是0到10）进行比较。很明显目标总线号在从属总线范围内，因此Bridge就准备好了接下来产生配置请求的目的地。

3. 代码中的in指令将产生一个IO读事务，读事务产生自处理器并发往RC Host Bridge中的配置数据端口。这是一个对配置数据端口的前两个byte进行读取的读事务。

4. 由于目标总线号不是bus 0，所以Host/PCI Bridge对bus 0发起一个Type 1配置读请求。

5. bus 0上的所有设备都会锁存这个事务请求，并发现它是一个Type 1配置请求。因此RC中的两个虚拟PCI-to-PCI Bridges各自都将Type 1请求中的目标总线号与下方的从属总线范围进行比较。

6. 目的地bus 4位于左侧Bridge的下方从属总线范围中，所以左侧Bridge将把这个请求数据包发送到它的次级总线上，但是这个请求仍然是一个Type 1请求，因为目标总线并不是这个左侧Bridge的次级总线。

7. 图左侧的Switch的上方端口将接收到这个请求数据包，并将它传送给Switch中靠上方的PCI-to-PCI Bridge。

8. 这个Switch中靠上方的PCI-to-PCI Bridge确定目标总线在自己下方，但是并不是自己的次级总线，因此它将请求数据包继续以Type 1请求的格式转发到bus 2上。

9. bus 2上的两个Bridge都接收到了Type 1请求包。偏右侧的Bridge确定目标总线就是自己的次级总线。

10. bus 2上偏右侧的Bridge将这个配置读请求发送到bus 4上，而且这次会将原本为Type 1的请求转换成Type 0配置读请求来发送，这是因为这个请求的目标总线就是当前Bridge的次级总线。

11. bus 4上的Device 0接收到这个请求包，并对数据包中的目标Device号、Function号、Register号的字段进行译码，以此来选中配置空间中的目标DW。（如图 3‑3）

12. First Dword Byte Enable字段中的bit 0和bit 1都被置为有效，因此Function将在完成包中返回它的前两个字节（前两个字节也就是Vendor ID）。然后完成包使用从Type 0请求包中获得的Requester ID作为路由信息，通过路由转发，最终回到Host Bridge。

13. 读出来的2byte数据将会呈交给处理器，以此来完成in指令的执行。Vendor ID的值被放置在处理器的AX寄存器中。

### 3.10     增强型配置访问示例（Example Enhanced Configuration Access）

请参阅图 3‑9。

下面的x86汇编代码将使得RC执行一个2byte的读操作，操作目标为Bus 4，Device 0，Function 0，Register 0（这个Register 0就是厂商Vendor ID）。在这段代码工作之前，Host Bridge必须已经被分配了一个基地址（base address value）。在这个例子中，我们假设增强型配置内存映射范围（Enhanced Configuration memory-mapped range）的256MB对齐基址为E0000000h。

 `mov  ax,[E0400000h] ;内存映射配置的读操作` 

- 地址[63:28]表示整体增强型配置地址范围的256MB对齐的基地址的高36bit，在本例中，这个整体增强型配置地址范围为00000000-E0000000h。

- 地址[27:20]用于选择目标bus（本例中为bus 4）

- 地址[19:15]用于选择bus上的目标device（本例中为device 0）

- 地址[14:12]用于选择device内的目标function（本例中为function 0）

- 地址[11:2]用于选择function配置空间中的目标DW（本例中为DW 0）

- 地址[1:0]用于定义选中的DW中的起始byte的位置（本例中为byte 0）

 

处理器发起一个2byte的memory read，读取的起始地址为E0400000h，这个地址会被RC中的Host Bridge锁存下来。Host Bridge识别到这个地址位于用来进行配置的区域之内，它将产生一个配置读请求，读取的位置为Bus 4—Device 0—Function 0—dword 0—首两个字节。剩下的操作就跟前面的小节中所讲的一样了，也就是如何把读出来的信息通过完成包返回给Host。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image132.jpg)

图 3‑9配置读访问示例



 

### 3.11     枚举——搜索发现拓扑（Enumeration-Discovering the Topology）

在完成了系统上电或是复位之后，配置软件需要扫描PCIe网络结构，来搜索发现整个机器的拓扑，并学习这个网络结构是如何被填充的（例如里面都有多少总线、多少设备以及它们的编号等等）。在这进行之前，如图 3‑10所示，软件唯一知道的就是拓扑中有一个Host/PCI Bridge以及这个Bridge的次级总线Bus 0。需要注意，一个Bridge自身上方相连的总线称为主总线（Primary Bus），而这个Bridge自身下方相连的总线称为次级总线（Secondary Bus）。扫描PCIe结构来发现整体拓扑结构的过程被称为**枚举过程**。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image134.jpg)

图 3‑10刚启动时软件所认为的拓扑结构

#### 3.11.1    搜索某个Function是否存在（Discovering the Presence or Absence of a Function）

处理器上运行的配置软件发现一个Function的方式一般是读取这个Function的Vendor ID寄存器。PCI-SIG给每个厂商都分配了一个唯一的Vendor ID，每个厂商都会将自己设计的Function中的Vendor ID寄存器的值固定为自己的Vendor ID。通过读取整个系统中的Bus—Device—Function这三者所有组合中的Vendor ID寄存器，枚举软件可以搜索遍整个拓扑，并得知有哪些设备存在。这个过程相当的简单，但是可能会出现两个问题：目标设备可能不存在，或者它虽然存在但是没有准备好响应事务请求。下面将介绍如何处理这两种情况。

##### 3.11.1.1 设备不存在（Device not Present）

目标设备在系统中不存在的这个情况，可能会在枚举搜索过程中发生很多次。当它发生时，我们需要对它有正确的理解。在PCI中，配置读请求（Configuration Read Request）有可能在总线上执行超时（timeout），那么就会产生一个主设备放弃（Master Abort）的错误情况。由于没有设备来驱动总线，所有的信号都是拉高的，那么数据位在总线上就是全为1的，这个全为1的值将成为总线上被看见的数据值。这使得值为FFFFh的Vendor ID并没有被分配出去，而是作为一个保留值。如果枚举软件收到的读取Vendor ID的结果是FFFFh，那么它就知道它读取的设备不存在。由于这种情况其实并不是一种错误情况，因此在枚举过程中，Master Abort并不会被当做是一个错误来进行报告。

对于PCIe来说，针对一个不存在的设备的配置读请求将使得目标设备上方连接的Bridge返回一个不携带数据的完成包，这个完成包的状态字段将被置为UR（Unsupported Request，不被支持的请求）。为了向后兼容传统的枚举模型，若RC在枚举过程中收到这样的完成包，它将会给处理器返回数据FFFFh。注意，枚举软件是依赖于接收到一个返回值为全1（FFFFh）的配置读请求来判定目标设备是不存在的，而系统中的在读取这样一个不存在的设备Function的Vendor ID时，其实是返回了一个状态为Unsupported Request的不携带数据的完成包。

当出现设备不存在的情况时，也需要避免意外地发送错误报告，这很重要。尽管这种超时或者UR的结果在系统正常运行是确实是出错的情况，但是在枚举过程中它并不能被认为是错误，它是一种正常的可以预料到的结果。为了能更简单的避免这种混乱，设备们一般在这过程中都不启用错误信号，直到枚举完成才启用。对于PCIe来说，记录这种事件（目标设备不存在）仍然是有作用的，这也就是为什么PCIe能力寄存器块（PCIe Capability register block）中存在第4个“错误”状态位，被称为Unsupported Request Status，不受支持的请求状态（更多关于这部分的内容请见“Enabling/Disabling Error Reporting”一节）。由于有这个状态位的存在，发生目标设备不存在的情况就可以被记录下来，而且不会被当做是一个错误，这一点非常重要，因为若检测到一个错误那么枚举过程将被停止并调用系统错误处理程序。在枚举还没进行完的这个时间段，错误处理软件的能力可能十分有限，使得问题无法被解决。这将造成枚举软件运行失败，因为通常是在操作系统（OS）或者其他错误处理软件可用之前就已经执行枚举软件。为了避免这种风险，在枚举期间，通常不应该报告错误。

##### 3.11.1.2 设备未准备好（Device not Ready）

前面提到的可能会发生的另一个问题就是目标设备虽然存在，但是可能并未准备好响应配置访问。对于配置操作来说，需要考虑发起配置的时间点，因为设备准备好被访问是需要时间的。如果数据速率小于等于5.0GT/s，软件必须在复位后等待100ms再发起配置请求。如果数据速率高于5.0GT/s（Gen3速率），软件必须在链路训练（Link Training）完成100ms之后再尝试发起配置操作。之所以更高的速率需要更多的延时（Gen3需要在链路训练完成后等待100ms而不是复位完成等待100ms），这是因为Gen3的链路训练中的均衡过程（Equalization Process）可能需要比较长的时间（约50ms，关于这里的更多内容请参阅“Link Equalization Overview”一节）。

在PCI 2.3中定义了初始化时间（Initialization Time，Trhfa-从复位释放到第一个访问所经历的时间），初始化时间起始于RST#被置为无效，结束于225个PCI时钟周期之后。在这段长度为一秒的时间内，Function正在为响应第一次配置访问做准备，这个时间长度也被PCIe协议所使用，协议规定初始化时间为1.0s（+50%/-0%）。一个Function可以利用这段时间来填充自己的配置寄存器，例如加载一个外挂的串行EEPROM的内容做为寄存器初始值。加载EEPROM内容需要花费一点时间，加载完成前Function都没有准备好响应配置请求。

在PCI中，若在Function准备好之前就收到了一个配置访问，那么它有三个选择：忽略这个请求、重试（Retry）这个请求、接收这个请求但是延期响应直到Function自身完全准备好为止。最后一种选择的响应可能会给热插拔（Hot-plug）系统带来麻烦，因为延期响应的时长可能会达到1s，在这1s中总线都是停止的，直到这个请求被执行完，总线才重新工作。

在PCIe中我们也面临着相同的问题，但是问题的过程有一点不同。首先，PCIe Function在临时无法响应配置访问时必须要给出一个完成包，这个完成包也要被置为指定的状态，这个状态为Configuration Request Retry Status（CRS，配置请求重试状态）。这个状态只有在响应配置请求的时候才是合法的，如果其他的请求收到了这个状态的完成包，则可能会认为数据包格式错误（Malformed Packet error）。这种状态的响应也只在复位后的1秒后有效，因为在1秒之后Function就被认为是可以正常响应配置请求了，如果1秒之后这个Function都还无法正常响应配置请求，那么就认为它已经损坏了。

除了系统复位之后的那一段时间之外，RC处理配置读请求的CRS（配置重试状态）完成包的方式不是通用的。在系统复位后的那一小段时间里，RC有两个可以进行的操作供选择，选择哪一种则取决于Root控制寄存器（Root Control Register）中CRS Software Visibility这一bit的值（如图 3‑11）。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image136.jpg)

图 3‑11 PCIe能力块中的Root控制寄存器

- 如果这个bit被置为1，而且发出的请求是配置读请求，要读取Vendor ID寄存器的全部两个byte（枚举过程通过这样的操作来搜索发现一个Function是否存在），那么在接收到CRS完成包时RC需要给Host返回伪造的0001h作为读取的寄存器值，并将其他的所有字节都置为全1。这个Vendor ID并没有被任何真实的设备所使用，软件将把这个0001h的Vendor ID理解为访问这个设备可能需要很长的延迟。这个信息很有用，因为软件就可以选择去执行其他的任务，更充分的利用这段等待设备响应的时间，稍后再返回来查询这个设备。要进行这样子的操作，软件必须确保其在复位后对Function的第一次访问就是读取2 byteVendor ID的配置读访问。

- 对于配置写访问或者是其他的配置读访问（并不是读2byte Vendor ID），RC必须自动地以一个新请求的形式对之前的配置请求进行重新下达。

#### 3.11.2    确认某个Function是EP还是Bridge

枚举过程的一个关键部分就是可以确定一个Function是Bridge还是EP。如图 3‑12所示，Header类型寄存器（Header Type Register，位于配置空间Header的偏移地址0Eh）的低7bit用于标识这个Function的基本种类，总共定义了三种不同的值：

- 0 = 不是一个Bridge（那也就是一个PCIe EP）

- 1 = PCI-to-PCI Bridge（缩写为P2P），用于连接两条总线

- 2 = 插件卡Bridge（CardBus Bridge，现在很少使用的历史遗留接口）

在图 3‑1中，每个虚拟P2P的Header Type字段（DW3，byte2）的返回值都为1，且PCI Express-to-PCI Bridge（Bus 8，Device 0）的Header Type字段也将返回1。但是对于EP来说，它返回的Header Type字段是0。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image138.jpg)

图 3‑12 Header类型寄存器

### 3.12     单RC枚举示例（Single Root Enumeration Example）

现在，让我们通过一个例子来讨论一下枚举过程中的基本要素。如图 3‑13展示了一个经过总线、设备枚举后的系统示例。接下来的讨论基于这样的假设：配置软件将使用本章节定义的两种配置访问机制中的一种来完成枚举过程。在系统启动时，处理器上执行的配置软件将如接下来所讲述的那样来进行枚举过程。

1. 软件将Host/PCI Bridge的次级总线号（Secondary Bus Number）更新为0，并将从属总线号（Subordinate Bus Number）更新为255。之所以将其设置为最大值是因为Sub Num可以这样一直保持不变，直到Host/PCI Bridge下方的所有总线都被识别出来，然后再更新成准确的值。在此时此刻，暂且认为在Host/PCI Bridge下方识别到的总线范围为Bus 0到255。

2. 从Device 0开始，枚举软件将会尝试去读取Bus 0上的32个可能存在的Device，读取的内容为它们各自的Function中的Vendor ID。如果Bus 0—Device 0—Function 0返回了一个有效的Vendor ID，那么就认为这个设备存在并至少含有一个Function。若Bus 0—Device 0—Function 0没有返回一个有效的Vendor ID，则继续去探测Bus 0—Device 1—Function 0。

3. 在本例中的值为1的Header Type字段（如图 3‑12）表示这是一个PCI-to-PCI Bridge。Header Type寄存器中的Multifunction位（bit 7）为0，这表示Function 0就是这个Bridge中唯一的Function。协议规范其实并没有阻止在Bridge这样的设备中去实现多个Function，相反地，当有多个Function时，每个Function都可以作为一个虚拟PCI-to-PCI Bridge，甚至还可以是一个非Bridge的Function。

4. 现在软件已经找到了一个Bridge，并执行了一系列配置写操作来将这个Bridge的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=0

- 次级总线号（Secondary Bus Number）=1

- 从属总线号（Subordinate Bus Number）=255

现在，这个Bridge就认为在它下方直接相连的总线的编号为1（次级总线号=1），它下方从属的最大总线号是255（从属总线号=255）。

5. 枚举软件必须进行深度优先（depth-first）的搜索。在继续发现Bus 0上的其他Device/Function之前，枚举软件必须先搜索Bus 1。

6. 软件读取Bus 1—Device 0—Function 0的Vendor ID，这个目标设备在我们的示例图 3‑13中为Bridge C。这次读取将返回一个有效的Vendor ID，这表示在Bus 1上存在Device 0—Function 0。

7. 在Header寄存器中的Header Type字段的值为1（0000001b），这表示它是又一个PCI-to-PCI Bridge。像之前一样，bit 7的值为0（Multifunction bit=0），表示这个Bridge C是一个单Function设备。

8. 软件现在执行一系列的配置写操作来将这个Bridge C的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=1

- 次级总线号（Secondary Bus Number）=2

- 从属总线号（Subordinate Bus Number）=255

9. 软件继续进行深度优先的搜索（depth-first search），读取Bus 2—Device 0—Function 0的Vendor ID。在本例中（图 3‑13），Bus 2上的Device 0—Function 0是Bridge D。

10. 读请求返回了一个有效的Vendor ID，表示Bus 2—Device 0—Function 0存在。

11. 在Header寄存器中的Header Type字段的值为1（0000001b），这表示它是又一个PCI-to-PCI Bridge，且bit 7的值为0（Multifunction bit=0），表示这个Bridge D是一个单Function设备。

12. 软件现在执行一系列的配置写操作来将这个Bridge D的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=2

- 次级总线号（Secondary Bus Number）=3

- 从属总线号（Subordinate Bus Number）=255

13. 继续进行深度优先的搜索（depth-first search），读取Bus 3—Device 0—Function 0的Vendor ID。在本例中（图 3‑13），Bus 2上的Device 0—Function 0是Bridge D。

14. 返回了一个有效的Vendor ID，表示Bus 3—Device 0—Function 0存在。

15. 这次有了不同，在Header寄存器中的Header Type字段的值为0（0000000b），这表示它是一个Endpoint Function（EP）。由于这个Function是一个EP而不是一个Bridge，它有一个Type 0 Header，且在这个EP之下就没有别的PCI兼容总线了（PCI-Compatible Bus）。这个EP的bit 7的值为1（Multifunction bit=1），表示这个EP是一个多Function设备。

16. 枚举软件对Bus 3—Device 0上的全部可能的8个Function进行访问，访问它们各自的Vendor ID，然后软件确认除了Function 0之外只存在Function 1。Function 1也是一个EP（Type 0 Header），因此在这个设备（Bus 3—Device 0）之下并不存在一个附加的总线。

17. 枚举软件继续在Bus 3上进行扫描，寻找Device 1到Device 31上有效的Function，然而并没有找到任何其他的Function。

18. 枚举软件完成了寻找Bridge D之下所有Function的任务之后，就会更新Bridge D的一些信息，也就是将从属总线号从预留的255更新为真实值3。然后，枚举软件回到上一个总线层级（Bus 2），并继续在这个总线上扫描，寻找有效的Function。在本例中（图 3‑13），Bus 3上的Device 1—Function 0是Bridge E。

19. 配置读请求返回了一个有效的Vendor ID，表示Bus 2—Device 1—Function 0存在。

20. 在Header寄存器中的Header Type字段的值为1（0000001b），这表示它是又一个PCI-to-PCI Bridge，且bit 7的值为0（Multifunction bit=0），表示这个Bridge E是一个单Function设备。

21. 软件现在执行一系列的配置写操作来将这个Bridge E的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=2

- 次级总线号（Secondary Bus Number）=4

- 从属总线号（Subordinate Bus Number）=255

22. 软件继续进行深度优先的搜索（depth-first search），读取Bus 4—Device 0—Function 0的Vendor ID。

23. 返回了一个有效的Vendor ID，表示Bus 4—Device 0—Function 0存在。

24. 在Header寄存器中的Header Type字段的值为0（0000000b），这表示它是一个EP设备，且bit 7的值为0（Multifunction bit=0），表示这个设备是一个单Function设备。

25. 枚举软件继续在Bus 4上进行扫描，寻找Device 1到Device 31上有效的Function，然而并没有找到任何其他的Function。

26. 枚举软件已经到达了这个搜索分支的底部，因此它会更新当前总线之上相连的Bridge，在这里为Bridge E，将其从属总线号从预留的255更新为真实值4。然后，枚举软件回到上一个总线层级（Bus 2），并继续读取该总线上的下一个设备（Device 2）的Vendor ID。在本例中（图 3‑13），Bus 2上并没有实现Device 2到Device 31，因此枚举软件将不会发现Bus 2上还有其他的设备，仅有Device 0和Device 1。

27. 枚举软件更新Bus 2之上相连的Bridge，在这里为Bridge C，将其从属总线号从预留的255更新为真实值4，并回到上一个总线层级（Bus 1），继续去读取该总线上的下一个设备（Device 1）的Vendor ID。在本例中（图 3‑13），Bus 1上并没有实现Device 1到Device 31，因此枚举软件将不会发现Bus 1上还有其他的设备，仅有Device 0。

28. 枚举软件更新Bus 1之上相连的Bridge，在这里为Bridge A，将其从属总线号从预留的255更新为真实值4，并回到上一个总线层级（Bus 0），继续去读取该总线上的下一个设备（Device 1）的Vendor ID。在本例中（图 3‑13），Bus 0上的Device 1—Function 0是Bridge B。

29. 如前面所述的相同的过程，枚举软件发现Bridge B，并执行一系列的配置写操作来将这个Bridge B的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=0

- 次级总线号（Secondary Bus Number）=5

- 从属总线号（Subordinate Bus Number）=255

30. 然后枚举软件又在Bus 5上发现Bridge F，并执行一系列的配置写操作来将这个Bridge F的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=5

- 次级总线号（Secondary Bus Number）=6

- 从属总线号（Subordinate Bus Number）=255

31. 接着枚举软件又在Bus 6上发现Bridge G，并执行一系列的配置写操作来将这个Bridge G的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=6

- 次级总线号（Secondary Bus Number）=7

- 从属总线号（Subordinate Bus Number）=255

32. 当枚举软件继续在Bus 7上搜索，它发现了一个单Function的EP设备，Bus 7—Device 0—Function 0，因此将Bridge G的从属总线号更新为7。

33. 枚举软件向上回到Bus 6继续进行搜索，然后它发现了Bridge H，并执行一系列的配置写操作来将这个Bridge H的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=6

- 次级总线号（Secondary Bus Number）=8

- 从属总线号（Subordinate Bus Number）=255

34. 然后枚举软件又在Bus 8上发现Bridge J，并执行一系列的配置写操作来将这个Bridge J的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=8

- 次级总线号（Secondary Bus Number）=9

- 从属总线号（Subordinate Bus Number）=255

35. Bus 9上的所有设备以及与它们相关的Function都已经全部被搜索完成，它们都不是Bridge，因此下方也就没有附加的总线，所以将Bridge H和Bridge J的从属总线号更新为9。

36. 枚举软件向上回到Bus 6继续进行搜索，然后它发现了Bridge I，并执行一系列的配置写操作来将这个Bridge I的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=6

- 次级总线号（Secondary Bus Number）=10

- 从属总线号（Subordinate Bus Number）=255

37. 当枚举软件继续在Bus 10上搜索，它发现了一个单Function的EP设备，Bus 10—Device 0—Function 0。

38. 由于枚举软件已经到达了这个搜索分支的底部，这个底部是也是整个PCIe拓扑树状结构的底部，因此Bridge B、F、I的从属总线号会被更新为10，而Host/PCI Bridge也会将从属总线寄存器更新为这个数值。

 

每个Bridge中最终的主总线号、次级总线号和从属总线号可以参阅图 3‑9。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image140.jpg)

图 3‑13单Root系统

### 3.13     多RC枚举示例（Multi-Root Enumeration Example）

#### 3.13.1    整体说明（General）

考虑如图 3‑14所示的多RC系统。在这个系统中，每个RC：

- 实现了各自的配置地址端口（Configuration Address Port）和配置数据端口（Configuration Data Port），并且各个RC中的这些相应的端口的IO地址是相同的，比如RC 0和RC 1的配置地址端口的IO地址相同。

- 都实现了增强型配置机制。

- 都包含一个Host/PCI Bridge

- 实现了各自的次级总线号寄存器和从属总线号寄存器，但是不同RC的这两个寄存器对于配置软件来说是不同的操作地址。

在图示中，每个RC都是一个芯片组成员，这其中的一个RC被设计为主RC，它的Bridge下方相连的总线为Bus 0，它被称为主RC（Primary Root Complex）。而另一个RC的Bridge下方相连的总线则是被暂时认为是Bus 255，它被称为次级RC（Secondary Root Complex）。

#### 3.13.2    多RC枚举过程（Multi-Root Enumeration Process）

在对图 3‑14中左侧的树状结构进行枚举的过程中，次级RC的Host/PCI Bridge将会忽略所有的配置访问，因为所有的目标总线号中最大的也不会超过9。需要注意，虽然Bus 8被检测到了并且也被编了号，但是这条总线上并没有连接设备。一旦左侧树状结构的枚举过程完成，枚举软件就会进行如下的这些步骤来对次级RC进行枚举：

1. 在本例中（图 3‑14），枚举软件将会把次级RC中Host/PCI Bridge的次级总线号和从属总线号都更改为64。（64和128这两个值在多RC系统中被经常用来作为起始总线地址，但是这只是软件的一个习惯，在PCI或者PCIe的协议规范中并没有对这种配置方式有硬性规定。比如在本例中即使给次级RC的起始总线地址设置为10，也不会引起任何错误。）

2. 枚举软件开始在Bus 64上进行搜索，并发现了与RC下方端口相连的Bridge。

3. 然后枚举软件执行一系列的配置写操作来将这个Bridge的总线号寄存器设置为如下值：

- 主总线号寄存器（Primary Bus Number Register）=64

- 次级总线号（Secondary Bus Number）=65

- 从属总线号（Subordinate Bus Number）=255

现在，这个Bridge就认为在它下方与它直接相连的总线编号为65（次级总线号=65），并且它下方最大的总线号为255（从属总线号=255）。

> 译注：这里原书有误，原书写其下方的最大总线号为65，但是应该是255，只不过之后这个号会被更新为65

4. 枚举软件继续在Bus65上进行搜索，并发现了Device 0，且它仅有一个Function 0。进一步搜索的结果显示，在Bus 65上并没有其他的Device了，因此搜索过程返回到上一级总线。

5. 枚举软件回到Bus 64继续进行，然而也没有发现任何其他的设备，因此就将Host/PCI Bridge的从属总线号更新为65。

6. 这样就完成了次级RC的枚举过程。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image142.jpg)

图 3‑14多Root系统

#### 3.13.3    热插拔注意事项（Hot-Plug Considerations）

热插拔，指的是一个插件卡（add-in card）可以在系统运行时插入或拔出。在这种热插拔环境中，对于图 3‑14中的Bus 8来说，可能带来潜在的麻烦。若系统已经完成枚举，并已经正常的开始运行了，这时将一个含有Bridge的插件卡插到Bus 8上，则有可能会引发问题。这个插件卡内的Bridge分配给自己的次级总线号和从属总线号将会比自己的主总线上的其他从属总线的编号还要大，并且将完全包含这些总线号，意思就是说，Bus 8下方若出现了新总线，那么这个新总线可能要被分配为Bus 9，而这就与已经存在的Bus 9产生了冲突。这是因为新的总线号必须在插件卡上方的Bridge的从属总线范围内，对于本例（图 3‑14）来说也就是在6-9的范围内。

一种用来分配Bus 8下方这些新增的总线号的方法是，将当前的Bus 9的编号增大，为Bus 8下的新增总线号腾出编号的空间。尽管在系统运行时调整总线号是可以做到的，但是据有经验的人说，使用这种方法的效果不是很好。

还有一种简单的方法来解决这个潜在的问题：只需要在发现未填充的插件卡插槽时，简单地留出一段总线编号间隔（bus number gap）即可。例如，当Bus 8被分配了总线号8，但是在它下方发现了一个空的插件卡插槽，那么就给下一个被发现的总线一个更高的总线号，比如给它编号19而不是9，这样就为Bus 8之下以后可能的新增总线留出了编号空间。然后，如果一个带有Bridge的插件卡被插入，那么新增的总线就可以从Bus 9开始编号，而不会产生任何问题。在大多数情况下，留出这样的总线编号间隔并不会引发问题，因为系统总共可以分配256个总线号，编号资源较为充足。

### 3.14     MindShare Arbor：调试/验证/分析和学习的软件工具

#### 3.14.1    整体说明（General）

MindShare Arbor是一个计算机系统调试（Debug）、验证（Validation）、分析（Analysis）和学习的工具，它允许用户对任意的memory、IO或者配置空间的地址进行读取或者写入。读取这些地址返回回来的数据将以一个简洁明了且信息丰富的样式进行查看。

本书的作者决定不在某一个标志性的特定章节中对所有的配置寄存器都进行详细描述。相反地，各个寄存器将分散在全书中，在与它们各自相关的章节中进行详细讲述。

MindShare Arbor是一个很棒的参考学习工具，可以用它来代替书中没有的配置寄存器空间的专用描述章节。通过MindShare Arbor，我们可以快速的理解PCI、PCI-X以及PCI Express中实现的配置寄存器和结构。所有的寄存器和字段定义都是按照最新版本的PCI Express协议进行更新的。一些其他种类的结构（例如x86 MSRs、ACPI、USB、NVM Express）也可以在MindShare Arbor中进行查看（一些暂时没有的也会很快就可以支持查看）。

访问[www.mindshare.com/arbor](http://www.mindshare.com/arbor)即可免费下载MindShare Arbor的试用版。

![img](img/3%20PCIe%20%E9%85%8D%E7%BD%AE%E6%A6%82%E8%BF%B0/clip_image144.jpg)

图 3‑15 MindShare Arbor的部分截图

#### 3.14.2    MindShare Arbor功能列表

- 包含PCIe 3.0协议中的所有配置寄存器的描述内容。
- 可以扫描一个系统中所有PCI可见Function的配置空间，并以可读性极强的格式将每个寄存器的讲解都展示出来。
- 可以直接访问任何的memory或者IO地址。
- 可以写入任意的配置空间位置、memory地址或是IO地址。
- 可以在一个已经解码的格式中查看标准或非标准的结构体
  - 解码信息可以给标准的PCI、PCI-X、PCI Express结构体进行解码。

  - 解码信息可以给一些基于x86的结构体和设备特别的寄存器进行解码。

- 可以创建自己定义的基于XML的译码文件，来驱动Arbor的信息显示。

  - 可以创建译码文件，来对配置空间、内存地址空间和IO空间内的结构体进行译码。

- 可以保存系统扫描的结果，并之后再在其他系统上查看这个结果。

  - 保存的系统扫描结果是基于XML的开源格式的文件。

- 已经拥有或者即将拥有的新功能特性

  - 可以检查几次扫描结果之间的差异。

  - 可以为扫描中的违规或者非最佳的设置进行后期处理。

  - 可以支持脚本来达到自动化。

  - 可以对x86结构体进行译码（MSRs、paging、segmentation、interrupt tables等等）。

  - 可以对ACPI结构体进行译码。

  - 可以对USB结构体进行译码。

  - 可以对NVM Express结构体进行译码。

### 术语翻译
|英文|翻译|
|--|--|
|PCI-Compatible configuration space|PCI兼容配置空间|
|PCIe extended configuration space|PCIe扩展配置空间|
|Base Address Register|基址寄存器|
|bus|总线|
|device|设备|
|function|功能|
|capability structures|能力结构|
|Enhanced configuration mechanism|增强配置机制|
|IO-indirect access|IO间接访问|
|memory-mapped access|内存映射访问|
|Primary Bus Number|主总线号|
|Secondary Bus Number|次级总线号|
|Subordinate Bus Number|从属总线号|
|Enumeration|枚举|

------

原文：  Mindshare

译者：  Michael ZZY

校对：  LJGibbs

欢迎参与 《Mindshare PCI Express Technology 3.0 一书的中文翻译计划》

https://gitee.com/ljgibbs/chinese-translation-of-pci-express-technology