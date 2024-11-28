
### Main

[C2][Main](index.md)😃.


### Table

- Size

1 byte always equal to 8 bits

| System | word | byte | bit |
| ------ | ---- | ---- | --- |
| 16     | 1    | 2    | 16  |
| 32     | 1    | 4    | 32  |
| 64     | 1    | 8    | 64  |

At the begining, KiB equal to kilo binary bit, KB equal to kilo bit

| Type |           Byte            | 0s  |  2   |
| ---- | ------------------------- | --- | ---- |
| 1KB  | 1000                      |     |      |
| 1KiB | 1024                      | 3   | 2^10 |
| 1MiB | 1048576                   | 6   | 2^20 |
| 1GiB | 1073741824                | 9   | 2^30 |
| 1TiB | 1099511627776             | 12  | 2^40 |
| 1PiB | 1125899906842624          | 15  | 2^50 |
| 1EiB | 1152921504606846976       | 18  | 2^60 |
| 1ZiB | 1180591620717411303424    | 21  | 2^70 |
| 1YiB | 1208925819614629174706176 | 24  | 2^80 |

Nowadays KB equal to KiB

| Name      | Equal To          | Size(In Bytes)                            |
| --------- | ----------------- | ----------------------------------------- |
| Bit       | 1 Bit             | 1/8                                      |
| Nibble    | 4 Bits            | 1/2 (rare)                                |
| Byte      | 8 Bits            | 1                                         |
| Kilobyte  | 1024 Bytes        | 1024                                      |
| Megabyte  | 1, 024 Kilobytes  | 1, 048, 576                               |
| Gigabyte  | 1, 024 Megabytes  | 1, 073, 741, 824                          |
| Terrabyte | 1, 024 Gigabytes  | 1, 099, 511, 627, 776                     |
| Petabyte  | 1, 024 Terabytes  | 1, 125, 899, 906, 842, 624                |
| Exabyte   | 1, 024 Petabytes  | 1, 152, 921, 504, 606, 846, 976           |
| Zettabyte | 1, 024 Exabytes   | 1, 180, 591, 620, 717, 411, 303, 424      |
| Yottabyte | 1, 024 Zettabytes | 1, 208, 925, 819, 614, 629, 174, 706, 176 |

- Hex

| Bytes |   Hex   | Address range |
| ----- | ------- | ------------- |
| 16    | 10      | 0 - F         |
| 256   | 100     | 0 - FF        |
| 1K    | 400     | 0 - 3FF       |
| 4K    | 1000    | 0 - FFF       |
| 64K   | 10000   | 0 - FFFF      |
| 1M    | 100000  | 0 - FFFFF     |
| 16M   | 1000000 | 0 - FFFFFF    |

### Endian

Check out little or big
```
    int i = 1;   
    char *p = (char *)&i;   
    if(*p == 1)     
          printf("Little Endian"); 
    else
          printf("Big Endian");
```

Take *two bytes file a* as example
```
# below is on little
# In this scheme, low-order byte is stored on the starting address (A) and high-order byte is stored on the next address (A + 1)
$ cat a
a
$ ls -l a
-rw-rw-r-- 1 zzx zzx 2 Mar  1 07:44 a
$ hexdump a # this is big
0000000 0a61
0000002
$ xxd a # this is little
00000000: 610a
```

| Hex  |           Char            | Digit |
| ---- | ------------------------- | ----- |
| 0x61 | a                         | 97    |
| 0x0a | LF/NL(Line Feed/New Line) | 10    |

Make random number

```
c=1
w=2
b=512
kB=1000
K=1024
MB=1000*1000
M=1024*1024
xM=M
GB=1000*1000*1000
G=1024*1024*1024
...
T (terabytes), P (petabytes), E (exabytes), Z (zettabytes), and Y (yottabytes)
# this makes 1024 byte file
dd if=/dev/urandom of=test.file bs=1024 count=1

xxd test.file
# use python get digit
>>> 0x000003ff
1023
>>> 0x00004000
16384
>>> 0x00000400
1024

# use linux bc
echo "ibase=16; 400" | bc
printf "%d\n" 0x400

printf "%d\n" 0x1ffff

# range size, MCP UART
echo "ibase=16; 4C001FFF-4C001000+1" | bc
4096
# this is 4KB

```

### Range

|Number|Order|Start|End|Range|
|----|----|----|----|----|
|1KB|10|0x00|0x3ff|0x400|
|2KB|11|0x00|0x7ff|0x800|
|4KB|12|0x000|0xfff|0x1000|
|1MB|20|0x00000|0xfffff|0x100000|
|1GB|30|0x00000|0x3fffffff|0x40000000|


<a href="#top">Back to top</a>
