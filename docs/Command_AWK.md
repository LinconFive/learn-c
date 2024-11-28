### Main

[C2][Main](index.md)😃.

### convert log into html

```
# same file
sed -i '1i\<pre>' scpcss0
sed -i '$a</pre>' scpcss0

# new file
sed '1i\<pre>' scpcss0 > output.html
sed -i '$a</pre>' output.html
```

### print row and column number
```
awk '{print NF}' a.txt
awk '{print NR}' a.txt
```

### file show
```
ls -lh | awk '{ print $1, $5, $9 }'

```

### first column
```
awk '{print $1}' a.txt

cut -d' ' -f1 a.txt

```

### last column
```
awk '{print $NF}' a.txt

awk '{for(i=1;i<=NF-1;i++) printf $i" "; print ""}'
```

### count line number
```
awk '{count++;print $0;} END{print "user count is ", count}' /etc/passwd

# same as less -N
```

### select cell
```
# row 5 col 3
awk 'FNR == 5 {print $3}'
```

### add line number
```

awk 'BEGIN {count=0;} {count=count+1;print count, $0;} END{}' a.txt


cat -n a.txt


```

### add column number
```
awk 'BEGIN {count=0;} {count=count+1;print count, $0;} END{}' a.txt
```

### convert into pre or code
```
awk 'BEGIN {count=0;print "<pre>"} {count=count+1;print count, $0;} END{print "</pre>"}' a.txt

<pre>
1 -rw-rw-r-- 1 zzx zzx        0 Apr  2 01:50 a.txt
2 -rw-r--r-- 1 zzx zzx      216 Nov 11  2007 AUTHORS
3 -rw-r--r-- 1 zzx zzx   232308 Jul  7  2019 ChangeLog
4 -rw-r--r-- 1 zzx zzx    17987 Jun 15  2006 COPYING
5 -rw-r--r-- 1 zzx zzx      504 Dec 24  2017 documentation.html
6 -rw-r--r-- 1 zzx zzx      914 Mar 26 12:31 fidentify.8
7 -rwxr-xr-x 1 zzx zzx  4136336 Mar 26 12:32 fidentify_static
8 drwxr-xr-x 4 zzx zzx     4096 Mar 26 12:23 icons
9 -rw-r--r-- 1 zzx zzx      117 Feb 25  2021 INFO
10 drwxr-xr-x 2 zzx zzx     4096 Mar 26 12:23 jni
11 drwxr-xr-x 2 zzx zzx     4096 Mar 26 12:32 l
12 -rw-r--r-- 1 zzx zzx    19633 Jul  7  2019 NEWS
13 -rw-r--r-- 1 zzx zzx     1171 Mar 26 12:31 photorec.8
14 -rwxr-xr-x 1 zzx zzx 10826368 Mar 26 12:32 photorec_static
15 -rw-r--r-- 1 zzx zzx     2256 Feb 24  2016 README_dev_photorec.txt
16 -rw-r--r-- 1 zzx zzx     2191 Mar 26 10:52 README.md
17 -rw-r--r-- 1 zzx zzx      301 Jul  7  2019 readme.txt
18 -rw-r--r-- 1 zzx zzx     1756 Mar 26 12:31 testdisk.8
19 -rwxr-xr-x 1 zzx zzx  8636688 Mar 26 12:32 testdisk_static
20 -rw-r--r-- 1 zzx zzx      344 Apr 21  2008 THANKS
21 -rw-r--r-- 1 zzx zzx       40 Mar 26 12:32 VERSION
</pre>

```


### add column header
```
awk 'BEGIN{print "1          2   3   4        5   6  7     8 9"}; {print}; END{print "END"}' a.txt

ls -l | awk 'BEGIN{print "1          2   3   4        5   6  7     8 9"}; {print}; END{print "END"}'
```

### add row and column header
```
ls -l | awk 'BEGIN{print "1          2   3   4        5   6  7     8 9"}; {print}; END{print "END"}' | cat -n
ls -l | awk 'BEGIN{print "1          2   3   4        5   6  7     8 9"}; {print}; END{print "END"}' | less -N
```

### first line and last line
```
xxd a.out | awk 'NR==1{print}'
xxd a.out | awk 'END{print}'
```
<a href="#top">Back to top</a>
