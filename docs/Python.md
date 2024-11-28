### try in a loop, print ok only

```
def a_function():
    return 1/0;
def b_function():
    return 1;
def c_function():
    return ["a" + 1];

functions = [a_function, b_function, c_function]
for f in functions:
    try:
        if f():
            print(f.__name__, "\033[95m{}\033[00m".format("is ok"))
    except Exception as exception:
        #print(f.__name__, "\033[91m{}\033[00m".format(exception))
        pass
```

### license to continue, here is "geek"
```
for _ in range(3):
  
    user_input = input(" Please enter your lucky word or type 'END' to terminate loop: ")
      
    if user_input == "geek":
        print("You are really a geek")
        break
  
    elif user_input == "END":
        break
  
    else:
        print("Try Again")
```

### break a list loop
```
functions = [-1,0,1,2,3]
for f in functions:
    try:
        if f == 1:
            print(f, "\033[95m{}\033[00m".format("is ok"))
            break
    except Exception as exception:
        #print(f.__name__, "\033[91m{}\033[00m".format(exception))
        pass
```

### break a generator
```
def fl(n):
    for x in range(n):
        yield x

for f in fl(10):
    try:
        if f == 5:
            print(f, "\033[95m{}\033[00m".format("is ok"))
            break
        else:
            print(f, "\033[91m{}\033[00m".format("is not ok"))
    except Exception as exception:
        print(f, "\033[92m{}\033[00m".format(exception))
```

### go over the generator to show only the ok
```
def fl(n):
    for x in range(n):
        yield x

for f in fl(10):
    try:
        if f == 5:
            print(f, "\033[95m{}\033[00m".format("is ok"))
        else:
            print(f, "\033[91m{}\033[00m".format("is not ok"))
    except Exception as exception:
        print(f, "\033[92m{}\033[00m".format(exception))
```

### bit print
```
# python a.py
from __future__ import print_function

import sys
from time import sleep

fp = sys.stdout
x = '11000000000000000000000000000000000000000000000110'

for i in range(len(x))[::-1]:
    print('%02d' % i, end='')
print()
for i in x:
    print(i.rjust(2), end='')
print()
fp.flush()
#sleep(5)

# python3 a.py
x = '11000000000000000000000000000000000000000000000110'.zfill(64)

for i in range(len(x))[::-1]:
    print('%02d' % i, end='')
print()
for i in x:
    print(i.rjust(2), end='')
print()
```

### printout with matched patter
```
with open('a', 'r') as f:
    lines = f.readlines()
    for line in lines:
        #print(repr(line))
        #if not line.endswith('0b0 \n'):
        #  print(line[0:3], line[-7:-2])
        i, j = 1, int(line[2])
        t = line[-7:-2]
        if t == "00000": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RSVD"))
        if t == "00001": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-I"))
        if t == "00010": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-D"))
        if t == "00011": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RSVD"))
        if t == "00100": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIB"))
        if t == "00101": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIB_ESAM"))
        if t == "00110": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIA"))
        if t == "00111": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIA_ESAM"))
        if t == "01000": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "HN-T"))
        if t == "01001": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "HN-I"))
        if t == "01010": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "HN-D"))
        if t == "01011": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "HN-P"))
        if t == "01100": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "SN-F_CHIC"))
        if t == "01101": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "SBSX"))
        if t == "01110": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "HN-F"))
        if t == "01111": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "SN-F_CHIE"))
        if t == "10000": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "SN-F_CHID"))
        if t == "10001": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "CXHA"))
        if t == "10010": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "CXRA"))
        if t == "10011": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "CXRH"))
        if t == "10100": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHID"))
        if t == "10101": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHID_ESAM"))
        if t == "10110": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIC"))
        if t == "10111": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIC_ESAM"))
        if t == "11000": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIE"))
        if t == "11001": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RN-F_CHIE_ESAM"))
        if t == "11010": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RSVD"))
        if t == "11011": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RSVD"))
        if t == "11100": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "MTSX"))
        if t == "11101": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "HN-V"))
        if t == "11110": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "CCG"))
        if t == "11111": print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, "RSVD"))
```

### open log and split the id out

```
['RN-SAM ID', '4']
['RN-SAM ID', '68']
['RN-SAM ID', '260']
['RN-SAM ID', '322']
['RN-SAM ID', '320']
['RN-SAM ID', '12']
['RN-SAM ID', '8']
['RN-SAM ID', '76']
['RN-SAM ID', '140']
['RN-SAM ID', '332']
['RN-SAM ID', '328']
['RN-SAM ID', '20']
['RN-SAM ID', '16']
['RN-SAM ID', '336']
['RN-SAM ID', '28']
['RN-SAM ID', '24']
['RN-SAM ID', '344']
['RN-SAM ID', '356']
['RN-SAM ID', '355']
['RN-SAM ID', '354']
['RN-SAM ID', '352']
['RN-SAM ID', '44']
['RN-SAM ID', '104']
['RN-SAM ID', '300']
['RN-SAM ID', '296']
['RN-SAM ID', '360']

a = []
with open('scpcss0', 'rb') as f:
    s= f.read().decode('ISO-8859-1')
    l = re.findall(r"RN-SAM ID:\d+", s)
    for i in l:
        #print(i.split(":")[1])
        #sort
        a.append(int(i.split(":")[1]))

for i in sorted(a):
    print(i)
```

### split binary into group
```
g = lambda x,y: list(range(64)[::-1])[63-x:63-y+1]
g(63, 48)
[63, 62, 61, 60, 59, 58, 57, 56, 55, 54, 53, 52, 51, 50, 49, 48]


XP35.POR_MXP_NODE_INFO   0b11000000000010001100000001011010000000000000000110

63626160595857565554535251504948474645444342414039383736353433323130292827262524232221201918171615141312111009080706050403020100
 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 0 0 0 0 0 0 0 0 0 0 1 0 0 0 1 1 0 0 0 0 0 0 0 1 0 1 1 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 0

31302928272625242322212019181716
 0 0 0 0 0 0 0 1 0 1 1 0 1 0 0 0
XP35 0000     000101      101000
 0 1 2 3 4 5 6 7 8 9101112131415
151413121110 9 8 7 6 5 4 3 2 1 0 
# 31-16 is posx, posy, nodeid
x, y = 31, 16
a = []
with open('a', 'r') as f:
    s = f.readlines()
    for i in s:
        l = i.split()
        a.append(tuple((l[0].split('.')[0].rjust(4), l[1][2:].zfill(64)[63-x:63-y+1])))
        
for i in sorted(a, key=lambda tup: (tup[0])):
    print(i[0], i[1])
    print(i[1][0:7], int(i[1][7:10], 2), int(i[1][10:13], 2), i[1][13:])

 
```

<a href="#top">Back to top</a>
