### Main

[C2][Main](index.md)😃.

### ISA

Probably the first confusing thing is instruction set architecture, so we start from a table below.

|  ISA   | Design |    Bits     | Begins |
| ------ | ------ | ----------- | ------ |
| x86    | CISC   | 16, 32, 64  | 1978   |
| ARM    | RISC   | 32          | 1983   |
| ARM64  | RISC   | 64          | 2011   |
| RISC-V | RISC   | 32, 64, 128 | 2010   |
| MIPS   | RISC   | 32, 64      | 1981   |

### MIX

Nowadays we have many ARM64 computer, which is mixed Harvard and Von-Neumann. 

As for RISC and CISC, ARM is RISC.

there is really no absolute superiority anyway. 

In short,

- CISC: The CISC approach attempts to minimize the number of instructions per program but at the cost of an increase in the number of cycles per instruction. 
- RISC: Reduce the cycles per instruction at the cost of the number of instructions per program. 

In long,

|CISC|RISC|
|----|----|
|Uses both hardwired and microprogrammed control unit|Focus on software	Focus on hardware|Uses only Hardwired control unit|
|Several-cycle instruction|Single-cycle instruction|
|Efficient use of RAM|Heavy use of RAM|
|Large number of instruction|Small number of fix-lenght instruction|

### CPI

Clock per instruction is a simple mearsument of the CPU speed, basicly the smaller the better.

CPU time = Instruction count x CPI / clock rate

For a RISC CPU, in university, its instruction usually falls into 5 groups,

- Instruction fetch cycle (IF).
- Instruction decode/Register fetch cycle (ID).
- Execution/Effective address cycle (EX).
- Memory access (MEM).
- Write-back cycle (WB).

And in factory, its groups extends much more, see [A64](https://developer.arm.com/documentation/ddi0602/2021-12/Base-Instructions?lang=en)(now we have A64, A32, T32 :sweat_smile:)

So if we want our program runs fast, we really have three ways,

- make the program code less, meaning smarter algorithm so inst count less
- make CPI smaller, meanning smarter ISA or shorter pipeline
- make clock frequency higher

### ARM

Before to try asm, we first should know which isa we want to use, so we have to learn the relationship.

ARMv3 ~ ARMv6, ARM name ARM{x}{y}{z}{T}{D}{M}{I}{E}{J}{F}{-S}
after ARMv7, cortex

| ARM family | ARM architecture |                    |
| ---------- | ---------------- | ------------------ |
| ARM6       | 	ARM v3          |                    |
| ARM7       | 	ARM v4          | T(Thumb)           |
| ARM9       | 	ARM v5          | E(DSP),J(JAVA)     |
| ARM11      | 	ARM v6          | S(SIMD),T(Thumb-2) |
| Cortex-A   | 	ARM v7-A        |                    |
| Cortex-R   | 	ARM v7-R        |                    |
| Cortex-M   | 	ARM v7-M        |                    |
|            | 	ARM v8          | A64,A32,T32        |

asm is composed of instructions which are the main building blocks.

ARM instructions are usually followed by one or two operands and generally use the following template: `MNEMONIC{S}{condition} {Rd}, Operand1, Operand2`

The meaning,
~~~
MNEMONIC     - Short name (mnemonic) of the instruction
{S}          - An optional suffix. If S is specified, the condition flags are updated on the result of the operation
{condition}  - Condition that is needed to be met in order for the instruction to be executed
{Rd}         - Register (destination) for storing the result of the instruction
Operand1     - First operand. Either a register or an immediate value 
Operand2     - Second (flexible) operand. Can be an immediate value (number) or a register with an optional shift
~~~

Operand2 fields are closely tied to the CPSR,
~~~
#123                    - Immediate value (with limited set of values). 
Rx                      - Register x (like R1, R2, R3 ...)
Rx, ASR n               - Register x with arithmetic shift right by n bits (1 = n = 32)
Rx, LSL n               - Register x with logical shift left by n bits (0 = n = 31)
Rx, LSR n               - Register x with logical shift right by n bits (1 = n = 32)
Rx, ROR n               - Register x with rotate right by n bits (1 = n = 31)
Rx, RRX                 - Register x with rotate right by one bit, with extend
~~~

### Instruction Count

```
$ cat e3.s
// e3.s
.text
.globl main

main:
  add w0, w0, #1   // w0 ← w0 + 1
  ret              // return from main

$ gcc -c -g e3.s
$ gcc -o e3 e3.o
```

❓ is one line one instruction

In C, there are three kinds of instruction: declare, arithmetic and contorl.
For control, there are four kinds:

|Type|Statement|
|----|----|
|Sequentail|Order|
|Decision|If..Else statement or If..Else If..Else statement|
|Loop|While loop or Do/While loop or For loop statement|
|Case|Switch Case statements|

### [Assembly](https://azeria-labs.com/writing-arm-assembly-part-1/)

- add x1, x2 into x3, use x0 as tmp

```
$ cat hello.S
.data                                                                    
                                                                         
/* Data segment: define our message string and calculate its length. */  
msg:                                                                     
    .ascii        "Hello, ARM64!\n"                                      
len = . - msg                                                            
                                                                         
.text                                                                    
                                                                         
/* Our application's entry point. */                                     
.globl _start                                                            
_start:                                                                  
    /* syscall write(int fd, const void *buf, size_t count) */           
    mov     x0, #1      /* fd := STDOUT_FILENO */                        
    ldr     x1, =msg    /* buf := msg */                                 
    ldr     x2, =len    /* count := len */                               
    mov     w8, #64     /* write is syscall #64 */                       
    svc     #0          /* invoke syscall */                             
                                                                         
    /* syscall exit(int status) */                                       
    mov     x0, #0      /* status := 0 */                                
    mov     w8, #93     /* exit is syscall #93 */                        
    svc     #0          /* invoke syscall */                             


$ as -o hello.o hello.S
$ ld -s -o hello hello.o

$ ./hello
Hello, ARM64!

zzx@ubuntu:~/test$  objdump -d hello

hello:     file format elf64-littleaarch64


Disassembly of section .text:

00000000004000b0 <.text>:
  4000b0:       d2800020        mov     x0, #0x1                        // #1
  4000b4:       580000e1        ldr     x1, 0x4000d0
  4000b8:       58000102        ldr     x2, 0x4000d8
  4000bc:       52800808        mov     w8, #0x40                       // #64
  4000c0:       d4000001        svc     #0x0
  4000c4:       d2800000        mov     x0, #0x0                        // #0
  4000c8:       52800ba8        mov     w8, #0x5d                       // #93
  4000cc:       d4000001        svc     #0x0
  4000d0:       004100e0        .inst   0x004100e0 ; undefined
  4000d4:       00000000        .inst   0x00000000 ; undefined
  4000d8:       0000000e        .inst   0x0000000e ; undefined
  4000dc:       00000000        .inst   0x00000000 ; undefined
  
# its corresponding c
#include <unistd.h>

void main() {
          const char msg[] = "Hello, ARM64!\n";
          write(0, msg, sizeof(msg));
          exit(0);
}
```

- checkout cpu arch
```
$ cat a.c
int main()
{
        int a = 1, b = 2;
        int c = a + b;
        return 0;
}
$ gcc -S a.c
$ cat a.s
        .arch armv8-a
        .file   "a.c"
        .text
        .align  2
        .global main
        .type   main, %function
main:
        sub     sp, sp, #16
        mov     w0, 1
        str     w0, [sp, 4]
        mov     w0, 2
        str     w0, [sp, 8]
        ldr     w1, [sp, 4]
        ldr     w0, [sp, 8]
        add     w0, w1, w0
        str     w0, [sp, 12]
        mov     w0, 0
        add     sp, sp, 16
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"
        .section        .note.GNU-stack,"",@progbits
        
$ as -o a.o a.s
$ ld -s -o a.out a.o # fail to have _start, _main not work


```

- gdb with armv8-a
```
(gdb) info registers                                           
x0             0x3      3                                      
x1             0x1      1                                      
x2             0xfffffffff4e8   281474976707816                
x3             0xfffffffff4f8   281474976707832                
x4             0x1c     28                                     
x5             0xffffbf700738   281473893533496                
x6             0x0      0                                      
x7             0x100404010000000        72128237928448000      
x8             0xffffffffffffffff       -1                     
x9             0x3f     63                                     
x10            0x20     32                                     
x11            0x0      0                                      
x12            0x0      0                                      
x13            0xffffbf6ff048   281473893527624                
x14            0xffffbf6fd1f8   281473893519864                
x15            0xffffbf6fd150   281473893519696                
x16            0xffffbf6ff048   281473893527624                
x17            0xffffbf6e6230   281473893425712                
x18            0x3      3                                      
x19            0x0      0                                      
x20            0x0      0                                      
x21            0xaaaaaaaaa340   187649984471872                
x22            0x0      0                                      
x23            0x0      0                                      
x24            0x0      0                                      
x25            0x0      0                                      
x26            0x0      0                                      
x27            0x0      0                                      
x28            0x0      0                                      
x29            0xfffffffff4c0   281474976707776                
x30            0xffffbf6d2244   281473893343812                
sp             0xfffffffff4c0   0xfffffffff4c0                 
pc             0xaaaaaaaaa368   0xaaaaaaaaa368 <_start+40>     
cpsr           0x60200000       [ EL=0 SS C Z ]                
fpsr           0x0      0                                      
fpcr           0x0      0                                      


```

### ARM64 asm self-note

|Register|Function|
|----|----|
|x0–x7| function arguments, scratch (x0 is also function return value)|
|x8–x18| scratch (x8 is syscall number, x16–x18 sometimes reserved)|
|x19–x28| callee-saved registers (save to stack at beginning of function, restore from stack before returning)|
|x29| frame pointer|
|x30| link register (save to stack for non-leaf functions)|
|sp| stack pointer|
|||
|/usr/include/asm-generic/unistd.h|syscall|

- asciz vs ascii
 .ascii "JNZ"      0x4A 0x4E 0x5A 0x00
 .ascii "JNZ"      0x4A 0x4E 0x5A
 
```
$ cat print.s
.data
    msg_output: .asciz "proc start \n"
.global _start
.text
_start:
    ldr x0, =msg_output  // x0 ← &msg_output [64-bit]
    bl printf                // call printf
end:
    mov x0,x2
    mov x8, #93
    svc #0
    
$ as -o print.o print.s
$ ld -g -o  print print.o -lc -I /lib/ld-linux-aarch64.so.1
$ ./print


```

rasm use objdump
```
cat hello.s
.global _start
.text
_start:
    mov x17, #0
lewp:
    mov x0, #1
    ldr x1, =msg
    ldr x2, =len
    mov w8, #64
    svc #0
    mov x18, #1
    add x17, x17, x18
    mov x18, #10
    cmp x18, x17
    bgt lewp


    mov x0, #0
    mov w8, #93
    svc #0
.data
    msg: .asciz "Hello World\n"
    len = .-msg
    
as -o hello.o hello.s
ld -o hello hello.o
objdump -s -d hello.o > hello.o.txt
cat hello.o.txt
```

❓how to get support ISA

|Type|Arch|
|----|----|
|Classic||
||ARM11|
||ARM9|
||ARM7|
|Sercure||
||SC000|
||SC300|
|MachineLearning||
||Ethos|
||CortexA|
||CortexM|
||Neoverse|
|||
|A64 instruction set|The A64 instruction set, introduced in Armv8-A to support the 64-bit architecture.|
|A32 instruction set|The A32 instruction set, referred to as ‘ARM’ in Armv6 and Armv7 architectures.|
|T32 instruction set|The T32 instruction set, referred to as ‘Thumb’ in Armv6 and Armv7 architectures.|

<a href="#top">Back to top</a>
