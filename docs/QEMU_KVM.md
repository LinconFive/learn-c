### Main

[C2][Main](index.md)😃.

### X86

qemu-system means FS(not SE) mode.
```
/usr/bin/qemu-img
/usr/bin/qemu-io
/usr/bin/qemu-nbd
/usr/bin/qemu-pr-helper
/usr/bin/qemu-system-i386
/usr/bin/qemu-system-x86_64
/usr/bin/qemu-system-x86_64-spice
```

### ISO

For arch linux iso, one can go to [archlinux](http://mirrors.163.com/archlinux/iso/2022.01.01/)
and using 512M RAM to boot, remember to enable gtk or x11,
```
qemu-system-x86_64 -boot d -cdrom archlinux.iso -m 512
```

or 

```
qemu-system-x86_64 -drive format=raw,media=cdrom,readonly,file=archlinux-2022.01.01-x86_64.iso -nographic
```

and if we want use disk, we need to create ourselves,
```
qemu-img create mydisk.img 10G
qemu-system-x86_64 -boot d -cdrom image.iso -m 512 -hda mydisk.img
```

The structure for fs is as below,


### IMG

For an ubuntu img, one can go to [img](http://cloud-images.ubuntu.com/daily/server/daily/server/minimal/releases/) :wink:
```
apt install qemu-img
qemu-img convert -f raw -O qcow2 image.img image.qcow2
qemu-img info image.qcow2
```

| Image format | Argument to qemu-img |
|----------|----------|
| QCOW2 (KVM, Xen) | qcow2 |
| QED (KVM) | qed |
| raw | raw |
| VDI (VirtualBox) | vdi |
| VHD (Hyper-V) | vpc |
| VMDK (VMware) | vmdk |


### [hello world](Kernel.md#hw)
the most simple fs, only one command and looping... 

### busybox

```
wget https://busybox.net/downloads/busybox-1.32.1.tar.bz2
tar -jxvf busybox-1.32.1.tar.bz2
# select static binary
make menuconfig
make -j20
make install
# now you should have _install

cd _install/
sudo mkdir dev 
sudo mknod dev/console c 5 1
sudo mknod dev/ram b 1 0 
sudo touch init
sudo vi init # copy below

#!/bin/sh
echo "INIT SCRIPT"
mkdir /proc
mkdir /sys
mount -t proc none /proc
mount -t sysfs none /sys
mkdir /tmp
mount -t tmpfs none /tmp
echo -e "\nThis boot took $(cut -d' ' -f1 /proc/uptime) seconds\n"
exec /bin/sh

sudo chmod +x init

find . -print0 | cpio --null -ov --format=newc | gzip -9 > initramfs-busybox-x64.cpio.gz

```

and with bzImage run

```
qemu-system-x86_64 -s \
    -kernel bzImage  \
    -initrd initramfs-busybox-x64.cpio.gz \
    --append "nokaslr root=/dev/ram init=/init"
```

### Parameter
| para  | note  |
| --- | --- |
| -M vexpress-a9 | machine: vexpress-a9 |
| -m 512M | dram=512MB |
| -cpu cortex-a9 | cpu arch=a9 |
| -smp n | cpu number, def=1 |
| -kernel ./zImage | image |
| -dtb ./vexpress-vap-ca9.dtb | device tree |
| -append cmdline | linux kernel options |
| -initrd file | use file for booting ram disk |
| -nographic | disable graph |
| -sd rootfs.ext3 | sd=rootfs.ext3 |
| -net nic | network |
| -net nic -net tap | bridge between host and guest |

### ARM 

qemu-system means FS(not SE) mode.

- ubuntu iso
```
apt-get install qemu-system-arm
apt-get install qemu-efi-aarch64
apt-get install qemu-utils

dd if=/dev/zero of=flash1.img bs=1M count=64
dd if=/dev/zero of=flash0.img bs=1M count=64
dd if=/usr/share/qemu-efi-aarch64/QEMU_EFI.fd of=flash0.img conv=notrunc

wget http://ports.ubuntu.com/ubuntu-ports/dists/bionic-updates/main/installer-arm64/current/images/netboot/mini.iso

qemu-img create ubuntu-image.img 20G

# 
qemu-system-aarch64 -nographic -machine virt,gic-version=max -m 512M -cpu max -smp 4 \
-netdev user,id=vnet,hostfwd=:127.0.0.1:0-:22 -device virtio-net-pci,netdev=vnet \
-drive file=ubuntu-image.img,if=none,id=drive0,cache=writeback -device virtio-blk,drive=drive0,bootindex=0 \
-drive file=mini.iso,if=none,id=drive1,cache=writeback -device virtio-blk,drive=drive1,bootindex=1 \
-drive file=flash0.img,format=raw,if=pflash -drive file=flash1.img,format=raw,if=pflash

# run
qemu-system-aarch64 -nographic -machine virt,gic-version=max -m 512M -cpu max -smp 4 \
-netdev user,id=vnet,hostfwd=:127.0.0.1:0-:22 -device virtio-net-pci,netdev=vnet \
-drive file=ubuntu-image.img,if=none,id=drive0,cache=writeback -device virtio-blk,drive=drive0,bootindex=0 \
-drive file=flash0.img,format=raw,if=pflash -drive file=flash1.img,format=raw,if=pflash 
```

- ubuntu img in terms of cpio
```
# Image: MS-DOS executable PE32+ executable (EFI application) Aarch64 (stripped to external PDB), for MS Windows
# make from kernel

# ramdisk-busybox.img: ASCII cpio archive (SVR4 with no CRC)
# make via cpio
mkdir rootfs
# cat rootfs/d.c
#include <stdio.h>
#include <time.h>
#define KNRM  "\x1B[0m"
#define KRED  "\x1B[31m"
#define KGRN  "\x1B[32m"
#define KYEL  "\x1B[33m"
#define KBLU  "\x1B[34m"
#define KMAG  "\x1B[35m"
#define KCYN  "\x1B[36m"
#define KWHT  "\x1B[37m"

int main(void)
{

        long i = 100000000L;
        clock_t start, finish;
        double duration;

        printf("Seconds used: ", i);

        start = clock();
        while(i--);
        finish = clock();
        duration = (double)(finish - start) / CLOCKS_PER_SEC;
        //printf("\e[1;34m%f\e[0m seconds\n", duration);
        printf("%s%f%s\n", KRED, duration, KNRM);
        //while(1);
//how to mimic key stroke? so we can exit qemu...
        
}

g++ -static -ggdb3 -O0 -std=c++11 -Wall -Wextra -pedantic -o init d.c

echo init | cpio -o --format=newc > ../ramdisk.img

# stdio to enable Ctrl+c
qemu-system-aarch64 -machine virt -cpu cortex-a57 -machine type=virt -m 100 -smp 2 -kernel Image -initrd ramdisk.img --append "rdinit=/init console=ttyAMA0" -nographic -serial mon:stdio



# 
Image  ramdisk.img

rootfs:
d.c  init  t.c
```

- ubuntu img in terms of raw

😅 suffer
```
qemu-system-aarch64 -M virt -cpu cortex-a57 -m 100 -smp 2 -kernel Image -drive format=raw,file=rootfs.ext -append "rdinit=/init console=ttyAMA0" -nographic 
```


- aarch64 (kernel)[https://github.com/freedomtan/aarch64-bare-metal-qemu/tree/2ae937a2b106b43bfca49eec49359b3e30eac1b1]
- arm(32bit)
```
$ cat startup.s
volatile unsigned int * const UART0DR = (unsigned int *)0x101f1000;
 
void print_uart0(const char *s) {
 while(*s != '\0') { /* Loop until end of string */
 *UART0DR = (unsigned int)(*s); /* Transmit char */
 s++; /* Next char */
 }
}
 
void c_entry() {
 print_uart0("Hello world!\n");
}

$ cat test.ld
ENTRY(_Reset)
SECTIONS
{
 . = 0x10000;
 .startup . : { startup.o(.text) }
 .text : { *(.text) }
 .data : { *(.data) }
 .bss : { *(.bss COMMON) }
 . = ALIGN(8);
 . = . + 0x1000; /* 4kB of stack memory */
 stack_top = .;
}

$ cat test.c
volatile unsigned int * const UART0DR = (unsigned int *)0x101f1000;
 
void print_uart0(const char *s) {
 while(*s != '\0') { /* Loop until end of string */
 *UART0DR = (unsigned int)(*s); /* Transmit char */
 s++; /* Next char */
 }
}
 
void c_entry() {
 print_uart0("Hello world!\n");
}

arm-none-eabi-gcc -c -mcpu=arm926ej-s -g test.c -o test.o
arm-none-eabi-ld -T test.ld test.o startup.o -o test.elf
arm-none-eabi-objcopy -O binary test.elf test.bin
qemu-system-arm -M versatilepb -m 128M -nographic -kernel test.bin

# file test.bin
data
```

<a href="#top">Back to top</a>
