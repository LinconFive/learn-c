### HPC
A 256-Processor Sun Cluster, HPC = high performance clusters

|Type|Comments|
|----|----|
|Processor|AMD OPETRON 2218 DUAL CORE DUAL SOCKET|
|Master Nodes|1|
|Computing Nodes|64|
|CLUSTER Software|ROCKS|
|Perf|1.33T.F=No of nodes X No .of processors and cores X CPU speed X No of floating point operations per second = 64 X 4 X 2.6GHz X 2|
||telecommunications|
||networks|
||statistical information|
||huge logs|
||system operation|

### Parallel

- PVP (Parallel Vector Processor)
- SMP (Symmetric Multiprocessor)
- MPP (Massively Parallel Processor) -> 20%
- COW (Cluster of Workstation) -> 58.8%
- DSM (Distributed Shared Memory)

MPI,(Message Passing Interface)

### CMN700

- scalabal mesh with 256-processor -> 16*16 -> 17*17(size)
- CHI Issue E
  - RN (Requster)
  - HN (Home)
  - SN (Subordinate)
- System Function
  - QoS
  - RAS
  - DT
- IP
  - DMC
  - GIC
  - MMU
  - NIC
  - Armv8 processor
- CML
  - CCIX
  - CXL
  - SMP

Software discovery consists of three steps:
1. Read information in the 64KB region at offset 0x0. This information determines the number of XPs
in CMN-700 and the offset from PERIPHBASE for the 64KB region of each XP.
2. Read information in the 64KB region that is associated with each XP. This information determines
the components that are associated with that XP, topology information for those components, and the
offset from PERIPHBASE for each component 64KB region.
3. Read information in the 64KB region that is associated with the component. This information
determines the type of block and the configuration details of the component.


Base  => 0x60000000

6x6   => address space 256MB

64KB  => 65536B * 8 / 64 = 8192 reg

256MB => 268435456B


|Device|Type|Inst|
|---|---|---|
|AP|RN-F|with cache|
|SRAM|SBSX||
|AP|RN-I|without cache|
|CML|CXG|CCIX|
|   |CCG|    |
|DMC-620|SN-F||
|DRAM|HN-F||
||HN-I||
|||HN-T|
|||HN-D|
|||HN-P|
|||HN-V|

~~~
SCP
0x6000_0000-0x9FFF_FFFF       1GB    Memory region to access AP Memory map
AP
0x00_5000_0000 0x00_5FFF_FFFF 256MB  Reserved space for CMN-700 configuration space
0x01_4000_0000 0x01_7FFF_FFFF 1GB    CMN-700 GPV space (configuration space for CMN-700)
~~~




### AMBA

|1  |2  |3  |4  |CHI|
|---|---|---|---|---|
|ASB<br>Advanced System Bus|ASB|   |    |   |
|APB<br>Advanced Peripheral Bus|APB|APB     |APB||
|   |AHB<br>Advanced High-performance Bus|AHB-Lite|   ||
|   |   |ATB|ATB||
|   |   |AXI<br>Advanced eXtensible Interface|AXI||
|   |   ||ACE||
|   |   ||ACE-Lite||
|   |   ||AXI-Lite||

APB : The Advanced Peripheral Bus (APB) is used for connecting low bandwidth peripherals. It is a simple non-pipelined protocol that can be used to communicate(read or write) from a bridge/master to a number of slaves through the shared bus. The reads and writes shares the same set of signals and no burst data transfers are supported. The latest spec (APB 2.0) is available on ARM website here and is a relatively easy protocol to learn.

AHB: The Advanced High-performance Bus (AHB) is used for connecting components that need higher bandwidth on a shared bus. These could be a internal memory or an external memory interface, DMA , DSP etc but the shared bus would limit the number of agents. Similar to APB, this is a shared bus protocol for multiple masters and slaves, but higher bandwidth is possible through burst data transfers. The latest spec can be found on ARM website here and is relatively easy to learn

AHB-lite protocol is a simplified version of AHB. The simplification comes with support for only a single master design and that removes need for any arbitration, retry, split transactions etc.

AXI: The Advanced Extensible interface (AXI) is useful for high bandwidth and low latency interconnects. This is a point to point interconnect and overcomes the limitations of a shared bus protocol in terms of number of agents that can be connected. The protocol also was an enhancement from AHB in terms of supporting multiple outstanding data transfers (pipe-lined), burst data transfers, separate read and write paths and supporting different bus widths.

AXI-lite protocol is a simplified version of AXI and the simplification comes in terms of no support for burst data transfers.

AXI-stream protocol is another flavor of the AXI protocol that supports only streaming of data from a master to a slave. There is no separate read/write channels in the stream protocol unlike a full AXI or AXI-lite as the intend is to only stream in one direction. Multiple streams of data can be transferred (even with interleaving) across a master and slave. This becomes useful in designs like video streaming applications.
The full AXI and AXI-lite specification can be downloaded on ARM website here. The AXI-stream protocol has a different spec and is available here for download.

ACE — AXI Coherence extension protocol is an extension to AXI 4 protocol and evolved in the era of multiple CPU cores with coherent caches getting integrated on a single chip. The ACE protocol extends the AXI read and write data channels by introducing separate snoop address, snoop data and snoop response channels. These extra channels provides mechanisms to implement a snoop based coherency protocol. If you are new to coherency, understanding that will be a pre-requisite before learning ACE protocol. The spec is available for download from ARM here as part of AXI4 spec

ACE-Lite — The ACE also has a simplified version of protocol for those agents that does not have a cache of its own but still are part of the shareable coherency domain. Typical agents like DMA or network interface agents fall implement this “one-way” coherency using a ACE-lite protocol.

CHI —( Coherent Hub Interface) — The ACE protocol was developed as an extension to AXI to support coherent interconnects. The ACE protocol used a signal level communication between master/slave and hence the interconnects needed large number of wires with added channels for snoops and responses. This worked well for small coherent clusters with dual/quad core mobile SOC designs. With increasing number of coherent clusters on SOC along with other heterogeneous compute elements and memory controllers — the AMBA 5 revision introduced CHI protocol as a complete re-design of the ACE protocol. The CHI protocol uses a layered packet based communication protocol with protocol, link layer and physical layer implementation and also supports QoS based flow control and retry mechanisms.


<a href="#top">Back to top</a>
