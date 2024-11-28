### Main

[C2][Main](index.md)😃.

### Windows

| type | command | note |
| -----| ---- | ---- |
| cmd in english | chcp 43 |  
| cpu detail | wmic cpu get /format:hform > cpu.html | 
| memory detail | wmic memorychip |  
| hard disk | wmic diskdrive list full /format:hform > hdd.html |
| rsync | set PATH=C:\Users\zixia\Documents\cwrsync_5.5.0_x86_free\bin;C:\Users\zixia\Documents\cmder\vendor\git-for-windows\cmd;C:\Users\zixia\Documents\cmder\vendor\conemu-maximus5\ConEmu\Scripts;C:\Users\zixia\Documents\cmder\vendor\conemu-maximus5;C:\Users\zixia\Documents\cmder\vendor\conemu-maximus5\ConEmu;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\Program Files\PuTTY\;C:\Users\zixia\AppData\Local\Microsoft\WindowsApps;C:\Users\zixia\Documents\cmder\vendor\git-for-windows\mingw64\bin;C:\Users\zixia\Documents\cmder\vendor\git-for-windows\usr\bin;C:\Users\zixia\Documents\cmder\vendor\bin;C:\Users\zixia\Documents\cmder;<br>rsync.exe -e 'ssh -v' --progress -av mywork.tar.gz zzx@192.168.10.85:/home/zzx | 

### Linux

| type | command | note |
| -----| ---- | ---- |
| hard disk | diskpart | |
| folder sort |  du -s * \| sort -rn | |
| current path folder size|du -h --max-depth=1 .||
| total folder size |du -h --max-depth=1 folder||
| mount | mount -o loop src.iso /dst | |
| umount | umount /dst | |
| comp tar | tar -cvf xxx.tar xxx | |
| decomp tar | tar -xvf xxx.tar | |
| comp tar.gz | tar -czf xxx.tar xxx | |
| decomp tar.gz | tar -xzf xxx.tar | |
| comp xz | tar -cvf xxx.tar xxx<br>xz -z xxx.tar | |
|decomp xz | tar -Jvf xxx.tar.xz | |
| comp bz2 | tar -cjf xxx.tar.bz2 xxx | |
|decomp bz2 |  tar -xvjf xxx.tar.bz2 | |
| kill all commands |ps aux \| grep runcpu \|  awk {'print $2}' \| sudo xargs kill -9| root no need sudo|
| ls in one line | ls \| xargs| |
| ls in one column | ls -1 | |
| nc passfile | | |
| receiver | nc -l -p #port > passfile | |
| sender | nc -w 3 [receiver #ip] #port < passfile | |
|download a file|curl -O http://baidu.com/uploads/a.bb||
| grep number |ls benchspec/CPU/ \| grep -E '[0-9]{3}' \| wc -l||
| echo flash |echo -e "\033[44;37;5m flash \033[44;37;0m"||
| cd in script |. ./a.sh||
| clear content| : > a.txt ||
|show column 2 and 3|cut -d' ' -f2,3|
|make folder into html|tree -C -L 3 -T "fvpdoc" -H "index.html" -I "node_modules" --charset=gbk -o ooTree.html||
|make html|ansifilter -i css.cfg1 -H -o css.cfg1.html||
|delete a file start with dash|rm -- -H||
|newest file in folder|find . -type f -printf "%T@ %p\n" \| sort -n \| cut -d' ' -f2 \| tail -n 1||
|align via awk|sudo cat /proc/1/maps \| awk '{printf("%-35s %-s\n", $1, $6);}'||
|show files in better format|ls -l \| awk '{printf("%6s %s %2s %s %-s\n", $5, $6, $7, $8, $9);}'||
|tree all file in a folder|tree . -H "." -o tree.html||
|kill a group|ps aux \| grep FVP \| awk '{print $2}' \| xargs kill -9||
|rsync copy folder|rsync -avhW --no-compress --progress --info=progress2 src_dir dst_dir||
|rsync ssh folder|rsync -avL local_dir root@ip:/remote_dir||
|sudo rclone sync ../fvp/rdn2-cfg1 rdn2 -P -L --transfers 64|rclone|
|sudo rclone copy src dst||
|dd if=in of=out status=progress||
|pv in > out||

<!---
div class="special-class" markdown="1"
is not working...


-->


### SVN
| type | command | note |
| -----| ---- | ---- |
||svn co https://svn.internal.foo.com/svn/mycoolgame/branches/1.81||
|create your new file|||
||svn add your new file||
||svn ci -m "added file lalalalala" you new file||

### Git
| type | command | note |
| -----| ---- | ---- |
| show |  git config --list --show-origin | |
| delete after commit | git restore --source=HEAD^ --staged  -- path/*.* | :heavy_multiplication_x: |
| check tag |git clone -b vx.x.x.x https://gem5.googlesource.com/public/gem5||
| clean build |git fetch origin<br>git checkout branchname<br>git reset --hard origin/branchname<br>git  clean -d --force||
| remove all|git checkout .<br>git clean -df<br>git status|only use when no add and commit|
|           |git reset .|otherwise one must cancel add |
| current version|git rev-parse HEAD||
|                |git log -1||




### Screen
| type | command | note |
| -----| ---- | ---- |
|detach |  Ctrl + a, d | |
|rettach |  screen -r pid | for dettached |
| |  screen -rd pid | for attached |
|list |  screen -ls | |
|kill a detached|screen -S 1186535 -X quit||
|con tcp|screen //telnet 127.0.0.1 5018||
|switch|Ctrl + a, Ctrl + a||
|lock|Ctrl + a, x|need input password|
|kill current|Ctrl + a, k, y|no to cancel|
|scroll| Ctrl + a, Esc, ← ↑ → ↓ |
|log screen|Ctrl + a, H|first time start, second time stop|


### Misc
| type | command | note |
| -----| ---- | ---- |
| make to show variable |  $(warning  $(XXX)) | |
| make log | make 2>&1 | tee log | |
| gcc log | gcc a.c > log 2>&1 | |


<a href="#top">Back to top</a>
