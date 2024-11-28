### Main

[C2][Main](index.md)😃.

### ARM

|bin|hex|axf|
|---|---|---|
|data|data|data|
||addr|addr|
|||debug|

### ISO vs IMG

(ISO)[http://cdimage.ubuntu.com/ubuntu/releases/18.04/release/ubuntu-18.04.6-server-arm64.iso] format is used to create CD and DVD images.

*As of Ubuntu 12.04, the .iso file sizes are now over 700 MB, which means you cannot even burn it to CD any more. You would have to waste a whole 4 GB blank DVD instead—all the more reason to "burn" to USB instead.*

```
# file ubuntu-18.04.6-server-arm64.iso
ubuntu-18.04.6-server-arm64.iso: DOS/MBR boot sector; partition 1 : ID=0xcd, start-CHS (0x0,2,1), end-CHS (0x3e3,33,24), startsector 64, 2038776 sectors; partition 2 : ID=0xef, start-CHS (0x3e3,33,25), end-CHS (0x3e4,11,24), startsector 2038840, 1344 sectors 

# fdisk -l ubuntu-18.04.6-server-arm64.iso
Disk ubuntu-18.04.6-server-arm64.iso: 996.2 MiB, 1044574208 bytes, 2040184 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

Device                           Boot   Start     End Sectors   Size Id Type
ubuntu-18.04.6-server-arm64.iso1           64 2038839 2038776 995.5M cd unknown
ubuntu-18.04.6-server-arm64.iso2      2038840 2040183    1344   672K ef EFI (FAT-12/16/32)

# show iso
sudo mount ~/aarch64.iso ~/openimg/
sudo umount ~/openimg/
```


(IMG) format is used to create hard disk image files.

- (ubuntu)[wget http://cloud-images.ubuntu.com/daily/server/daily/server/minimal/releases/bionic/release-20220325/ubuntu-18.04-minimal-cloudimg-amd64.img]
```
# show img
sudo mount -o loop,offset=1049kB  /home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/grub-busybox.img ~/openimg
sudo umount ~/openimg

```

- (busybox)[build myself]


- goal
mbr + rootfs
```
$ fdisk grub-busybox-arm64.img

Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk grub-busybox-arm64.img: 17 MiB, 17825792 bytes, 34816 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: A61F2B17-AE1F-4E83-BA0E-90EC5817A4F0

Device                  Start   End Sectors    Size Type
grub-busybox-arm64.img1  2048  4094    2047 1023.5K Microsoft basic data
grub-busybox-arm64.img2  4096 32766   28671     14M Linux filesystem

Command (m for help): q
```

|do|cmd|
|----|----|
|free dev|sudo losetup -f|
|list all dev|sudo losetup -l|
|remove dev|sudo losetup -d /dev/loop18|
||sudo dmsetup remove /dev/mapper/loop18p1|
||sudo dmsetup remove /dev/mapper/loop18p2|
|show all|sudo losetup -a|
||sudo fdisk -l | sed -e '/Disk \/dev\/loop/,+5d'|
|show img|sudo mount -o loop,offset=1048576  /home/zzx/armimg/ubuntu.img ~/openimg/|
||sudo umount ~/openimg/|


```

# unit must B
# offset is the B value
node2:~/fvp/rdn2-cfg1$ sudo parted
GNU Parted 3.3
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) help
  align-check TYPE N                       check partition N for TYPE(min|opt) alignment
  help [COMMAND]                           print general help, or help on COMMAND
  mklabel,mktable LABEL-TYPE               create a new disklabel (partition table)
  mkpart PART-TYPE [FS-TYPE] START END     make a partition
  name NUMBER NAME                         name partition NUMBER as NAME
  print [devices|free|list,all|NUMBER]     display the partition table, available devices, free space, all found partitions, or a particular partition
  quit                                     exit program
  rescue START END                         rescue a lost partition near START and END
  resizepart NUMBER END                    resize partition NUMBER
  rm NUMBER                                delete partition NUMBER
  select DEVICE                            choose the device to edit
  disk_set FLAG STATE                      change the FLAG on selected device
  disk_toggle [FLAG]                       toggle the state of FLAG on selected device
  set NUMBER FLAG STATE                    change the FLAG on partition NUMBER
  toggle [NUMBER [FLAG]]                   toggle the state of FLAG on partition NUMBER
  unit UNIT                                set the default unit to UNIT
  version                                  display the version number and copyright information of GNU Parted
(parted) unit B
(parted) print /home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/grub-busybox.img
Model: DELL PERC H310 (scsi)
Disk /dev/sda: 6000069312512B
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start          End             Size            File system     Name  Flags
 1      1048576B       1050673151B     1049624576B                           bios_grub
 2      1050673152B    74392272895B    73341599744B    ext4
 3      74392272896B   675687694335B   601295421440B   linux-swap(v1)        swap
 4      675687694336B  5953343717375B  5277656023040B  ext4

  align-check TYPE N                       check partition N for TYPE(min|opt) alignment
  help [COMMAND]                           print general help, or help on COMMAND
  mklabel,mktable LABEL-TYPE               create a new disklabel (partition table)
  mkpart PART-TYPE [FS-TYPE] START END     make a partition
  name NUMBER NAME                         name partition NUMBER as NAME
  print [devices|free|list,all|NUMBER]     display the partition table, available devices, free space, all found partitions, or a particular partition
  quit                                     exit program
  rescue START END                         rescue a lost partition near START and END
  resizepart NUMBER END                    resize partition NUMBER
  rm NUMBER                                delete partition NUMBER
  select DEVICE                            choose the device to edit
  disk_set FLAG STATE                      change the FLAG on selected device
  disk_toggle [FLAG]                       toggle the state of FLAG on selected device
  set NUMBER FLAG STATE                    change the FLAG on partition NUMBER
  toggle [NUMBER [FLAG]]                   toggle the state of FLAG on partition NUMBER
  unit UNIT                                set the default unit to UNIT
  version                                  display the version number and copyright information of GNU Parted
(parted) q
node2:~/fvp/rdn2-cfg1$ sudo mount -o loop,offset=1048576  /home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/grub-busybox.img ~/openimg/
node2:~/fvp/rdn2-cfg1$ ls !$
ls ~/openimg/
EFI/  grub/
node2:~/fvp/rdn2-cfg1$ sudo umount !$
sudo umount ~/openimg/

```

<a href="#top">Back to top</a>
