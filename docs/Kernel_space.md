### Main

[C2][Main](index.md)😃.

### LinuxOS

|   composite    |         |                                                                                                                |     |
| -------------- | ------- | -------------------------------------------------------------------------------------------------------------- | --- |
| app            |         |                                                                                                                |     |
| glibc and file |         |                                                                                                                |     |
| Shell          |         |                                                                                                                |     |
| System Call    |         |                                                                                                                |     |
| Kernel         |         |                                                                                                                |     |
|                | VFS     |                                                                                                                |     |
|                | Process | Scheduling Policy, Architecture-specific Schedulers, Architecture-independent Scheduler, System Call Interface | CPU |
|                | Memory  | Architecture Specific Managers, Architecture Independent Manager, System Call Interface                        | DDR |
|                | IPC     |                                                                                                                |     |
|                | Driver  |                                                                                                                | IO  |
|                | Network |                                                                                                                | NIC |





### MM

Memory in modern linux is not accessed directly. A virtual address space is used that is backed by physical memory. Conceptually, virtual and physical memory is divided into chunks called pages. The typical page size is 4096 bytes.

So, what does this buy us? Well, quite a lot, as it turns out. For one, there's less memory management hassle while sharing memory among processes. This model is also more secure, as each process has its own virtual memory space (memory isolation). It also yields virtually unlimited memory, as backing with physical pages can be on-demand (demand paging). Moreover, the system can swap inactive pages to the hard drive. Refer to our article on managing swap-space for additional information.

Virtual memory space is segregated into user and kernel space. The kernel space is the higher part of the virtual memory address space. For example, in x86_64 architecture, this mapping starts at 0xffff800000000000.

Besides memory regions, hardware architectures also provide restrictions on I/O ports and [CPU](Hardware_CPU.md) instructions. For example, in x86, we have four protection rings numbered 0 to 3, although in Linux, we only use ring-0 (kernel mode) and ring-3 (user mode).

If a user process requires services that are restricted, it can use system calls (syscalls). Collectively, these syscalls form an interface for user applications to access kernel resources.

In the user space, we can find the user stack that grows downward to lower addresses, whereas dynamic allocations (heap) grow upwards to higher addresses. The user stack is only used while the process is running in user mode.

The kernel stack is part of the kernel space. Hence, it is not directly accessible from a user process. Whenever a user process uses a syscall, the CPU mode switches to kernel mode. During the syscall, the kernel stack of the running process is used.

The size of the kernel stack is configured during compilation and remains fixed. This is usually two pages (8KB) for each thread. Moreover, additional per-CPU interrupt stacks are used to process external interrupts. While the process runs in user mode, these special stacks don’t have any useful data.

Unlike the kernel stack, we can change the user stack via `ulimit`.

- space

|         |                              32                               |                                       64                                        |
| ------- | ------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| address | 4GB(2^32)                                                     | 16EB(2^64)                                                                      |
| user    | 0~3G                                                          | 128TB（0x0000 00000000∼0x7FFF FFFFFFFF                                           |
| kernel  | 3G~4G                                                         | 128TB（0xFFFF8000 00000000∼0xFFFFFFFF FFFFFFFF）                                 |
| cpu     | 32-bit operating systems and applications require 32-bit CPUs | 64-bit OS demands 64-bit CPU, and 64-bit applications require 64-bit OS and CPU |
| ram     | 3.2 GB                                                        | 17 Billion GB                                                                   |

- C layout

user space (reserve -> text -> data -> bss -> heap -> ... -> mmap -> stack -> argc, argv -> env)                                                  -> kernel space

reserve: null

text: cpu instruction code

data: variables not equal to 0

bss: variables equal to 0

heap: to high

mmap: dynamic so, input file

stack: to low

### ARMv8

- MMU

ARM Core ---- MMU(TLB & TWU) ---- Cache ---- Memory(TCU)
| L1    |     |L2     |     |
| --- | --- | --- | --- |
| Instruction    | 48 entry    |     | 1024 entry    |
|  Data   |  32 entry   |     |     |
|     |     |     |     |

- Page
TTBR ---- L0 Table ---- L1 Table ---- L2 Table ---- L3 Table


<a href="#top">Back to top</a>
