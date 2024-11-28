### Main

[C2][Main](index.md)😃.

### 8 in data type

|  1   | 2  |  3   | 4  |
|  ----  | ----  |  ----  | ----  |
| auto  | <span style="color:green">double</span> | <span style="color:green">long</span> | switch |
| break  | else | register | typedef |
| case  | enum | return  | <span style="color:green">unsigned</span> |
| <span style="color:green">char</span> | extern | <span style="color:green">short</span> | union |
| const  | <span style="color:green">float</span> | <span style="color:green">signed</span> | void |
| continue  | for | sizeof | volatile |
| default  | goto | while | static | 
| do  | if | <span style="color:green">int</span> | struct |

The basic data type used up 8 keywords, thus left 24. See previous [Number](Number.md) for detail.

### 4 in storage
|  type  | storage  |  default<br>value  | scope | life<br>cycle  |
|  ----  | ----  |  ----  | ----  | ----  |
| auto |  stack  |  garbage  | within block  | end of block  |
| extern |  data segment  |  zero  | global multiple files  | till end of the program  |
| static |  data segment  |  zero  | within block  | till end of the program  |
| register |  cpu  |  garbage  | within block  | end of block  |

The storage type used up 4 keywords, thus left 20.

*auto* is a kind of long long ago story, now we hardly ever use it, since all variables in functions are local by default now.

| type | default value |
|  ----  | ----  |
| int |	0 |
| char	| '\0' |
| float	| 0 |
| double	| 0 |
| pointer	|NULL|

*extern* variables can be declared number of times but defined only once. 
Definition of a variable is when the variable is created and the memory for it is allocated.
Declaration of a variable just tells the compiler that this variable exists. It does not allocate any memory.
In practice it should be used in our header files and then we will include the necessary headers. We should avoid the usage of exern keyword in our source files.
*static* variables remain in memory while the program is running. A normal or auto variable is destroyed when a function call where the variable was declared is over. 
```
# cat a.c
#include<stdio.h>
int fun()
{
  static int count = 0;
  count++;
  return count;
}
  
int main()
{
  printf("%d ", fun());
  printf("%d ", fun());
  return 0;
}
# ./a.out
1 2
```
*register* variables tell the compiler to store the variable in CPU register instead of memory. Frequently used variables are kept in registers and they have faster accessibility. We can never get the addresses of these variables.

### 1 sizeof
return a typedef value *size_t* depends on the compiler, 32 bit for unsigned int and 64 bit for unsigned long long. 
```
#include <stdio.h>
   
int main()
{
    printf("Size of char   =  %ld \n", sizeof(char));
    printf("Size of int    =  %ld \n", sizeof(int));
    printf("Size of float  =  %ld \n", sizeof(float));
    printf("Size of double =  %ld \n\n", sizeof(double));
   
    printf("Size of short int      =  %ld \n", sizeof(short int));
    printf("Size of long int       =  %ld \n", sizeof(long int));
    printf("Size of long long int  =  %ld \n", sizeof(long long int));
    printf("Size of long double    =  %ld \n", sizeof(long double));
   
    return 0;
}
```

### 1 union
*union* is a derived type, like 🦎. That is to say, its size equal to the max size of its member.

```
# cat a.c
#include <stdio.h>
union Job {
   float salary;
   int workerNo;
} j;

int main() {
   j.salary = 12.3;
   printf("sizeof = %ld\n", sizeof(j));
   printf("Salary = %.1f\n", j.salary);
   // when j.workerNo is assigned a value,
   // j.salary will no longer hold 12.3
   j.workerNo = 100;
   printf("sizeof = %ld\n", sizeof(j));
   printf("Salary = %.1f\n", j.salary);
   printf("Number of workers = %d", j.workerNo);
   return 0;
}
# ./a.out
sizeof = 4
Salary = 12.3
sizeof = 4
Salary = 0.0
Number of workers = 100
```

### 1 struct
Sometimes we need compound data type, and in hardware we care more about alignment.

```
# cat a.c
#include <stdio.h>
#include <string.h>
 
/*  Below structure1 and structure2 are same. 
    They differ only in member's allignment */
 
struct structure1 
{
       int id1;
       int id2;
       char name;
       char c;
       float percentage;
};
 
struct structure2 
{
       int id1;
       char name;
       int id2;
       char c;
       float percentage;                      
};
 
int main() 
{
    struct structure1 a;
    struct structure2 b;
 
    printf("size of structure1 in bytes : %d\n", 
            sizeof(a));
    printf ( "\n   Address of id1        = %u", &a.id1 );
    printf ( "\n   Address of id2        = %u", &a.id2 );
    printf ( "\n   Address of name       = %u", &a.name );
    printf ( "\n   Address of c          = %u", &a.c );
    printf ( "\n   Address of percentage = %u", &a.percentage );
 
    printf("   \n\nsize of structure2 in bytes : %d\n",
                   sizeof(b));
    printf ( "\n   Address of id1        = %u", &b.id1 );
    printf ( "\n   Address of name       = %u", &b.name );
    printf ( "\n   Address of id2        = %u", &b.id2 );
    printf ( "\n   Address of c          = %u", &b.c );
    printf ( "\n   Address of percentage = %u", &b.percentage );
    getchar();
    return 0;
}
# ./a.out
size of structure1 in bytes : 16

   Address of id1        = 1761339056
   Address of id2        = 1761339060
   Address of name       = 1761339064
   Address of c          = 1761339065
   Address of percentage = 1761339068   

size of structure2 in bytes : 20

   Address of id1        = 1761339072
   Address of name       = 1761339076
   Address of id2        = 1761339080
   Address of c          = 1761339084
   Address of percentage = 1761339088
```
The result is different from math, `int id1 + char name + int id2 + char c + float percentage = 4 + 1 + 4 + 1 + 4 = 14 bytes`, due to structure padding, that is, in order to align the data in memory,  one or more empty bytes (addresses) are inserted (or left empty) between memory addresses which are allocated for other structure members while memory allocation. 
The above example has a step of *8 bytes*, thus 64-bit platform. For structure1, `int, int, char, char, empty(2), float`, and structre2, `int, char, empty(3), int, char, empty(3), float`.

use `pack` to get 14 bytes,
```
#include <stdio.h>
#include <string.h>
 
/*  Below structure1 and structure2 are same. 
    They differ only in member's allignment */
 
#pragma pack(1)
struct structure1 
{
       int id1;
       int id2;
       char name;
       char c;
       float percentage;
};
 
struct structure2 
{
       int id1;
       char name;
       int id2;
       char c;
       float percentage;                      
};
 
int main() 
{
    struct structure1 a;
    struct structure2 b;
 
    printf("size of structure1 in bytes : %d\n",
                   sizeof(a));
    printf ( "\n   Address of id1        = %u", &a.id1 );
    printf ( "\n   Address of id2        = %u", &a.id2 );
    printf ( "\n   Address of name       = %u", &a.name );
    printf ( "\n   Address of c          = %u", &a.c );
    printf ( "\n   Address of percentage = %u", 
                   &a.percentage );
 
    printf("   \n\nsize of structure2 in bytes : %d\n", 
                   sizeof(b));
    printf ( "\n   Address of id1        = %u", &b.id1 );
    printf ( "\n   Address of name       = %u", &b.name );
    printf ( "\n   Address of id2        = %u", &b.id2 );
    printf ( "\n   Address of c          = %u", &b.c );
    printf ( "\n   Address of percentage = %u", 
                   &b.percentage );
    getchar();
    return 0;
}
```

### 1 enum
An easier way to number sequnce in a group(😶at this point, left 16).

### 1 typedef
An alias for an existing type
```
### single type
typedef signed char           S8;
typedef char                  U8;
typedef short                 S16;
typedef unsigned short        U16;
typedef int                   S32;
typedef unsigned int          U32;
typedef long long             S64;
typedef unsigned long long    U64;
### compound type
int main(int argc, char* argv[])
{

typedef struct Student{
int age;

char s;

}Stum1;

typedef struct{
int age;

char s;

}Stum2;

struct Studentm3{
int age;

char s;
};

typedef struct Studentm3 Stum3;

typedef struct Student4{
int age;

char s;
};
//struct Student4 alice;

typedef struct Student5{
int age;

char s;

}*Stum5;
//Stum5 bob;
//bob->age;
### pointer type
typedef int *p1;//p1 myp1
typedef int* p2;//p2 myp2
typedef int (*p3)[3];//p3 myp3
typedef int p4[4][4];//p4 myp4
### function type
typedef int f1(void);//f1 myf1(void) => return int
typedef int (*f2)(void);//f2 myf2(void) => return int*
### function hook
typedef struct Student
{
	char *name;
　　　　int id;
　　　　void (*fun)(int a);
}Stu;

void showid(int i){
	return &i;
}

void main(){
	int a = 1;
	Stu alice;
	alice.fun = &showid;
	alice.fun(a);
}
```

### 1 define
this is another way for alias, but it is pre-processed in terms of compilation.

```
### show in color
#define SHOW(debug, var, val) \
   if (debug == "d") \
        { printf("SHOW: \033[1;32m%s = %d\033[0m;\n", var, val); } \
   if (debug == "s") \
        { printf("SHOW: \033[1;34m%s = %s\033[0m;\n", var, val); } \
   if (debug == "p") \
        { printf("SHOW: \033[1;36m%s = %p\033[0m;\n", var, val); }
	
SHOW ("d", "a", 32);
SHOW ("s", "b", "cc");
### show in level
#include <stdio.h>
#define filename(x) strrchr(x,'/')?strrchr(x,'/')+1:x

int log_lvl;//maybe extern so all files can use

#define LOG(lvl, ...) \
        if(lvl <= log_lvl) { \
            switch(lvl) { \
                case 0: \
                              fprintf(stderr, "[%s:%s:%d][err]: ", filename(__FILE__), __FUNCTION__, __LINE__); \
                break; \
                case 1: \
                              fprintf(stderr, "[%s:%s:%d][ntc]: ", filename(__FILE__), __FUNCTION__, __LINE__); \
                break; \
                case 2: \
                              fprintf(stderr, "[%s:%s:%d][inf]: ", filename(__FILE__), __FUNCTION__, __LINE__); \
                break; \
                case 3: \
                              fprintf(stderr, "[%s:%s:%d][dbg]: ", filename(__FILE__), __FUNCTION__, __LINE__); \
                break; \
                default: \
                         fprintf(stderr, "[%s:%s:%d][def]: ", filename(__FILE__), __FUNCTION__, __LINE__); \
                break; \
            } \
            fprintf(stderr, __VA_ARGS__); \
            fprintf(stderr, "\n"); \
        } \

int
main (int argc, const char *argv[])
{
        log_lvl = 1;
        //info and debug will not print

        LOG(0, "this is error");
        LOG(1, "this is notice");
        LOG(2, "this is info");
        LOG(3, "this is debug");

  return 0;
}
```

### 1 volatile
Read variable value from address every time. GCC optimization can not skip.

### 1 const
Force variable to be unchangeable.
|     |     |     |     |
| --- | --- | --- | --- |
| **Type** | **Declaration** | **pointer value change **<br><br>**(  *ptr = 100 ) ** | **pointing value change **<br><br>**(  ptr  = &a)** |
| **1) Pointer to Variable** | **int * ptr** | **     yes** | **   yes** |
| **2) Pointer to Constant ** | * **const int * ptr**<br>* **int const * ptr** | **  no** | **   yes** |
| **3) Constant Pointer to Variable** | **int * const ptr** | ** yes** | ** no** |
| **4) Constant Pointer to Constant ** | **const int * const ptr** | **no** | ** no** |

### 1 void
A type, not a value(😶at this point, left 12).
```
void printCompanyInfo()
{
    printf("====================\n");
    printf("Company **************\n");
    printf("Company Id ******************\n");
    printf("Contact information: \n");
    printf("address *********************\n");
    printf("Phone ****************** \n");
    printf("Fax ****************** \n");
    printf("Email ****************** \n");
    printf("====================\n");
}
```

### 11 flow
Data, address and control are the basic logic of a CPU. 11 keywords(switch break else case continue default goto while for do if) are use to describe the *control* part.

### 1 return
[Function](Function.md) needs this to notify program result(more non-keywords coming below).

### null
0x0, usually for [pointer](Pointer.md).

### label:
Notify a step point, usually with `goto`.

<a href="#top">Back to top</a>
