### Main

[C2][Main](index.md)😃.

### Perf Flame

```
git clone https://github.com/brendangregg/FlameGraph.git
```

Take anbox as example
```
robox@ubuntu:~/robox_920_huawei/script/download/robox/binaryFiles$ ./robox -v stop 10
./robox: line 40: $ROBOXALLLOG: ambiguous redirect
: @@@ args vnc
./robox: line 40: $ROBOXALLLOG: ambiguous redirect
: @@@ args stop
10: Attempting to stop instance instance10
10: STOPPING Docker
./robox: line 196: 108288 Killed                  taskset -c 8-14 anbox session-manager --run-multiple=10 --standalone --experimental --single-window --window-size=720,1280

10: NOT stopping Session Manager, it's not running
robox@ubuntu:~/robox_920_huawei/script/download/robox/binaryFiles$ ./robox -v start 10
./robox: line 40: $ROBOXALLLOG: ambiguous redirect
: @@@ args vnc
./robox: line 40: $ROBOXALLLOG: ambiguous redirect
: @@@ args start
10: Attempting to start instance instance10
10: @@ start frame
10: 10: STARTING Frame Buffer
10: The Xorg service has been created
10: @@ start session
10: STARTING Session Manager
taskset -c 8-14 anbox session-manager --run-multiple=10 --standalone --experimental --single-window --window-size=720,1280
10: @@ start configurenetwork
10: CREATING network configuration (using 172.17.0.14)
10: @@ start container
10: @@@ STARTING Docker container
10: @@@ add vnc start
10: STARTING VNC Server 000
10: STARTING VNC Server
10: FAILED to start the VNC Server
10: @@@ STARTING Launcher 000
robox@ubuntu:~/robox_920_huawei/script/download/robox/binaryFiles$ ps aux | grep start
root       2071  0.0  0.0 100016 16296 ?        Ssl  Feb24   0:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-triggers
robox     16638  0.0  0.0  14724 11488 pts/8    T    09:56   0:00 /bin/bash ./robox -v start 1
robox     20778  0.0  0.0  14724 11548 pts/8    T    09:57   0:00 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-09AM -x 2
robox     21431  0.0  0.0   6528  3188 pts/8    T    09:57   0:00 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-09AM -x 3
robox     31766  0.0  0.0 455648 11396 ?        Sl   Feb24   0:00 /usr/bin/pulseaudio --start --log-target=syslog
robox     44961  0.0  0.0  14724 11488 pts/8    T    10:01   0:00 /bin/bash ./robox -v start 1
robox     47168  0.9  0.0   6528  3256 pts/8    T    10:02   0:05 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-10AM -x 4
root      89723  0.0  0.0  14724 11352 pts/8    T    10:08   0:00 /bin/bash ./robox -v start 1
root      97549  0.0  0.0  14724 11328 pts/8    S+   10:09   0:00 /bin/bash ./robox -v start 1
root      97594  0.0  0.0   6528  1800 pts/8    S+   10:09   0:00 /bin/bash ./robox -v start 1
root      99474  0.0  0.0  14724 11352 pts/8    S+   10:09   0:00 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-10AM -x 1
root     103463  0.0  0.0  14724 11384 pts/8    S+   10:10   0:00 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-10AM -x 2
root     107784  0.1  0.0  14724 11376 pts/8    S+   10:10   0:00 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-10AM -x 3
root     112578  0.2  0.0  14724 11284 pts/8    S+   10:11   0:00 /bin/bash ./robox -v start 1 -lf /tmp/robox/1/2022-02-26-10AM -x 4
robox    113913  0.0  0.0   6528  1868 pts/2    S    10:11   0:00 /bin/bash ./robox -v start 10
robox    114817  0.0  0.0   5668   644 pts/2    S+   10:11   0:00 grep --color=auto start
robox@ubuntu:~/robox_920_huawei/script/download/robox/binaryFiles$ pstree -p 113913
robox(113913)───anbox(113914)─┬─{anbox}(113926)
                              ├─{anbox}(113927)
                              ├─{anbox}(113934)
                              ├─{anbox}(113935)
                              ├─{anbox}(113936)
                              ├─{anbox}(113937)
                              ├─{anbox}(113938)
                              ├─{anbox}(113939)
                              ├─{anbox}(113940)
                              ├─{anbox}(113941)
                              ├─{anbox}(113942)
                              ├─{anbox}(114495)
                              ├─{anbox}(114496)
                              ├─{anbox}(115227)
                              ├─{anbox}(115228)
                              ├─{anbox}(115234)
                              ├─{anbox}(115235)
                              ├─{anbox}(115338)
                              ├─{anbox}(115339)
                              ├─{anbox}(115721)
                              ├─{anbox}(115722)
                              ├─{anbox}(115749)
                              ├─{anbox}(115750)
                              ├─{anbox}(115756)
                              ├─{anbox}(115757)
                              ├─{anbox}(115936)
                              ├─{anbox}(115937)
                              ├─{anbox}(116038)
                              └─{anbox}(116039)

sudo perf record -F 99 -p 113913 -g -- sleep 30
sudo perf script | FlameGraph-master/stackcollapse-perf.pl | FlameGraph-master/flamegraph.pl > process.svg
```

### CPU Time
- CPU time is a true measure of processor/memory performance
- Performance of processor/memory = 1 / CPU time 
~~~
CPU time = CPU clock cycles x clock cycle time = CPU clock cycles / clock rate(is given in Hz (=1/sec), i.e. 2.6GHz)

CPU clock cycles = (instructions/program) x (clock cycles/instruction) = Instruction count x CPI

CPU time = Instruction count x CPI / clock rate(i.e. 256739 * 0.66 / 2.6E9 = 0.00006517 = 0.6ms)


~~~

### CPI

- From instruction to CPI
~~~
Assume that a benchmark has 100 instructions:
25 instructions are loads/stores (each take 2 cycles)
50 instructions are adds (each takes 1 cycle)
25 instructions are square root (each takes 50 cycles)

What is the CPI for this benchmark?
CPI = ((0.25 * 2) + (0.50 * 1) + (0.25 * 50)) = 13.5
~~~

- Composite CPI: CPI = Σ CPI x F

|Op|F|CPI|CPIxF|%Time|
|---|---|---|---|---|
|ALU|50%|1|.5|23%|
|Load|20%|5|1.0|45%|
|Store|10%|3|.3|14%|
|Branch|20%|2|.4|18%|
|Total|100%|2.2|100%|


### Benchmark

- 508
```
# modify try1.cfg to have static linked via gcc before gem5 start

../bin/runcpu --config=try1.cfg --action=build 508.namd_r

# remove previous build if not first time
rm -rf benchspec/CPU/508.namd_r/build;
rm -rf benchspec/CPU/508.namd_r/run;
rm -rf benchspec/CPU/508.namd_r/exe;

# ls
ls ../benchspec/CPU/508.namd_r/build && ls ../benchspec/CPU/508.namd_r/run ; ls ../benchspec/CPU/508.namd_r/exe ; echo "Good"


```

- 509
```
../bin/runcpu --config=try1.cfg --action=build 519.lbm_r
```

- bench table

|Rate and Speed|Total 43|Language|Line Count/1000|APP|
|----|----|----|----|----|
| SPECrate®2017 Integer	| SPECspeed®2017 Integer | Language |	KLOC | Application Area |
|500.perlbench_r|600.perbench_s|C|362|Per interpreter|
|502.gcc_r|602.gcc_s|C|1304|GNU C compiler|
|505.mcf_r|605.mcf_s|C|3|Route planing|
|520.omnetpp_r|620.omnetpp_s|C++|134|Discrete Devent simulation - computer network|
|523.xalancbmk_r|623.xalancbmk_s|C++|520|XML to HTML conversion via XSLT|
|525.X264_r|625.X264_s|C|C|96|Video compression|
|531.deepsjeng_r|631.deepsjeng_s|C++|10|Artificial Intelligence: alpha-beta tree search(Chess)|
|541.leela_r|641.leela_s|C++|21|Artificial Intelligence: Monte Carlo tree search(Go)|
|548.exchage2_r|648.exchange2_s|Fortran|1|Artificial Intelligence: recursive solution generator(Sudoku)|
|557.xz_r|657.xz_s|C|33|General data compression|
| SPECrate®2017 Floating Point	| SPECspeed®2017 Floating Point | Language |	KLOC | Application Area |
|503.bwaves_r|603.bwaves_s|Fortran|1|Explosion modeling|
|508.namd_r||C++|8|Molecular dynamics|
|510.parest_r||C++|427|Biomedical imaging: optical tomography with finite elements|
|511.povray_r||C++, C|170|Ray tracing|
|519.lbm_r|619.lbm_s|C|1|Fluid dynamics|
|521.wrf_r|621.wrf_s|Fortran, C|991|Weather forecasting|
|526.blender_r||C++, C|1577|3D rendering and animation|
|527.cam4_r|627.cam4_s|Fortran, C|407|Atmosphere modeling|
||628.pop2_s|Fortran, C|338|Wide-scale ocean modeling (climate level)|
|538.imagick_r|638.imagick_s|C|259|Image manipulation|
|544.nab_r|644.nab_s|C|24|Molecular dynamics|
|549.fotonik3d_r|649.fotonik3d_s|Fortran|14|Computational Electromagnetics|
|554.roms_r|654.roms_s|Fortran|210|Regional ocean modeling|

install all,
```
sudo apt-get install gfortran, gcc, g++
 ../bin/runcpu --config=ysemi.cfg all -i test &
Locating benchmarks...found 47 benchmarks in 53 benchsets.
Reading config file '/home/zzx/cpu2017/config/ysemi.cfg'
4 configurations selected:

 Action    Run Mode   Workload      Report Type      Benchmarks
--------   --------   --------   -----------------   --------------------------
validate   rate       test       SPECrate2017_fp     fprate                    
validate   speed      test       SPECspeed2017_fp    fpspeed                   
validate   rate       test       SPECrate2017_int    intrate                   
validate   speed      test       SPECspeed2017_int   intspeed                  
-------------------------------------------------------------------------------
503-554
Benchmarks selected: 503.bwaves_r, 507.cactuBSSN_r, 508.namd_r, 510.parest_r, 511.povray_r, 519.lbm_r, 521.wrf_r, 526.blender_r, 527.cam4_r, 538.imagick_r, 544.nab_r, 549.fotonik3d_r, 554.roms_r, 997.specrand_fr
603-654
Benchmarks selected: 603.bwaves_s, 607.cactuBSSN_s, 619.lbm_s, 621.wrf_s, 627.cam4_s, 628.pop2_s, 638.imagick_s, 644.nab_s, 649.fotonik3d_s, 654.roms_s, 996.specrand_fs
500-557
Benchmarks selected: 500.perlbench_r, 502.gcc_r, 505.mcf_r, 520.omnetpp_r, 523.xalancbmk_r, 525.x264_r, 531.deepsjeng_r, 541.leela_r, 548.exchange2_r, 557.xz_r,  999.specrand_ir
600-657
Benchmarks selected: 600.perlbench_s, 602.gcc_s, 605.mcf_s, 620.omnetpp_s, 623.xalancbmk_s, 625.x264_s, 631.deepsjeng_s, 641.leela_s, 648.exchange2_s, 657.xz_s, 998.specrand_is

result/CPU2017.003.log ====>>>

500-557, 600-657, 503-554, 603-654
```

- app command

```
ls benchspec/CPU/  -I *.bset -I 99*

ls benchspec/CPU/  -I *.bset -I 99* -m 

for app in `ls benchspec/CPU/  -I *.bset -I 99*`; do echo $app; done

for app in `ls benchspec/CPU/  -I *.bset -I 99*`; do find benchspec/CPU/$app/build/build_base_mytest-m64.0000/ -type f -executable -print; done
test -x $f && 

find . -type f -exec file {} + | grep ELF



for app in `ls benchspec/CPU/  -I *.bset -I 99*`;
do 
    d=benchspec/CPU/$app/build/build_base_mytest-m64.0000/
    if [ -d $d ]; then
        find $d -type f -exec file {} + | grep linked, | cut -d: -f1
    fi
done

benchspec/CPU/500.perlbench_r/build/build_base_mytest-m64.0000/perlbench_r

benchspec/CPU/503.bwaves_r/build/build_base_mytest-m64.0000/bwaves_r
benchspec/CPU/505.mcf_r/build/build_base_mytest-m64.0000/mcf_r
benchspec/CPU/507.cactuBSSN_r/build/build_base_mytest-m64.0000/cactusBSSN_r
benchspec/CPU/508.namd_r/build/build_base_mytest-m64.0000/namd_r
benchspec/CPU/510.parest_r/build/build_base_mytest-m64.0000/parest_r
benchspec/CPU/511.povray_r/build/build_base_mytest-m64.0000/imagevalidate_511
benchspec/CPU/511.povray_r/build/build_base_mytest-m64.0000/povray_r
benchspec/CPU/519.lbm_r/build/build_base_mytest-m64.0000/lbm_r
benchspec/CPU/520.omnetpp_r/build/build_base_mytest-m64.0000/omnetpp_r
benchspec/CPU/521.wrf_r/build/build_base_mytest-m64.0000/wrf_r
benchspec/CPU/521.wrf_r/build/build_base_mytest-m64.0000/diffwrf_521
benchspec/CPU/523.xalancbmk_r/build/build_base_mytest-m64.0000/cpuxalan_r
benchspec/CPU/525.x264_r/build/build_base_mytest-m64.0000/x264_r
benchspec/CPU/525.x264_r/build/build_base_mytest-m64.0000/imagevalidate_525
benchspec/CPU/525.x264_r/build/build_base_mytest-m64.0000/ldecod_r
benchspec/CPU/526.blender_r/build/build_base_mytest-m64.0000/blender_r
benchspec/CPU/526.blender_r/build/build_base_mytest-m64.0000/imagevalidate_526
benchspec/CPU/527.cam4_r/build/build_base_mytest-m64.0000/cam4_validate_527
benchspec/CPU/527.cam4_r/build/build_base_mytest-m64.0000/cam4_r
benchspec/CPU/531.deepsjeng_r/build/build_base_mytest-m64.0000/deepsjeng_r
benchspec/CPU/538.imagick_r/build/build_base_mytest-m64.0000/imagevalidate_538
benchspec/CPU/538.imagick_r/build/build_base_mytest-m64.0000/imagick_r
benchspec/CPU/541.leela_r/build/build_base_mytest-m64.0000/leela_r
benchspec/CPU/544.nab_r/build/build_base_mytest-m64.0000/nab_r
benchspec/CPU/548.exchange2_r/build/build_base_mytest-m64.0000/exchange2_r
benchspec/CPU/549.fotonik3d_r/build/build_base_mytest-m64.0000/fotonik3d_r
benchspec/CPU/554.roms_r/build/build_base_mytest-m64.0000/roms_r
benchspec/CPU/557.xz_r/build/build_base_mytest-m64.0000/xz_r
benchspec/CPU/600.perlbench_s/build/build_base_mytest-m64.0000/perlbench_s
benchspec/CPU/603.bwaves_s/build/build_base_mytest-m64.0000/speed_bwaves
benchspec/CPU/605.mcf_s/build/build_base_mytest-m64.0000/mcf_s
benchspec/CPU/607.cactuBSSN_s/build/build_base_mytest-m64.0000/cactuBSSN_s
benchspec/CPU/619.lbm_s/build/build_base_mytest-m64.0000/lbm_s
benchspec/CPU/620.omnetpp_s/build/build_base_mytest-m64.0000/omnetpp_s
benchspec/CPU/621.wrf_s/build/build_base_mytest-m64.0000/wrf_s
benchspec/CPU/621.wrf_s/build/build_base_mytest-m64.0000/diffwrf_621
benchspec/CPU/627.cam4_s/build/build_base_mytest-m64.0000/cam4_validate_627
benchspec/CPU/627.cam4_s/build/build_base_mytest-m64.0000/cam4_s
benchspec/CPU/628.pop2_s/build/build_base_mytest-m64.0000/speed_pop2
benchspec/CPU/638.imagick_s/build/build_base_mytest-m64.0000/imagick_s
benchspec/CPU/638.imagick_s/build/build_base_mytest-m64.0000/imagevalidate_638
benchspec/CPU/644.nab_s/build/build_base_mytest-m64.0000/nab_s
benchspec/CPU/649.fotonik3d_s/build/build_base_mytest-m64.0000/fotonik3d_s
benchspec/CPU/654.roms_s/build/build_base_mytest-m64.0000/sroms

for app in `ls benchspec/CPU/  -I *.bset -I 99*`;
do 
    d=benchspec/CPU/$app/build/build_base_mytest-m64.0000/
    
    if [ -d $d ]; then
        a=`find $d -type f -exec file {} + | grep linked, | cut -d: -f1`
        #[ -n "$a" ] && c="$a"" ""--help | head -n 1" && $c
        [ -n "$a" ] && c="$a"" ""--help" && echo $c
    fi
done

```

|APP|FIX|
|---|---|
|benchspec/CPU/502.gcc_r/build/build_base_mytest-m64.0000/cpugcc_r|--std=gnu89|
|benchspec/CPU/625.x264_s/build/build_base_mytest-m64.0000/x264_s|--std=c99|

../bin/runcpu --config=ysemi.cfg intspeed -i test
../bin/runcpu --config=ysemi.cfg 625.x264_s -i test 
../bin/runcpu --config=ysemi.cfg 657.xz_s -i test

<a href="SPEC2017.md">SPEC2017</a>

### Perf Metric
- execution time : time to do the task
- throughput : number of tasks completed per unit time

||Instruction|CPI|Clock Freq|
|---|---|---|---|
|Program|x|||
|Compiler|x|x||
|Arch|x|x||
|Orgnization||x|x|
|Tech|||x|

### Profile
```
gcc -pg a.c -o a
./a
gprof a ./gmon.out > gmon.txt
cat gmon.txt

```

### Self Cal
```
#define _GNU_SOURCE
#include <asm/unistd.h>
#include <linux/perf_event.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/ioctl.h>
#include <unistd.h>

#include <inttypes.h>
#include <sys/types.h>

#include <time.h>
static long
perf_event_open(struct perf_event_attr *hw_event, pid_t pid,
                int cpu, int group_fd, unsigned long flags)
{
    int ret;

    ret = syscall(__NR_perf_event_open, hw_event, pid, cpu,
                    group_fd, flags);
    return ret;
}


int
main(int argc, char **argv)
{
    struct perf_event_attr pe;
    long long count;
    int fd;

    uint64_t n;
    if (argc > 1) {
        n = strtoll(argv[1], NULL, 0);
    } else {
        n = 10000;
    }

    memset(&pe, 0, sizeof(struct perf_event_attr));
    pe.type = PERF_TYPE_HARDWARE;
    pe.size = sizeof(struct perf_event_attr);
    pe.config = PERF_COUNT_HW_INSTRUCTIONS;
    pe.disabled = 1;
    pe.exclude_kernel = 1;
    // Don't count hypervisor events.
    pe.exclude_hv = 1;

    fd = perf_event_open(&pe, 0, -1, -1, 0);
    if (fd == -1) {
        fprintf(stderr, "Error opening leader %llx\n", pe.config);
        exit(EXIT_FAILURE);
    }

    ioctl(fd, PERF_EVENT_IOC_RESET, 0);
    ioctl(fd, PERF_EVENT_IOC_ENABLE, 0);

clock_t start, finish;
double duration;


start = clock();
    long i = 100000000L;
    while(i--);
    /* Loop n times, should be good enough for -O0.
    __asm__ (
        "1:;\n"
        "sub $1, %[n];\n"
        "jne 1b;\n"
        : [n] "+r" (n)
        :
        :
    );
    */

finish = clock();
duration = (double)(finish - start) / CLOCKS_PER_SEC;
printf("%.7f seconds\n", duration);
    ioctl(fd, PERF_EVENT_IOC_DISABLE, 0);
    read(fd, &count, sizeof(long long));

    printf("%lld instructions\n", count);

    close(fd);
}

g++ -static -ggdb3 -O0 -std=c++11 -Wall -Wextra -pedantic -o a.out a.c
```

### TOP
~~~
Help for Interactive Commands - procps-ng 3.3.16
Window 1:Def: Cumulative mode Off.  System: Delay 3.0 secs; Secure mode Off.

  Z,B,E,e   Global: 'Z' colors; 'B' bold; 'E'/'e' summary/task memory scale
  l,t,m     Toggle Summary: 'l' load avg; 't' task/cpu stats; 'm' memory info
  0,1,2,3,I Toggle: '0' zeros; '1/2/3' cpus or numa node views; 'I' Irix mode
  f,F,X     Fields: 'f'/'F' add/remove/order/sort; 'X' increase fixed-width

  L,&,<,> . Locate: 'L'/'&' find/again; Move sort column: '<'/'>' left/right
  R,H,J,C . Toggle: 'R' Sort; 'H' Threads; 'J' Num justify; 'C' Coordinates
  c,i,S,j . Toggle: 'c' Cmd name/line; 'i' Idle; 'S' Time; 'j' Str justify
  x,y     . Toggle highlights: 'x' sort field; 'y' running tasks
  z,b     . Toggle: 'z' color/mono; 'b' bold/reverse (only if 'x' or 'y')
  u,U,o,O . Filter by: 'u'/'U' effective/any user; 'o'/'O' other criteria
  n,#,^O  . Set: 'n'/'#' max tasks displayed; Show: Ctrl+'O' other filter(s)
  V,v     . Toggle: 'V' forest view; 'v' hide/show forest view children

  k,r       Manipulate tasks: 'k' kill; 'r' renice
  d or s    Set update interval
  W,Y       Write configuration file 'W'; Inspect other output 'Y'
  q         Quit
          ( commands shown with '.' require a visible task display window )
Press 'h' or '?' for help with Windows,
Type 'q' or <Esc> to continue
~~~

| Opt| l1 | l2 |
|----|----|----|
|FILTER|o||
|||COMMAND=sshd|
|||USER=zzx|
|SORT|N||
||M||
||P||
|COLUMN|F|<space>|
||Last CPU||
||TTY||
||||
|FIND|L||

### Robox

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

```

<a href="#top">Back to top</a>
