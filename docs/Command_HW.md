### Main

[C2][Main](index.md)😃.

### ARMv-7

ref: DDI0403E_d_armv7m_arm.pdf

address space: 2^32 - 1, total 2^32 8-bit bytes, this address space is regarded as consisting of 230 32-bit words, each of whose addresses is word-aligned, meaning 
that the address is divisible by 4. The word whose word-aligned address is A consists of the four bytes with addresses A, A+1, A+2, and A+3. Thus, normal sequential execution of instructions effectively calculates `(address_of_current_instruction) + (2 or 4)`, mapping to 16 and 32 bit instr. If the instr overflows the top addr, 0xFFFFFFF, then it shows `UNPREDICTABLE`.

aligh: All instruction fetches must be halfword-aligned

### ARMv-7

ref: DDI0403E_d_armv7m_arm.pdf

address space: 2^32 - 1, total 2^32 8-bit bytes, this address space is regarded as consisting of 230 32-bit words, each of whose addresses is word-aligned, meaning 
that the address is divisible by 4. The word whose word-aligned address is A consists of the four bytes with addresses A, A+1, A+2, and A+3. Thus, normal sequential execution of instructions effectively calculates `(address_of_current_instruction) + (2 or 4)`, mapping to 16 and 32 bit instr. If the instr overflows the top addr, 0xFFFFFFF, then it shows `UNPREDICTABLE`.

aligh: All instruction fetches must be halfword-aligned

take below as example, align with 4 byte, can need one time or two time r/w.

~~~                                                                                                   
 0            1           2           3           4           5           6           7           8                         
  +-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
  |           |           |           |           |           |           |           |           |
  |     d     |     a     |     t     |     a     |           |           |           |           |
  |           |           |           |           |           |           |           |           |
  +-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
  <----------------------one time----------------->
               
 0            1           2           3           4           5           6           7           8                         
  +-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
  |           |           |           |           |           |           |           |           |
  |           |     d     |     a     |     t     |      a    |           |           |           |
  |           |           |           |           |           |           |           |           |
  +-----------+-----------+-----------+-----------+-----------+-----------+-----------+-----------+
  <----------------------one time----------------->
                                                  <----------------------one time----------------->
  
  
~~~


address map:

```
32bit 

>>> 2**32
4294967296 = 4 294 967 296 = 4GB 
>>> '0x1 0000 0000'

64KB, 256MB, 1GB to B => bits * 8
>>> 1024*64
65536
>>> 1024*1024*256
268435456
>>> 1024*1024*1024
1073741824

B / 1024 / 1024 / 1024
    KB     MB     GB



```
  
|Addr|Name|Description|
|---|---|---|
|0000 0000<br>1FFF FFFF|Code|Flash|
|2000 0000<br>3FFF FFFF|SRAM|On-chip RAM|
|4000 0000<br>5FFF FFFF|Prph|On-chip peripheral|
|6000 0000<br>7FFF FFFF|RAM|L2-L3|
|8000 0000<br>9FFF FFFF|RAM|cache|
|A000 0000<br>BFFF FFFF|share||
|C000 0000<br>DFFF FFFF|non-share||
|E000 0000<br>FFFF FFFF|system|Private Peripheral Bus or verdor system peripheral|

from 6000 0000 to 7FFF FFFF, 0.5GB
from 6000 0000 to 9FFF FFFF, 1GB


      
|cmd|note|      
|---|---|
|echo "ibase=16;obase=2;1000000000000000000000000000000" \| BC_LINE_LENGTH=0 bc| show number in bits|
|xxd -c 8 a.out| show each line 64 bits in hex|
|xxd -c 8 -b a.out| show each line 64 bits|
|xxd -c 8 a.out \| cut -d' ' -f2-5| show only 8 bytes without addr and ascii|
|xxd -c 8 -g 8 a.out \| cut -d' ' -f2| show 8 bytes without space|
|xxd -s 0x4170 a.out| print from addr 4170|
|xxd ||


### bus size

|CPU|Addr Bus|Max RAM|
|---|--------|-------|
|8086|20b|1MB|
|80286|24b|16MB|
|80386DX|32b|4GB|
|80486DX|32b|4GB|
|Pentium I|32b|4BG|
|Pentium II|36b|64GB|
|Pentium III|36b|64GB|
|Pentium 4|36b|64GB|
|Athlon-64|40b|1TB|

ref: (linux 64 bit addr)[https://www.kernel.org/doc/Documentation/x86/x86_64/mm.txt]


















### Linux get hardware

- architechture
```
arch

ls /sys/devices/system/node #only node0 means smp, more means numa node 
ls /sys/devices/system/node/node0 #get the logic CPU for node

```

- bus
```
grep ^address /proc/cpuinfo | uniq
```

- logic CPU
```
grep process /proc/cpuinfo

```

- core
```
grep core /proc/cpuinfo
```

- processor stat
```
cat /proc/stat
```

- run bit
```
getconf LONG_BIT #64means running at 64bit
```

- cache, l123
```
$ cat /sys/devices/system/cpu/cpu0/cache/index0/level #this is D-Cache
1
$ cat /sys/devices/system/cpu/cpu0/cache/index1/level #this is I-cache
1
$ cat /sys/devices/system/cpu/cpu0/cache/index2/level
2
$ cat /sys/devices/system/cpu/cpu0/cache/index3/level
3
$ ls /sys/devices/system/cpu/cpu0/cache
index0  index1  index2  index3  uevent
$ cat /sys/devices/system/cpu/cpu0/cache/index0/type
Data
$ cat /sys/devices/system/cpu/cpu0/cache/index1/type
Instruction
$ cat /sys/devices/system/cpu/cpu0/cache/index2/type
Unified
$ cat /sys/devices/system/cpu/cpu0/cache/index3/type
Unified

$ cat /sys/devices/system/cpu/cpu0/cache/index3/size
25600K
$ cat /sys/devices/system/cpu/cpu0/cache/index2/size
256K
$ cat /sys/devices/system/cpu/cpu0/cache/index1/size
32K
$ cat /sys/devices/system/cpu/cpu0/cache/index0/size
32K
```


- numa check on/off
```
dmesg | grep numa

find /sys -name numa_node | xargs cat #get device numa id
```




<a href="#top">Back to top</a>



address map:

32bit >>> 4294967296 = 4 294 967 296 = 0.5GB 
      >>> '0x1 0000 0000'
  
|Addr|Name|Description|
|---|---|---|
|0000 0000<br>1FFF FFFF|Code|Flash|
|2000 0000<br>3FFF FFFF|SRAM|On-chip RAM|
|4000 0000<br>5FFF FFFF|Prph|On-chip peripheral|
|6000 0000<br>7FFF FFFF|RAM|L2-L3|
|8000 0000<br>9FFF FFFF|RAM|cache|
|A000 0000<br>BFFF FFFF|share||
|C000 0000<br>DFFF FFFF|non-share||
|E000 0000<br>FFFF FFFF|system|Private Peripheral Bus or verdor system peripheral|

```
64KB, 256MB, 1GB to B => bits * 8
>>> 1024*64
65536
>>> 1024*1024*256
268435456
>>> 1024*1024*1024
1073741824
```
      






















### Linux get hardware

- architechture
```
arch

ls /sys/devices/system/node #only node0 means smp, more means numa node 
ls /sys/devices/system/node/node0 #get the logic CPU for node

```


- logic CPU
```
grep process /proc/cpuinfo

```

- core
```
grep core /proc/cpuinfo
```

- processor stat
```
cat /proc/stat
```

- run bit
```
getconf LONG_BIT #64means running at 64bit
```

- cache, l123
```
$ cat /sys/devices/system/cpu/cpu0/cache/index0/level #this is D-Cache
1
$ cat /sys/devices/system/cpu/cpu0/cache/index1/level #this is I-cache
1
$ cat /sys/devices/system/cpu/cpu0/cache/index2/level
2
$ cat /sys/devices/system/cpu/cpu0/cache/index3/level
3
$ ls /sys/devices/system/cpu/cpu0/cache
index0  index1  index2  index3  uevent
$ cat /sys/devices/system/cpu/cpu0/cache/index0/type
Data
$ cat /sys/devices/system/cpu/cpu0/cache/index1/type
Instruction
$ cat /sys/devices/system/cpu/cpu0/cache/index2/type
Unified
$ cat /sys/devices/system/cpu/cpu0/cache/index3/type
Unified

$ cat /sys/devices/system/cpu/cpu0/cache/index3/size
25600K
$ cat /sys/devices/system/cpu/cpu0/cache/index2/size
256K
$ cat /sys/devices/system/cpu/cpu0/cache/index1/size
32K
$ cat /sys/devices/system/cpu/cpu0/cache/index0/size
32K
```


- numa check on/off
```
dmesg | grep numa

find /sys -name numa_node | xargs cat #get device numa id
```




<a href="#top">Back to top</a>
