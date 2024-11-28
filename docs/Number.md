### Main

[C2][Main](index.md)😃.

### Small Number

C language is based on binary, or digital electrical component, that is to say, everything is a number. 
We classify the number in different size as data type, just like street block number. The minum size is one *byte*, or eight bits, or harf word.

| Data Type | Memory (bytes) | Range | Format Specifier |
|---|---|---|---|
| short int | 2   | -32,768 to 32,767 | %hd |
| unsigned short int | 2   | 0 to 65,535 | %hu |
| unsigned int | 4   | 0 to 4,294,967,295 | %u  |
| int | 4   | -2,147,483,648 to 2,147,483,647 | %d  |
| long int | 4   | -2,147,483,648 to 2,147,483,647 | %ld |
| unsigned long int | 4   | 0 to 4,294,967,295 | %lu |
| long long int | 8   | -(2^63) to (2^63)-1 | %lld |
| unsigned long long int | 8   | 0 to 18,446,744,073,709,551,615 | %llu |
| signed char | 1   | -128 to 127 | %c  |
| unsigned char | 1   | 0 to 255 | %c  |
| float | 4   | 1.175494351 E - 38 to 3.402823466 E + 38, 6 decimal places | %f  |
| double | 8   | 2.2250738585072014 E - 308 to 1.7976931348623158 E + 308, 9 decimal places | %lf |
| long double | 16  |     | %Lf |

For a particular platform, check `#include <limits.h>` and `#include <float.h>` to get more details,
```
# cat a.c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#include <float.h>

int main(int argc, char** argv) {

    printf("CHAR_BIT    :   %d\n", CHAR_BIT);
    printf("CHAR_MAX    :   %d\n", CHAR_MAX);
    printf("CHAR_MIN    :   %d\n", CHAR_MIN);
    printf("INT_MAX     :   %d\n", INT_MAX);
    printf("INT_MIN     :   %d\n", INT_MIN);
    printf("LONG_MAX    :   %ld\n", (long) LONG_MAX);
    printf("LONG_MIN    :   %ld\n", (long) LONG_MIN);
    printf("SCHAR_MAX   :   %d\n", SCHAR_MAX);
    printf("SCHAR_MIN   :   %d\n", SCHAR_MIN);
    printf("SHRT_MAX    :   %d\n", SHRT_MAX);
    printf("SHRT_MIN    :   %d\n", SHRT_MIN);
    printf("UCHAR_MAX   :   %d\n", UCHAR_MAX);
    printf("UINT_MAX    :   %u\n", (unsigned int) UINT_MAX);
    printf("ULONG_MAX   :   %lu\n", (unsigned long) ULONG_MAX);
    printf("USHRT_MAX   :   %d\n", (unsigned short) USHRT_MAX);
    printf("Storage size for float : %d \n", sizeof(float));
    printf("FLT_MAX     :   %g\n", (float) FLT_MAX);
    printf("FLT_MIN     :   %g\n", (float) FLT_MIN);
    printf("-FLT_MAX    :   %g\n", (float) -FLT_MAX);
    printf("-FLT_MIN    :   %g\n", (float) -FLT_MIN);
    printf("Precision value: %d\n", FLT_DIG );
    printf("Storage size for double : %d \n", sizeof(double));
    printf("DBL_MAX     :   %g\n", (double) DBL_MAX);
    printf("DBL_MIN     :   %g\n", (double) DBL_MIN);
    printf("-DBL_MAX     :  %g\n", (double) -DBL_MAX);
    printf("Precision value: %d\n", DBL_DIG );
    printf("Storage size for long double : %d \n", sizeof(long double));
    printf("LDBL_MAX     :   %g\n", (long double) LDBL_MAX);
    printf("LDBL_MIN     :   %g\n", (long double) LDBL_MIN);
    printf("-LDBL_MAX     :  %g\n", (long double) -LDBL_MAX);
    printf("Precision value: %d\n", LDBL_DIG );
    return 0;
}
```


### Big Number
When we build a caculator, we really need to process big number, like a billion or more, which could exceed the limits, so we have to find a way to represent it.
Take below factorial as example,
```
# cat a.c
#include <assert.h>
#include <stdio.h>

int fact(int n){
        int i;
        int p = 1;
        for (i=1; i <= n ; ++i){
                p = p * i;
        }
        return p;
}
int main(int argc, char * argv[]){
        int n;
        n = atoi(argv[1]);
        assert( n >= 0);
        /* Print the factorial */
        printf ("%d ! = %d \n", n, fact(n));
        return 1;
}
# ./a.out 17
17 ! = -288522240 
```

On Linux, we can use GMP to calculate BN.
```
# cat a.c
#include<gmp.h>
#include<stdio.h>

int main()
{
    mpz_t a, c;
    mpz_init(a);
    mpz_init(c);
    //2 to the power of 1000
    mpz_init_set_ui(a, 2);
    mpz_pow_ui(c, a, 1000);
    gmp_printf("2 to the power of 1000 = %Zd\n", c);
    mpz_clear(a);
    
    //fact of 16
    mpz_fac_ui(c, 16);
    gmp_printf("fact of 16 = %Zd\n", c);
    mpz_clear(c);
    
    return 0;
}
# gcc a.c -lgmp
# ./a.out 
2 to the power of 1000 = 10715086071862673209484250490600018105614048117055336074437503883703510511249361224931983788156958581275946729175531468251871452856923140435984577574698574803934567774824230985421074605062371141877954182153046474983581941267398767559165543946077062914571196477686542167660429831652624386837205668069376
fact of 16 = 20922789888000
```

### Matrix
Numbers can be put into homogeneous function of degree n.
In C, array is developed to hold such data, and such data is consecutive numbers, corresponding to Σ range in math.
Let's say someone buys a whole building of apartments, of which has 21 levels overall and 14 rooms in each level.
If we denote someone as `s`, then we can sort the rooms from `s[0][0]` to `s[20][13]` one by one, overall *294* rooms. Guests can occupy the room, and the room size is `short int` at this time(can be changed yet must in the previous table). So now we have a `short int s[21]14]` array, type is *short integer*. Guests, do as the locals do, must be assigned as the same type. 

```
#include <stdio.h>

int main(void)
{
        int s[21][14] = {
                {10, 11, 12, 13, 10, 11, 12, 13, 10, 11, 12, 13, 10, 11},
                {14, 15, 16, 17, 14, 15, 16, 17, 14, 15, 16, 17, 14, 15},
                {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, 
                {20, 22, 22, 23, 20, 22, 22, 23, 20, 22, 22, 23, 20, 22}
        };

        printf("+-------+------+-------+\n");
        printf("| Level | Room | Value |\n");
        printf("| %5d | %4d | %5d |\n", 0, 1, s[0][0]);
        printf("| %5d | %4d | %5d |\n", 20, 13, s[20][13]);
        printf("+-------+------+-------+\n");


        return 0;
}
```

We can use *loop* to go over the array item, 
```
#include<stdio.h>
int main(){
   /* 2D array declaration*/
   int disp[21][14];
   /*Counter variables for the loop*/
   int i, j;
   for(i=0; i<21; i++) {
      for(j=0;j<14;j++) {
         printf("Enter value for disp[%d][%d]: ", i, j);
         scanf("%d", &disp[i][j]);
      }
   }
   //Displaying array elements
   printf("Two Dimensional array elements:\n");
   for(i=0; i<21; i++) {
      for(j=0;j<14;j++) {
         printf("%5d ", disp[i][j]);
         if(j==13){
            printf("\n");
         }
      }
   }
   return 0;
}
```

There are three ways of loop, and let's take string(1D array) for example,

1. `for loop`

```
# cat f.c
#include <stdio.h>

int main()
{
     char s[13];
     s[0] = 'a';
     s[1] = 'b';
     s[2] = 'c';
     s[3] = 'd';
     s[4] = 'e';
     s[5] = 'f';
     s[6] = 'g';
     s[7] = 'h';
     s[8] = 'i';
     s[9] = 'j';
     s[10] = 'k';
     s[11] = 'l';
     s[12] = 'm';
     int i;
     for (i = 0; i < 13; i++) {
        printf("%c\n", s[i]);
     }
     return 0;
}
# ./a.out
a
b
c
d
e
f
g
h
i
j
k
l
m
```

2. `while`
```
# cat a.c
#include <stdio.h>

int main(int argc, char* argv[])
{
        char str[] = "abcdefghijklm";
        printf("%d\n", sizeof(str));
        printf("%d\n", strlen(str));
        printf("%s\n", (str));
        int i;
        while (i < 13) {
                printf("%c\n", str[i]);
                i++;
        }
        return 0;
}
# ./a.out.
14
13
abcdefghijklm
a
b
c
d
e
f
g
h
i
j
k
l
m
```

3. `do while`

```
# cat a.c
int main(int argc, char* argv[])
{
        char str[] = "abcdefghijklm";
        printf("%d\n", sizeof(str));
        printf("%d\n", strlen(str));
        printf("%s\n", (str));
        int i;
        do {
                printf("%c\n", str[i]);
                i++;
        } while(i < 13);
        return 0;
}
# ./a.out.
14
13
abcdefghijklm
a
b
c
d
e
f
g
h
i
j
k
l
m
```

### Memory
As we all know, C memory is always 1D, so how it performs 2D?
For example, a matrix summation as below,

```
# cat a.c
#include <stdio.h>
int main()
{
  float a[2][2], b[2][2], result[2][2];

  // Taking input using nested for loop
  printf("Enter elements of 1st matrix\n");
  for (int i = 0; i < 2; ++i)
    for (int j = 0; j < 2; ++j)
    {
      printf("Enter a%d%d: ", i + 1, j + 1);
      scanf("%f", &a[i][j]);
    }

  // Taking input using nested for loop
  printf("Enter elements of 2nd matrix\n");
  for (int i = 0; i < 2; ++i)
    for (int j = 0; j < 2; ++j)
    {
      printf("Enter b%d%d: ", i + 1, j + 1);
      scanf("%f", &b[i][j]);
    }

  // adding corresponding elements of two arrays
  for (int i = 0; i < 2; ++i)
    for (int j = 0; j < 2; ++j)
    {
      result[i][j] = a[i][j] + b[i][j];
    }

  // Displaying the sum
  printf("\nSum Of Matrix:\n");

  for (int i = 0; i < 2; ++i)
    for (int j = 0; j < 2; ++j)
    {
      printf("%.1f\t", result[i][j]);

      if (j == 1)
        printf("\n");
    }
  return 0;
}
# ./a.out
Enter elements of 1st matrix
Enter a11: 21
Enter a12: 23
Enter a21: 31
Enter a22: 13
Enter elements of 2nd matrix
Enter b11: 24
Enter b12: 34
Enter b21: 21
Enter b22: 12

Sum Of Matrix:
45.0    57.0
52.0    25.0
```

Add a line `printf("addr: result %x, a %x, b %x, \n", &result[i][j], &a[i][j], &b[i][j]);` to print out the address before summation, we can get,
```
addr: result d1db5290, a d1db5270, b d1db5280, 
addr: result d1db5294, a d1db5274, b d1db5284, 
addr: result d1db5298, a d1db5278, b d1db5288, 
addr: result d1db529c, a d1db527c, b d1db528c, 
```
We know `float` is *4 bytes*, and hex 0, 4, 8, c, 0 as a circle, so array `a, b, result` follows one by one, and meaningingly 1D, not 2D.

Besides, there are three ways of creating 2D array,

```
int c[2][3] = {
{1, 3, 0}, {-1, 5, 9}
};
         
int c[][3] = {
{1, 3, 0}, {-1, 5, 9}
};
                
int c[2][3] = {1, 3, 0, -1, 5, 9};
```

Furthermore, to create 3D array,
```
int test[2][3][4] = {
    {
    {3, 4, 2, 3}, {0, -3, 9, 11}, {23, 12, 23, 2}
    },
    {
    {13, 4, 56, 3}, {5, 9, 3, 5}, {3, 1, 4, 9}
    }
    };
```
and we should know its memory also in 1D.
In fact, 1D is reasonable, since C is not good to print such high dimesion, within text mode, e.g,

```
# cat a.c
#include <stdio.h>
int main()
{
  int test[2][3][2];  // C Program to store and print 12 values, in 3D array

  printf("Enter 12 values: \n");

  for (int i = 0; i < 2; ++i)
  {
    for (int j = 0; j < 3; ++j)
    {
      for (int k = 0; k < 2; ++k)
      {
        scanf("%d", &test[i][j][k]);
      }
    }
  }
  
  printf("\nDisplaying values:\n"); // Printing values with proper index, no 3D printing...

  for (int i = 0; i < 2; ++i)
  {
    for (int j = 0; j < 3; ++j)
    {
      for (int k = 0; k < 2; ++k)
      {
        printf("test[%d][%d][%d] = %d\n", i, j, k, test[i][j][k]);
      }
    }
  }

  return 0;
}
```

In other words, C can at most print line or curve, e.g,
```
# cat a.c
#include <stdio.h>
#include <math.h>

#define WIDTH 60
#define HEIGHT 20
#define X WIDTH/2
#define Y HEIGHT/2
#define XMAX WIDTH-X-1
#define XMIN -(WIDTH-X)
#define YMAX HEIGHT-Y
#define YMIN -(HEIGHT-Y)+1

char grid[HEIGHT][WIDTH];

int plot(int x, int y);
void init_grid(void);
void show_grid(void);

int main()
{
    float x,y;

    init_grid();
    for(x=-3.14159;x<=3.14159;x+=0.1)
    {
        y = sin(x);
        plot(rintf(x*10),rintf(y*8));
    }
    show_grid();

    return(0);
}

/* Set "pixel" at specific coordinates */
int plot(int x, int y)
{
    if( x > XMAX || x < XMIN || y > YMAX || y < YMIN )
        return(-1);

    grid[Y-y][X+x] = '*';
    return(1);
}

/* Initialize grid */
void init_grid(void)
{
    int x,y;

    for(y=0;y<HEIGHT;y++)
        for(x=0;x<WIDTH;x++)
            grid[y][x] = ' ';
    /* draw the axis */
    for(y=0;y<HEIGHT;y++)
        grid[y][X] = '|';
    for(x=0;x<WIDTH;x++)
        grid[Y][x] = '-';
    grid[Y][X] = '+';
}

/* display grid */
void show_grid(void)
{
    int x,y;

    for(y=0;y<HEIGHT;y++)
    {
        for(x=0;x<WIDTH;x++)
            putchar(grid[y][x]);
        putchar('\n');
    }
}

# ./a.out
                              |                             
                              |                             
                              |            *******          
                              |         ***       ***       
                              |       **             **     
                              |      *                 *    
                              |    **                   **  
                              |   *                       * 
                              |  *                         *
                              | *                           
------------------------------**----------------------------
*                            *|                             
 **                         * |                             
   *                      **  |                             
    *                    *    |                             
     **                **     |                             
       **            **       |                             
         ***       **         |                             
            *******           |                             
                              |  
```

<a href="#top">Back to top</a>


