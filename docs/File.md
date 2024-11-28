### Main

[C2][Main](index.md)😃.

### ELF

ELF is in disk, later it is going to map to memory...

Take below as example,

.plt, .text, .init => Executable Segment (r-x)
.got, .data, .bss => Data Segment (rw-)
under .bss => Stack (rw-)

~~~
 .text: Executable instructions
 .bss: Unitialized data (usually the heap)
 .data: initialized data
 .rodata: Read-Only data
 .got: Global Offset Table
 .plt: Procedure Linkage Table
 .init/.fini glibc
~~~

- graph

~~~

     ┌────────────────┐
     │                │
     │  ELF header    │
     │                │
     │                │
     └────────────────┘

     ┌────────────────┐
     │                │
     │  Program Header│
     │                │
     │                │
     └────────────────┘

     ┌────────────────┐
     │                |
     │  .plt          |
     │                │
     │                │
     │                │
     └────────────────┘


     ┌────────────────┐
     │                |
     │  .text         |
     │                │
     │                │
     │                │
     └────────────────┘

     
     ┌────────────────┐
     │                |
     │  .init         |
     │                │
     │                │
     │                │
     └────────────────┘

     ┌────────────────┐
     │                │
     │   .rodata      │
     │                │
     │                │
     └────────────────┘

     ┌────────────────┐
     │                │
     │    .data       │
     │                │
     │                │
     └────────────────┘
      
     ┌────────────────┐
     │                │
     │    .bss        │
     │                │
     │                │
     └────────────────┘
     
      ...
      
     ┌────────────────┐
     │                |
     │  section header|
     │                │
     │                │
     │                │
     └────────────────┘

~~~

- content

`readelf -a` and `hexdump -C` will print the ELF contents.
Use `objdump -s -d .o` to get the asm code of the ELF, this is same as .text from above.
Use `objdump -h .elf` to get sections.

The ELF header `readelf -h` is 32 bytes long, and identifies the format of the file. It starts with a sequence of four unique bytes that are 0x7F followed by 0x45, 0x4c, and 0x46 which 
translates into the three letters E, L, and F. Among other values, the header also indicates whether it is an ELF file for 32 or 64-bit format, uses little or big endianness,
shows the ELF version as well as for which operating system the file was compiled for in order to interoperate with the right application binary interface (ABI) and cpu 
instruction set.

The program header `readelf -l` shows the segments used at run-time, and tells the system how to create a process image. The header from Listing 2 shows that the ELF file consists of 9 
programheaders that have a size of 56 bytes each, and the first header starts at byte 64.

The third part of the ELF structure is the section header `read -S`. It is meant to list the single sections of the binary. The switch -S (short for –section-headers or –sections) lists
the different headers. As for the touch command, there are 27 section headers, and Listing 5 shows the first four of them plus the last one, only. Each line covers the section
size, the section type as well as its address and memory offset.


- Layout

~~~
0x0                                                            ----- Code Area
Program Text (.text)                                           -----
Initialized Data (.data)                                             Static Area
Uninitialized Data (.bss)                                      -----
Heap                                                           -----
    |
    v
Memory Mapped Region for Shared Libraries or Anything Else           Dynamic Area
    ^
    |
User Stack                                                     -----
~~~

Static + Dynamic = Data
Data + Code = Memory 

```
#include <stdlib.h>
#include <stdio.h>

#define KNRM  "\x1B[0m"
#define KRED  "\x1B[31m"
#define KGRN  "\x1B[32m"
#define KYEL  "\x1B[33m"
#define KBLU  "\x1B[34m"
#define KMAG  "\x1B[35m"
#define KCYN  "\x1B[36m"
#define KWHT  "\x1B[37m"

char *globalVar = "Global";

int main () {

    printf("%sLook at /proc/%d/maps%s\n", KMAG, getpid(), KNRM);
    // printf("%snormal\n", KNRM);

    /*
    // allocate 200 KiB, forcing a mmap instead of brk
    char * addr = (char *) malloc(204800);

    getchar();

    free(addr);
    */

        char stackVar[16];
        char *heapVar = (char *) malloc(4);
        printf("Global var: %p\n", globalVar);
        printf("Heap   var: %p\n", heapVar);
        printf("Stack  var: %p\n", stackVar);
        getchar();
        free(heapVar);
    return 0;

}

gcc mmap.c -o mmap

./mmap
Look at /proc/53594/maps
Global var: 0xaaaaca37ca98
Heap   var: 0xaaaad92dd670
Stack  var: 0xffffdd8a7388

readelf -l mmap
Elf file type is DYN (Shared object file)
Entry point 0x830
There are 8 program headers, starting at offset 64

Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  PHDR           0x0000000000000040 0x0000000000000040 0x0000000000000040
                 0x00000000000001c0 0x00000000000001c0  R      0x8
  INTERP         0x0000000000000200 0x0000000000000200 0x0000000000000200
                 0x000000000000001b 0x000000000000001b  R      0x1
      [Requesting program interpreter: /lib/ld-linux-aarch64.so.1]
  LOAD           0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000b04 0x0000000000000b04  R E    0x10000
  LOAD           0x0000000000000d40 0x0000000000010d40 0x0000000000010d40
                 0x00000000000002d8 0x00000000000002e0  RW     0x10000
  DYNAMIC        0x0000000000000d50 0x0000000000010d50 0x0000000000010d50
                 0x0000000000000200 0x0000000000000200  RW     0x8
  NOTE           0x000000000000021c 0x000000000000021c 0x000000000000021c
                 0x0000000000000044 0x0000000000000044  R      0x4
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     0x10
  GNU_RELRO      0x0000000000000d40 0x0000000000010d40 0x0000000000010d40
                 0x00000000000002c0 0x00000000000002c0  R      0x1

 Section to Segment mapping:
  Segment Sections...
   00
   01     .interp
   02     .interp .note.ABI-tag .note.gnu.build-id .gnu.hash .dynsym .dynstr .gnu.version .gnu.version_r .rela.dyn .rela.plt .init .plt .text .fini .rodata .eh_frame
   03     .init_array .fini_array .dynamic .got .data .bss
   04     .dynamic
   05     .note.ABI-tag .note.gnu.build-id
   06
   07     .init_array .fini_array .dynamic .got

cat /proc/53594/maps
aaaaca37c000-aaaaca37d000 r-xp 00000000 08:02 228207670                  /home/zzx/test/mmap
aaaaca38c000-aaaaca38d000 r--p 00000000 08:02 228207670                  /home/zzx/test/mmap
aaaaca38d000-aaaaca38e000 rw-p 00001000 08:02 228207670                  /home/zzx/test/mmap
aaaad92dd000-aaaad92fe000 rw-p 00000000 00:00 0                          [heap]
ffffaefa1000-ffffaf0e0000 r-xp 00000000 08:02 61210694                   /lib/aarch64-linux-gnu/libc-2.27.so
ffffaf0e0000-ffffaf0f0000 ---p 0013f000 08:02 61210694                   /lib/aarch64-linux-gnu/libc-2.27.so
ffffaf0f0000-ffffaf0f4000 r--p 0013f000 08:02 61210694                   /lib/aarch64-linux-gnu/libc-2.27.so
ffffaf0f4000-ffffaf0f6000 rw-p 00143000 08:02 61210694                   /lib/aarch64-linux-gnu/libc-2.27.so
ffffaf0f6000-ffffaf0fa000 rw-p 00000000 00:00 0
ffffaf115000-ffffaf132000 r-xp 00000000 08:02 61210664                   /lib/aarch64-linux-gnu/ld-2.27.so
ffffaf13e000-ffffaf140000 rw-p 00000000 00:00 0
ffffaf140000-ffffaf141000 r--p 00000000 00:00 0                          [vvar]
ffffaf141000-ffffaf142000 r-xp 00000000 00:00 0                          [vdso]
ffffaf142000-ffffaf143000 r--p 0001d000 08:02 61210664                   /lib/aarch64-linux-gnu/ld-2.27.so
ffffaf143000-ffffaf145000 rw-p 0001e000 08:02 61210664                   /lib/aarch64-linux-gnu/ld-2.27.so
ffffdd888000-ffffdd8a9000 rw-p 00000000 00:00 0                          [stack]


```

Stack  var: 0xffffdd8a7388 is in ffffdd888000-ffffdd8a9000

Heap   var: 0xaaaad92dd670 is in aaaad92dd000-aaaad92fe000

```
objdump -d mmap | grep section -A 2
# 16 hex-bit addr = 64 bit cpu

Disassembly of section .init:

0000000000000750 <_init>:
--
Disassembly of section .plt:

0000000000000770 <.plt>:
--
Disassembly of section .text:

0000000000000830 <_start>:
--
Disassembly of section .fini:

0000000000000a7c <_fini>:

readelf --file-header mmap
readelf --program-header mmap
readelf --section-header mmap
```

so we realise, 

Entry point = 0830, same as start
```
objdump --disassemble-all mmap | grep \>:

0000000000000200 <.interp>:
000000000000021c <.note.ABI-tag>:
000000000000023c <.note.gnu.build-id>:
0000000000000260 <.gnu.hash>:
0000000000000280 <.dynsym>:
0000000000000400 <.dynstr>:
00000000000004de <.gnu.version>:
0000000000000500 <.gnu.version_r>:
0000000000000540 <.rela.dyn>:
0000000000000660 <.rela.plt>:
0000000000000750 <_init>:
0000000000000770 <.plt>:
0000000000000790 <__cxa_finalize@plt>:
00000000000007a0 <getpid@plt>:
00000000000007b0 <malloc@plt>:
00000000000007c0 <__libc_start_main@plt>:
00000000000007d0 <__stack_chk_fail@plt>:
00000000000007e0 <__gmon_start__@plt>:
00000000000007f0 <abort@plt>:
0000000000000800 <free@plt>:
0000000000000810 <getchar@plt>:
0000000000000820 <printf@plt>:
0000000000000830 <_start>:
0000000000000868 <call_weak_fn>:
0000000000000880 <deregister_tm_clones>:
00000000000008b0 <register_tm_clones>:
00000000000008e8 <__do_global_dtors_aux>:
0000000000000930 <frame_dummy>:
0000000000000934 <main>:
00000000000009f8 <__libc_csu_init>:
0000000000000a78 <__libc_csu_fini>:
0000000000000a7c <_fini>:
0000000000000a90 <_IO_stdin_used>:
0000000000000b00 <__FRAME_END__>:
0000000000010d40 <__frame_dummy_init_array_entry>:
0000000000010d48 <__do_global_dtors_aux_fini_array_entry>:
0000000000010d50 <.dynamic>:
00000000000004de <.gnu.version>:
0000000000000500 <.gnu.version_r>:
0000000000000540 <.rela.dyn>:
0000000000000660 <.rela.plt>:
0000000000000750 <_init>:
0000000000000770 <.plt>:
0000000000000790 <__cxa_finalize@plt>:
00000000000007a0 <getpid@plt>:
00000000000007b0 <malloc@plt>:
00000000000007c0 <__libc_start_main@plt>:
00000000000007d0 <__stack_chk_fail@plt>:
00000000000007e0 <__gmon_start__@plt>:
00000000000007f0 <abort@plt>:
0000000000000800 <free@plt>:
0000000000000810 <getchar@plt>:
0000000000000820 <printf@plt>:
0000000000000830 <_start>:
0000000000000868 <call_weak_fn>:
0000000000000880 <deregister_tm_clones>:
00000000000008b0 <register_tm_clones>:
00000000000008e8 <__do_global_dtors_aux>:
0000000000000930 <frame_dummy>:
0000000000000934 <main>:
00000000000009f8 <__libc_csu_init>:
0000000000000a78 <__libc_csu_fini>:
0000000000000a7c <_fini>:
0000000000000a90 <_IO_stdin_used>:
0000000000000b00 <__FRAME_END__>:
0000000000010d40 <__frame_dummy_init_array_entry>:
0000000000010d48 <__do_global_dtors_aux_fini_array_entry>:
0000000000010d50 <.dynamic>:
0000000000010f50 <.got>:
0000000000011000 <__data_start>:
0000000000011008 <__dso_handle>:
0000000000011010 <globalVar>:
0000000000011018 <__bss_start>:
0000000000000000 <.comment>:

stat mmap
  File: mmap
  Size: 9360            Blocks: 24         IO Block: 4096   regular file
Device: 802h/2050d      Inode: 228207670   Links: 1
Access: (0775/-rwxrwxr-x)  Uid: ( 1001/     zzx)   Gid: ( 1001/     zzx)
Access: 2022-03-11 11:54:04.417823093 +0800
Modify: 2022-03-11 11:54:02.425727141 +0800
Change: 2022-03-11 11:54:02.425727141 +0800
 Birth: -
 
objdump --disassemble-all --start-address=0x000000 --stop-address=0x401000 mmap | less +/0000000000000830
0000000000000830 <_start>:
 830:   d280001d        mov     x29, #0x0                       // #0
 834:   d280001e        mov     x30, #0x0                       // #0
 838:   aa0003e5        mov     x5, x0
 83c:   f94003e1        ldr     x1, [sp]
 840:   910023e2        add     x2, sp, #0x8
 844:   910003e6        mov     x6, sp
 848:   90000080        adrp    x0, 10000 <__FRAME_END__+0xf500>
 84c:   f947f800        ldr     x0, [x0, #4080]
 850:   90000083        adrp    x3, 10000 <__FRAME_END__+0xf500>
 854:   f947f463        ldr     x3, [x3, #4072]
 858:   90000084        adrp    x4, 10000 <__FRAME_END__+0xf500>
 85c:   f947e084        ldr     x4, [x4, #4032]
 860:   97ffffd8        bl      7c0 <__libc_start_main@plt>
 864:   97ffffe3        bl      7f0 <abort@plt>
```


shift aaaaca37c000

```
0
Nothing here, because it was just an arbitrary choice by the linker
ELF and Program and Section Headers - 0x200
Program Text (.text) - Entry Point as Reported by readelf 0x830
Nothing Here either
Some unknown assembly and data - 0x
Initialised Data (.data) - 0x11000
Uninitialised Data (.bss) - 0x11018 
Heap
    |
    v
Memory Mapped Region for Shared Libraries or Anything Else
    ^
    |
User Stack

```

### Memory

when a code is compiled on linux, then the program maps
|addr|part|
|----|----|
|0xFFFFFFFF|kernel virtual|
|0xC0000000|user stack|
|0x40000000|disk part|
||bss|
||text|
|0x08048000|reserved|
|0x0|>>>|


### APK
use android studio to build apk, and debug with adb, take robox as example
```
#ssh 920 use robox
cd ~/robox_920_huawei/script/download/robox/binaryFiles
./robox start 1
netstat -tlp | grep 5559
adb connect 192.168.10.111:5559
scrcpy -s 192.168.10.111:5559
http://192.168.10.111:8090/files
adb install zd.apk
#success
#inside anbox
adb shell
#inside 920
perf

adb push icon.png /sdcard/Download

adb shell am start com.android.documentsui/.LauncherActivity

adb shell am start com.android.gallery3d/.LauncherActivity

adb shell am start com.android.gallery3d/.app.GalleryActivity

adb shell am force-stop com.android.gallery3d

adb shell am force-stop com.android.deskclock

adb shell am start com.android.music/.MusicBrowserActivity

adb push icon.png /storage/self/primary/Pictures/

adb shell am start -a android.intent.action.VIEW -d http://www.baidu.com

adb push icon.png /storage/self/primary/Movies C:\Users\zixia\Documents\robox

adb shell am force-stop com.qiyi.video

adb shell dumpsys activity | findstr "mFocusedActivity"

adb shell am force-stop com.android.documentsui

adb shell ls sdcard

```

### EXE

<a href="#top">Back to top</a>
