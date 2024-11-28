### Main

[C2][Main](index.md)😃.

### Hello World
The first application that we learn, almost for every programming language is "Hello World".

In C, this is quite neat,

```
# cat a.c
#include <stdio.h>    
int main()
{ 
     // Displays the string inside quotations
     printf("Hello World\n");
     return 0;
}
# gcc a.c
# ./a.out
Hello World
```

The above can be understood via a table,

| command  | function                  | note           |
|----------|---------------------------|----------------|
| cat      | linux show command        |                |
| include  | header for c source file  |                |
| int main | main function             |                |
| {}       | code block                |                |
| //       | comment for code          |                |
| printf   | standard output to screen |                |
| \n       | new line                  |                |
| return 0 | function return value     |                |
| gcc      | compile c source file     | must install   |
| ./       | linux execute command     | 755 permission |
| a.out    | object of c source file   |                |

### CJK and etc
On the earth planet, we have nearly 200 countries, though English is wide spreaded, people do have dialect and corresponding characters.
Let's translate "Hello World" into other human languages, in a table,

| lang | char        |
|------|-------------|
| ENG  | Hello World |
| CHN  | 你好        |
| JPN  | こんにちわ   |
| KRN  | 안녕하세요   |
| ARB  | السلام عليكم|

```
# cat a.c
#include <stdio.h>
int main()
{
     // Displays the string inside quotations
     printf("你好\n");
     return 0;
}
# ./a.out 
你好

# cat a.c
#include <stdio.h>
int main()
{
     // Displays the string inside quotations
     printf("السلام عليكم\n");
     return 0;
}
# ./a.out 
السلام عليكم
```

### scanf
sometimes we want to type-in to interact with program, 
```
$ cat a.c
#include <stdio.h>

int main()
{
     char s[2]; //buffer size is 2
     printf("you type request: ");
     scanf("%s", s);  
     printf("program response: %s\n", s);
     return 0;
}
$ gcc a.c
$ ./a.out
you type request: hi
program response: hi
```

We use two `char` to hold `hi`, `%s` indicates it's *string*.
If we go over the buffer size, we would fail.
```
you type request: seventh
program response: seventh
*** stack smashing detected ***: terminated
Aborted
```

In fact, `%s` is composite of `%c`, 
```
# cat a.c
#include <stdio.h>

int main()
{
     char s[13]; //buffer size is 13
     printf("program  input: ");
     scanf(" %c", &s[0]);
     scanf( "%c", &s[1]);
     scanf( "%c", &s[2]);
     scanf( "%c", &s[3]);
     scanf( "%c", &s[4]);
     scanf( "%c", &s[5]);
     scanf( "%c", &s[6]);
     scanf( "%c", &s[7]);
     scanf( "%c", &s[8]);
     scanf( "%c", &s[9]);
     scanf( "%c", &s[10]);
     scanf( "%c", &s[11]);
     scanf( "%c", &s[12]);
     printf("program output: ");
     printf("%c%c%c%c%c%c%c%c%c%c%c%c%c\n", s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8], s[9], s[10], s[11], s[12]); 
     printf("program  index: 12345678901234\n");
     return 0;
}
# gcc a.c
# ./a.out
program  input: whylifeisfunny
program output: whylifeisfunn
program  index: 12345678901234
```

`%c` indicates print as *char*, and we see the `y` is cut off, since the buffer size is not *14*.

Let's try understand deeper with *GDB*.
```
# ./a.out
program  input: 从入门到放弃
program output: 从入门到�
program  index: 12345678901234
```

What happens on question mark? We may recompile the code as below,
```
# cat a.c
#include <stdio.h>

int main()
{
     char s[13];
     printf("program  input: ");
     scanf(" %s", s);
     printf("program output: %s\n", s);
     printf("program  index: 12345678901234\n");
     return 0;
}
# gcc -g a.c
# gdb a.out
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from a.out...
(gdb) b main
Breakpoint 1 at 0x11a9: file a.c, line 4.
(gdb) r
Starting program: /root/a.out 

Breakpoint 1, main () at a.c:4
4       {
(gdb) n
6            printf("program  input: ");
(gdb) 
7            scanf(" %s", s);
(gdb) 
program  input: 从入门到放弃
8            printf("program output: %s\n", s);
(gdb) l
3       int main()
4       {
5            char s[13];
6            printf("program  input: ");
7            scanf(" %s", s);
8            printf("program output: %s\n", s);
9            printf("program  index: 12345678901234\n");
10           return 0;
11      }
(gdb) p s
$1 = "从入门", <incomplete sequence \346>
(gdb) x/32cb s
0x7fffffffe44b: -28 '\344'      -69 '\273'      -114 '\216'     -27 '\345'      -123 '\205'-91 '\245'       -23 '\351'      -105 '\227'
0x7fffffffe453: -88 '\250'      -27 '\345'      -120 '\210'     -80 '\260'      -26 '\346' -108 '\224'      -66 '\276'      -27 '\345'
0x7fffffffe45b: -68 '\274'      -125 '\203'     0 '\000'        5 '\005'        -101 '\233'0 '\000' 0 '\000'        0 '\000'
0x7fffffffe463: 0 '\000'        0 '\000'        0 '\000'        0 '\000'        0 '\000'   -77 '\263'       96 '`'  -33 '\337'
(gdb) x/32xb s
0x7fffffffe44b: 0xe4    0xbb    0x8e    0xe5    0x85    0xa5    0xe9    0x97
0x7fffffffe453: 0xa8    0xe5    0x88    0xb0    0xe6    0x94    0xbe    0xe5
0x7fffffffe45b: 0xbc    0x83    0x00    0x05    0x9b    0x00    0x00    0x00
0x7fffffffe463: 0x00    0x00    0x00    0x00    0x00    0xb3    0x60    0xdf
(gdb) quit
A debugging session is active.

        Inferior 1 [process 172376] will be killed.

Quit anyway? (y or n) y
```
We see the string is cut off due to *incomplete*. 346 goes to *thirteenth* bytes, which is the last index of the buffer. `\346` is octal, 230 in decimal. GB2312 uses two bytes while utf-8 uses three, so we know we can only print out `从入门到`, the next byte would be a black question mark, which follows specials definition (U+FFFD � REPLACEMENT CHARACTER used to replace an unknown, unrecognized, or unrepresentable character).

We can also confirm the utf-chars via below code,
```
# cat b.c
#include<stdio.h>
#include<string.h>
int main(){
    char a[5];
    //strcpy(a,"从");
    strcpy(a,"门");
    printf("%d %d %d\n",(char)a[0],(char)a[1],(char)a[2]); // decimal
    printf("%X %X %X\n",(unsigned char)a[0],(unsigned char)a[1],(unsigned char)a[2]); //hex
    printf("%o %o %o\n",(unsigned char)a[0],(unsigned char)a[1],(unsigned char)a[2]); //oct
    return 0;
}
```

Finally, we rewrite the code to have enough space to print out,
```
# cat a.c
#include <stdio.h>

int main()
{
     char s[19]; //buffer size increased to 19
     printf("program  input: ");
     scanf(" %s", s);
     printf("program output: %s\n", s);
     printf("program  index: 12345678901234567890\n");
     return 0;
}
```

We find the buffer size cubersome, maybe there is a dynamic way?
```
# cat e.c
#include <stdio.h>

int main(void)
{
    char *foo; 
    scanf("%ms", &foo);
//    scanf("%s", &foo);
    printf("%s\n", foo);
    printf("1234567890         0         0         0         0         0         0\n");
}
# ./a.out
YourimplementationmustincludeanasynchronousinterfacebetweenthecoreandtheDSUtoplevel
YourimplementationmustincludeanasynchronousinterfacebetweenthecoreandtheDSUtoplevel
1234567890         0         0         0         0         0         0
```

<a href="#top">Back to top</a>
