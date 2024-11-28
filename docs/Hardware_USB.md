### Main

[C2][Main](index.md)😃.

### USB format
```
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0    7:0    0     4K  1 loop /snap/bare/5
loop1    7:1    0 373.8M  1 loop /snap/anbox/186
loop2    7:2    0 110.7M  1 loop /snap/core/12821
loop3    7:3    0  55.5M  1 loop /snap/core18/2284
loop4    7:4    0  55.5M  1 loop /snap/core18/2344
loop5    7:5    0  61.9M  1 loop /snap/core20/1376
loop6    7:6    0  61.9M  1 loop /snap/core20/1405
loop7    7:7    0 248.8M  1 loop /snap/gnome-3-38-2004/99
loop8    7:8    0   9.6M  1 loop /snap/kubeadm/2417
loop9    7:9    0  65.2M  1 loop /snap/gtk-common-themes/1519
loop10   7:10   0  10.3M  1 loop /snap/kubectl/2373
loop11   7:11   0 247.9M  1 loop /snap/gnome-3-38-2004/87
loop12   7:12   0  67.9M  1 loop /snap/lxd/22526
loop13   7:13   0  67.8M  1 loop /snap/lxd/22753
loop14   7:14   0  54.2M  1 loop /snap/snap-store/558
loop15   7:15   0  43.6M  1 loop /snap/snapd/14978
loop16   7:16   0  43.6M  1 loop /snap/snapd/15177
loop17   7:17   0 310.8M  1 loop
sda      8:0    0   5.5T  0 disk
├─sda1   8:1    0  1001M  0 part
├─sda2   8:2    0  68.3G  0 part /boot
├─sda3   8:3    0   560G  0 part
└─sda4   8:4    0   4.8T  0 part /
sdb      8:16   1  28.8G  0 disk

# delete usb
sudo dd if=/dev/zero of=/dev/sdb
# clean usb
sfdisk --delete /dev/sdb

wipefs -a /dev/sdb

sudo fdisk /dev/sdb
# label type
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty DOS partition table
   s   create a new empty Sun partition table

Command (m for help): F
Unpartitioned space /dev/sdb: 28.84 GiB, 30942947328 bytes, 60435444 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

Start      End  Sectors  Size
 2048 60437491 60435444 28.8G

Command (m for help): p
Disk /dev/sdb: 28.84 GiB, 30943995904 bytes, 60437492 sectors
Disk model: DataTraveler 3.0
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd16aecce

# create partition
sudo fdisk /dev/sdb
Command (m for help): n
Partition number (1-128, default 1): 1
First sector (34-30310366, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-30310366, default 30310366): 40956
Partition number (1-128, default 1): 2
First sector (34-30310366, default 2048): 40960
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-30310366, default 30310366): 30605311

Command (m for help): w
Changes will be confirmed.

sudo parted /dev/sdb

# fat16
sduo mkfs.vfat  -n "SDP" -F 16 /dev/sdb1

sudo fdisk -l /dev/sdb

lsusb -t
```

<a href="#top">Back to top</a>

