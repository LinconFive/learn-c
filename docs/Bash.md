### choose position

```
$ cat ~/go.sh
#!/usr/bin/env bash

HEIGHT=15
WIDTH=40
CHOICE_HEIGHT=4
BACKTITLE="Backtitle here"
TITLE="Where to go?"
MENU="Choose one of the following options:"

OPTIONS=(1 "Gem5"
         2 "FVP"
         3 "Qemu")

CHOICE=$(dialog --clear \
                --backtitle "$BACKTITLE" \
                --title "$TITLE" \
                --menu "$MENU" \
                $HEIGHT $WIDTH $CHOICE_HEIGHT \
                "${OPTIONS[@]}" \
                2>&1 >/dev/tty)

clear
case $CHOICE in
        1)
            echo "You chose Option 1"
            cd /home/zzx/85test/gem/gem5-resources/src/spec-2017/gem5
            ;;
        2)
            echo "You chose Option 2"
            cd /home/zzx/85test/fvp/rdn2-cfg1/model-scripts/rdinfra
            ;;
        3)
            echo "You chose Option 3"
            cd /home/zzx/85test/qemu/
            ;;
esac
pwd
exec bash

```

### add into pre
```
# sh script.sh c.c c.html
exec >"$2"
cat <<HERE
<html><body><h1>Log output for: $1</h1>
<pre>
HERE
grep "${3:-^}" "$1"
echo '</pre></body></html>'
```

### do until file exists
```
until [ -f ubuntu.img ]
do
echo "ubuntu.img not found"
done
echo "ubuntu.img moved"
mv ubuntu.img ~
exit

```

### color
```
echo -e "\033[0;31mred\033[0m"
echo -e "\033[0;32mgreen\033[0m" 

Yellow='\033[0;33m'       # Yellow
Blue='\033[0;34m'         # Blue
Purple='\033[0;35m'       # Purple
Cyan='\033[0;36m'         # Cyan
White='\033[0;37m'        # White
```

### enter to continue
```
echo $script
read -p "Press enter to continue"

```

### shift to left
```
echo $1 $2
x=$1; shift
y=$1; shift
echo $x $y
```
<a href="#top">Back to top</a>
