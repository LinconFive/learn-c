### Main

[C2][Main](index.md)😃.

### Stack

We should first locate where C stands on, mostly it locates up from OS to APP.
C is known for FW, which links HW and SW, and many other langueges have embedding interface to C, so they can modify the lower level of the system easily.
Maybe the only drawback is C's UI not fashionable, after all it is 2021 now :flushed:.
                            
~~~
   ┌─────────────────┐
   │                 │
   │ Application     │
   │                 │
   └─────────────────┘

   ┌─────────────────┐
   │                 │
   │ Algorithm       │
   │                 │
   └─────────────────┘

   ┌─────────────────┐
   │                 │
   │ C Language      │
   │                 │
   └─────────────────┘

   ┌─────────────────┐
   │                 │
   │ Operating System│
   │                 │
   └─────────────────┘

   ┌─────────────────┐
   │                 │
   │ ISA             │
   │                 │
   └─────────────────┘

   ┌─────────────────┐
   │                 │
   │ RTL             │
   │                 │
   └─────────────────┘

   ┌─────────────────┐
   │                 │
   │ Circuit         │
   │                 │
   ├─────────────────┤
   
   ├─────────────────┤
   │                 │
   │ Component       │
   │                 │
   └─────────────────┘
      
   ├─────────────────┤
   │                 │
   │ Physical        │
   │                 │
   └─────────────────┘
~~~

### Linux

For most users, C runs on Windows, Linux or MacOS, corresponding to Lenovo, Dell or Mac in hardware.

We are going to use Linux since it's [open](https://github.com/torvalds/linux) source, and we can download [here](https://www.kernel.org/).

The creator of Linux is Linus Torvalds, who mimiced Unix in 1991, thus name combined. 

As of 25 Nov 2021, he is 52 years old, since he was born at 28 December 1969. Yet Linux has a very <a href="./asset/linux_distro.png" target="_blank">vast</a> history in terms of its version.

Less equal than 2.6, the version goes major.minor.modification-release cycle. Current version is 5.16-rc2, so there is no modification time, nor even represent stable.

### GNU

GNU's Not Unix! 

Linux is actually GNU/Linux, that is to say GNU is a bigger project. One may ask then what is GNU's OS? [Hurd](https://www.gnu.org/software/hurd/).

On Linux, GNU is more like toolchain, includes the GNU Make, the GNU C Library, the GNU Debugger, and the GNU build system.

GCC is short for GNU Compiler Collection, and GDB is short for GNU Project.

At this point, we should recall who created such perfect tool, or toolchain?

Richard Stallman, collegue of Bill Gates, fights with [FSF](https://my.fsf.org/join?mtm_campaign=frfall2021&mtm_source=appeal) alone in 1985, thus we can build bigger dream since it is free.

Besides, we should recall C father Dennis Ritchie, who was gone on October 12, 2011. 

| Language      | Year | Developer                  |
|---------------|------|----------------------------|
| Algol         | 1960 | International Group        |
| BCPL          | 1967 | Martin Richard             |
| B             | 1970 | Ken Thompson               |
| Traditional C | 1972 | Dennis Ritchie             |
| K & R C       | 1978 | Kernighan & Dennis Ritchie |
| ANSI C        | 1989 | ANSI Committee             |
| ANSI/ISO C    | 1990 | ISO Committee              |
| C99           | 1999 | Standardization Committee  |
| C11           | 2011 | ANSI, ISO/IEC Committee    |

### Ubuntu

The first electronic digital computer ENIAC was developed at the University of Pennsylvania in 1946. It was not a stored program computer yet and its programming was provided through physical wiring. EDSAC, the world’s first stored-program computer, developed by Maurice Wilkes of University of Cambridge in 1949, had initial orders, an initial input routine, for storing a program described with symbols while reading and converting the program to binary codes. In the 1950s, commercial computers appeared and peripherals such as magnetic tape drives were developed in conjunction with improvements to the central processing unit, while advances were also made in software support. Early-stage programs were coded using machine languages, including hexadecimal numbers; support for assemblers and compilers followed. In 1957, IBM began providing the high-level language FORTRAN for scientific and engineering computing. The advent of high-level languages improved programming efficiency and enabled computers that used different machine languages to use the same programs. In 1958, ALGOL was introduced. And in 1961, COBOL was proposed as a language for business applications.

Around the end of the 1950s, second-generation transistor computers appeared, and computerization spread rapidly. Initially, each programmer operated the machines by himself to process his own program. Later, a monitor, early operating system was developed in order to improve the efficiency of processing . Batch processing was adopted to execute a series of programs on a computer without human interaction. Online systems and time-sharing systems(TSS) were developed in addition to batch processing systems, and the corresponding operating systems were also developed.

Over the two decades of the 1960s through the 1970s, operating systems development advanced rapidly. The first- and second-generation computers were categorized into those for scientific and engineering computing and those for business applications, and there was separate development of operating systems for batch processing, online processing and TSS. In 1964, IBM's announcement of the general-purpose computer System/360 integrated with a general-purpose operating system OS/360 was followed by widespread adoption and use of general-purpose machines and operating systems. As a result, most manufacturers shifted to production of general-purpose systems. The term "operating system （OS）" also became established. 

The first official Ubuntu release — Version 4.10, codenamed the 'Warty Warthog' — was launched in October 2004, and sparked dramatic global interest as thousands of free software enthusiasts and experts joined the Ubuntu community. 

|Number|Name|
|------|----|
|18.04|bionic|
|18.10|cosmic|
|19.04|disco|
|19.10|eoan|
|20.04|focal|
||groovy|
|21.04|hirsute|
||impish|
||streams|
|16.04|xenial|


We are going to start our journey now!

<a href="#top">Back to top</a>
