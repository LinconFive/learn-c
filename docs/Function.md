### Main

[C2][Main](index.md)😃.

### Fork

```
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>
#include <errno.h>
#include <sys/wait.h>
int main() {
   pid_t process_id;
   int return_val = 1;
   int state;
   process_id = fork();
   if (process_id == -1) { //when process id is negative, there is an error, unable to fork
      printf("can't fork, error occured\n");
         exit(EXIT_FAILURE);
   } else if (process_id == 0) { //the child process is created
      printf("The child process is (%u)\n",getpid());
         char * argv_list[] = {"ls","-lart","/home",NULL};
      execv("ls",argv_list); // the execv() only return if error occured.
      exit(0);
   } else { //for the parent process
      printf("The parent process is (%u)\n",getppid());
      if (waitpid(process_id, &state, 0) > 0) { //wait untill the process change its state
         if (WIFEXITED(state) && !WEXITSTATUS(state))
            printf("program is executed successfully\n");
         else if (WIFEXITED(state) && WEXITSTATUS(state)) {
            if (WEXITSTATUS(state) == 127) {
               printf("Execution failed\n");
            } else
               printf("program terminated with non-zero status\n");
         } else
            printf("program didn't terminate normally\n");
      }
      else {
         printf("waitpid() function failed\n");
      }
      exit(0);
   }
   return 0;
}
```

### Stack
```
#include <execinfo.h>
#include <stdio.h>
#include <stdlib.h>

void fun1();
void fun2();
void fun3();

void print_stacktrace();

int main()
{
printf("%s is calling\n", "main");
fun3();
}

void fun1()
{
printf("%d is calling\n", 1);
printf("stackstrace begin:\n");
print_stacktrace();
}

void fun2()
{
printf("%d is calling\n", 2);
fun1();
}

void fun3()
{
printf("%d is calling\n", 3);
fun2();
}

void print_stacktrace()
{
int size = 16;
void * array[16];
int stack_num = backtrace(array, size);
char ** stacktrace = backtrace_symbols(array, stack_num);
for (int i = 0; i < stack_num; ++i)
{
printf("%s\n", stacktrace[i]);
}
free(stacktrace);
}

$ gcc b.c -rdynamic -g
$ ./a.out
main is calling
3 is calling
2 is calling
1 is calling
stackstrace begin:
./a.out(print_stacktrace+0x3f) [0x5594ba9fb2e6]
./a.out(fun1+0x34) [0x5594ba9fb24e]
./a.out(fun2+0x28) [0x5594ba9fb279]
./a.out(fun3+0x28) [0x5594ba9fb2a4]
./a.out(main+0x2a) [0x5594ba9fb213]
/lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf3) [0x7fbeb502c0b3]
./a.out(_start+0x2e) [0x5594ba9fb12e]
$ addr2line -a 0x5594ba9fb279 -e a.out -f -C
0x0000563af01ee279
??
??:0
```

<a href="#top">Back to top</a>
