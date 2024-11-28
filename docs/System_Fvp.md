### Main

[C2][Main](index.md)😃.

### Platform

* `FVP_Base_AEMv8A-AEMv8A-AEMv8A-AEMv8A-CCN502`
    
* `FVP_Base_AEMv8A-AEMv8A` (For certain configurations also uses 11.14/21)
    
* `FVP_Base_AEMv8A-GIC600AE`
    
* `FVP_Base_AEMvA` (For certain configurations also uses 0.0/6684)
    
* `FVP_Base_Cortex-A32x4` (Version 11.12/38)
    
* `FVP_Base_Cortex-A35x4`
    
* `FVP_Base_Cortex-A53x4`
    
* `FVP_Base_Cortex-A55x4`
    
* `FVP_Base_Cortex-A55x4+Cortex-A75x4`
    
* `FVP_Base_Cortex-A57x1-A53x1`
    
* `FVP_Base_Cortex-A57x2-A53x4`
    
* `FVP_Base_Cortex-A57x4-A53x4`
    
* `FVP_Base_Cortex-A57x4`
    
* `FVP_Base_Cortex-A65AEx8`
    
* `FVP_Base_Cortex-A65x4`
    
* `FVP_Base_Cortex-A710x4`
    
* `FVP_Base_Cortex-A72x4-A53x4`
    
* `FVP_Base_Cortex-A72x4`
    
* `FVP_Base_Cortex-A73x4-A53x4`
    
* `FVP_Base_Cortex-A73x4`
    
* `FVP_Base_Cortex-A75x4`
    
* `FVP_Base_Cortex-A76AEx4`
    
* `FVP_Base_Cortex-A76AEx8`
    
* `FVP_Base_Cortex-A76x4`
    
* `FVP_Base_Cortex-A77x4`
    
* `FVP_Base_Cortex-A78x4`
    
* `FVP_Base_Neoverse-E1x1`
    
* `FVP_Base_Neoverse-E1x2`
    
* `FVP_Base_Neoverse-E1x4`
    
* `FVP_Base_Neoverse-N1x4`
    
* 
```diff
+ FVP_Base_Neoverse-N2x4` (Version 11.12 build 38)
```
    
* `FVP_Base_Neoverse-V1x4`
    
* `FVP_Base_RevC-2xAEMvA` (For certain configurations also uses 0.0/6557)
    
* `FVP_CSS_SGI-575` (Version 11.15/26)
    
* `FVP_Morello` (Version 0.11/19)
    
* `FVP_RD_E1_edge` (Version 11.15/26)
    
* `FVP_RD_N1_edge_dual` (Version 11.15/26)
    
* `FVP_RD_N1_edge` (Version 11.15/26)
    
* `FVP_RD_V1` (Version 11.15/26)
    
* `FVP_TC0`
    
* `FVP_TC1`



### Alpine

- host
```
./build-scripts/build-test-uefi.sh -p rdn2cfg1 all
export MODEL=/home/alpine/models/Linux64_GCC-6.4/FVP_RD_N2_Cfg1

ip tuntap add dev tap0 mode tap user root
ifconfig tap0 0.0.0.0 promisc up
brctl addif virbr0 tap0

./distro.sh -p rdn2cfg1 -i /home/alpine/alpine-standard-3.15.0-aarch64.iso -s 16 -n true
ps aux grep FVP
screen //telnet 127.0.0.1 5018
```
- guest
```
ip addr add 192.168.122.22/24 dev eth0
ip link show eth0
ip link set eth0 up
 
echo "nameserver 192.168.122.1" >  /etc/resolv.conf 
echo "nameserver 8.8.8.8" >> /etc/resolv.conf
cat /etc/resolv.conf
route add default gw 192.168.122.1 eth0

cat /etc/issue

echo "http://dl-cdn.alpinelinux.org/alpine/v3.15/main" > /etc/apk/repositories
echo "http://dl-cdn.alpinelinux.org/alpine/v3.15/community" >> /etc/apk/repositories
sleep 1
echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
cat /etc/apk/repositories

sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

cat > a.c << EOF; $(echo)
#include <stdio.h>
int main()
{
  printf("Hello world\n");
}
EOF

gcc a.c

```

### Busybox

we can run busybox via . ./run.sh
```
echo $DISPLAY
export XAUTHORITY=/home/zzx/.Xauthority
xauth list
p=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra
cd $(dirname $p)
#echo `pwd`
export MODEL=/home/zzx/85test/fvp/support/Model/FVP_RD_N2_Cfg1
echo $MODEL
echo "run model now..."

./boot.sh -p rdn2cfg1 -n false -a "-C board.flash0.diagnostics=1 -a cluster0.cpu*=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/hello.axf"
```

```
# qemu-img info output/rdn2cfg1/grub-busybox.img
image: output/rdn2cfg1/grub-busybox.img
file format: raw
virtual size: 222 MiB (232783872 bytes)
disk size: 222 MiB

# file output/rdn2cfg1/grub-busybox.img 
output/rdn2cfg1/grub-busybox.img: DOS/MBR boot sector; partition 1 : ID=0xee, start-CHS (0x0,0,2), end-CHS (0x1c,76,48), startsector 1, 454655 sectors, extended partition table (last)

# fdisk -l output/rdn2cfg1/grub-busybox.img
Disk output/rdn2cfg1/grub-busybox.img: 222 MiB, 232783872 bytes, 454656 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 276A6AB2-EFB5-40AB-AA91-36CAE935643F

Device                            Start    End Sectors  Size Type
output/rdn2cfg1/grub-busybox.img1  2048  43006   40959   20M Microsoft basic dat
output/rdn2cfg1/grub-busybox.img2 43008 452606  409599  200M Linux filesystem

```

we can unpack the img
```
mkdir ramfs
sudo mount -o loop,offset=$((43008 * 512)) output/rdn2cfg1/grub-busybox.img ramfs

ls ramfs
Image  lost+found  ramdisk-busybox.img

file ramfs/ramdisk-busybox.img
ramfs/ramdisk-busybox.img: ASCII cpio archive (SVR4 with no CRC)

cpio -t -F ramfs/ramdisk-busybox.img 
dev
dev/console
dev/loop0
bin
bin/busybox
bin/sh
linuxrc
proc
sys
mnt
usr
sbin
usr/bin
usr/sbin
init
etc
usr/share
usr/share/udhcpc
usr/share/udhcpc/default.script
3837 blocks

mkdir tmp
cd tmp

# sudo cpio -i -u -I ../ramfs/ramdisk-busybox.img

sudo cpio -idmv < ../ramfs/ramdisk-busybox.img
ls
sudo find . | cpio -o -H newc > ../bb.cpio
sudo mv ramfs/ramdisk-busybox.img .
sudo mv bb.cpio ramfs/ramdisk-busybox.img
sudo umount ramfs
ls ramfs


```

Checkpoint is a faster way to boot up to a certain point, we try to use *Iris*

```
/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/../../../support/Model/FVP_RD_N2_Cfg1 --data css.scp.armcortexm7ct=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/scp_ramfw.bin@0x0BD80000 --data css.mcp.armcortexm7ct=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/mcp_ramfw.bin@0x0BF80000 -C css.mcp.ROMloader.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/mcp_romfw.bin -C css.scp.ROMloader.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/scp_romfw.bin -C css.trustedBootROMloader.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/tf-bl1.bin -C board.flashloader0.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/fip-uefi.bin -C board.flashloader1.fname=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor1_flash.img -C board.flashloader1.fnameWrite=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor1_flash.img -C board.flashloader2.fname=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor2_flash.img -C board.flashloader2.fnameWrite=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor2_flash.img -I -R -A -p --iris-port 7101 -C css.scp.pl011_uart_scp.out_file=rdn2cfg1/refinfra-948586-uart-0-scp_2022-02-08_06.39.15 -C css.scp.pl011_uart_scp.unbuffered_output=1 -C css.scp.pl011_uart_scp.uart_enable=true -C css.pl011_s_uart_ap.out_file=rdn2cfg1/refinfra-948586-uart-0-console_2022-02-08_06.39.15 -C soc.pl011_uart_mcp.out_file=rdn2cfg1/refinfra-948586-uart-0-mcp_2022-02-08_06.39.15 -C soc.pl011_uart_mcp.unbuffered_output=1 -C soc.pl011_uart0.out_file=rdn2cfg1/refinfra-948586-uart-0-armtf_2022-02-08_06.39.15 -C soc.pl011_uart0.unbuffered_output=1 -C soc.pl011_uart0.flow_ctrl_mask_en=1 -C soc.pl011_uart0.enable_dc4=0 -C soc.pl011_uart1.out_file=rdn2cfg1/refinfra-948586-uart-1-mm_2022-02-08_06.39.15 -C soc.pl011_uart1.unbuffered_output=1 -C soc.pl011_uart1.flow_ctrl_mask_en=1 -C soc.pl011_uart1.enable_dc4=0 -C css.pl011_s_uart_ap.unbuffered_output=1 -C css.gic_distributor.ITS-device-bits=20 -C pcie_group_0.pciex16.hierarchy_file_name=\<default\> -C pcie_group_0.pciex16.pcie_rc.ahci0.endpoint.ats_supported=true -C soc.nonPCIe_devices_iomacro.pl330_dma_0.p_controller_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_0.p_irq_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_0.p_periph_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_1.p_controller_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_1.p_irq_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_1.p_periph_nsecure=1 -C board.virtioblockdevice.image_path=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/grub-busybox.img -C board.flash0.diagnostics=1 -s /home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/rdn2cfg1/  --stat --log rdn2cfg1/log -o rdn2cfg1/output -C board.virtio_net.hostbridge.interfaceName=tap0 -C board.virtio_net.enabled=1 -C board.virtio_net.transport=legacy

Iris server started listening to port 7101
terminal_uart_scp: Listening for serial connection on port 5001
terminal_uart_mcp: Listening for serial connection on port 5002
INFO: cmn700:: CMN700:: Using 'CFGM_PERIPHBASE_PARAM' for periphbase address.

terminal_ns_uart_ap: Listening for serial connection on port 5003
terminal_s_uart_ap: Listening for serial connection on port 5004
iomacro_terminal_0: Listening for serial connection on port 5005
iomacro_terminal_1: Listening for serial connection on port 5006
terminal_s0: Listening for serial connection on port 5007
terminal_s1: Listening for serial connection on port 5008
terminal_mcp: Listening for serial connection on port 5009
terminal_0: Listening for serial connection on port 5010
terminal_1: Listening for serial connection on port 5011

Info: RD_N2_Cfg1: RD_N2_Cfg1.css.scp.ROMloader: FlashLoader: Loaded 44 kB from file '/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/scp_romfw.bin'

Info: RD_N2_Cfg1: RD_N2_Cfg1.css.mcp.ROMloader: FlashLoader: Loaded 43 kB from file '/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/mcp_romfw.bin'

Info: RD_N2_Cfg1: RD_N2_Cfg1.css.trustedBootROMloader: FlashLoader: Loaded 30 kB from file '/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/tf-bl1.bin'

Info: RD_N2_Cfg1: RD_N2_Cfg1.board.flashloader0: FlashLoader: Loaded 5 MB from file '/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/fip-uefi.bin'

Info: RD_N2_Cfg1: RD_N2_Cfg1.board.flashloader1: FlashLoader: Loaded 64 MB from file '/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor1_flash.img'

Info: RD_N2_Cfg1: RD_N2_Cfg1.board.flashloader2: FlashLoader: Loaded 64 MB from file '/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor2_flash.img'

Info: RD_N2_Cfg1: board.flash0: reset



cd /home/zzx/FVP_ARM_Std_Library/Iris/Python
python3
import sys
sys.path.append('/home/zzx/FVP_ARM_Std_Library/Iris/Python/iris')
import iris.debug
model = iris.debug.NetworkModel("localhost",7101)
cpulist = model.get_cpus()[2]
cpu = cpulist #cluseter 0 cpu0
cpu.get_pc()
cpu.get_instruction_count()

>>> model.host
'localhost'
>>> model.port
7101
>>> model.client.hostname
'localhost'
>>> model.client
<iris.iris.IrisTcpClient object at 0x7f23db51e940>
>>> model.client.port
7101
>>> model.client.instance
<iris.iris.IrisInstance object at 0x7f23db51ebe0>
>>> model.client.instanceName
'client.iris_debug'
>>> model.client.instId
516

# there are two types of sub-class, CPU vs NON-CPU 

cpus = model.get_cpus()
target = model.get_target(name) #this is the target name
# but we need get name via info
l = list(model.get_target_info())
# and we need to print cut-line
for i, x in enumerate(l): print(i, x.target_name)

s = []
for i in l:
    s.append(i.target_name)

set(s)
{'PVBusSlave', 'VisEventRecorder', 'MessageHandlingUnitV2', 'Kits2_PowerStateGate', 'FlashLoader', 'TZFilterUnit', 'filter1', 'PVBusExclusiveMonitor', 'ARM_Cortex-M7', 'hostbridge', 'MSIRewriter', 'SMSC_91C111', 'PCIe_Controllers', 'ahci1', 'PVBusLogger', 'RAMDevice', 'IOMACRO', 'nonPCIe_devices', 'Kits2_Timer', 'MemoryMappedGenericWatchdog', 'PVBusSecureSquasher', 'Infra5_SCP', 'Infra5_MSCP_PIK', 'IntelStrataFlashJ3', 'ahci0', 'Infra5_CSS_NIC400', 'Infra5_Board', 'WarningMemory', 'PCIeEndpoint', 'DMA330x4', 'ClockDivider', 'HostBridge', 'RD_N2_Cfg1_CSS', 'TelnetTerminal', 'MMC', 'RD_N2_Cfg1', 'Kits4_SystemIdUnit', 'Infra5_SoC', 'filter0', 'SMMUv3_FOR_PCIE', 'exerciser4', 'exerciser3', 'PL330_DMAC', 'exerciser2', 'Ashbrook_DebugUnit', 'ClockGate', 'PVBusMapper', 'PL011_Uart', 'Infra5_Debug_PIK', 'Infra_AddrTran', 'ClockTimerThread', 'VirtioNetMMIO', 'OrGate', 'Infra5_MCP', 'CounterModule', 'Cluster_ARM_Neoverse-N2', 'ahci2', 'Infra1_PLLControl', 'PL370_HDLCD', 'ClockTimerThread64', 'GICv4RedistributorInternal', 'Ashbrook_MCP_SCC', 'RandomNumberGenerator', 'LabellerForDMA330', 'NI-700', 'WideOrGate', 'SchedulerThreadEvent', 'PPUv1', 'GICv3ProtocolChecker', 'exerciser5', 'ScalableClockControl', 'Juno_sysregs', 'ARM_Neoverse-N2', 'smmuv3testengine0', 'GICv3Redistributor', 'Infra5_SCP_SCC', 'TLB', 'SP804_Timer', 'MMU_700', 'SMMUv3AEM', 'AsyncCacheFlushUnit', 'transaction_router_tracer', 'SP810_SysCtrl', 'PL031_RTC', 'Infra5_System_PIK', 'SMMUv3TestEngine', 'exerciser0', 'MemoryMappedGenericTimer', 'TestbedGPIOConnector', 'DummyAPB', 'smmuv3testengine1', 'Temperature', 'SRAM_ECC_RAS', 'CMSDK_Watchdog', 'MemoryMappedCounterModule', 'PS2Keyboard', 'PCIeBridge', 'PS2Mouse', 'MMU_500_BASE', 'GICv3CPUInterface', 'Exerciser', 'PVBusExclusiveSquasher', 'GICv3Distributor', 'NonVolatileCounter', 'TlbCadi', 'PL180_MCI', 'GICv3CPUInterfaceDecoder', 'PVCache', 'SP805_Watchdog', 'Infra5_Cluster_PIK', 'ClockSelector', None, 'PL350_SMC', 'SwitchedClockControl', 'FrequencyProbe', 'Infra4_MemoryElement_TZC400', 'PL061_GPIO', 'GICv4InterruptTranslationService', 'Ashbrook_SoC_SCC', 'TZSwitch', 'RD_MemoryElement_NoDMC', 'DisplayMemory', 'exerciser1', 'PVBusMaster', 'filter2', 'DSU', 'filter3', 'smmuv3testengine2', 'BasePlatformPCISBSA', 'PL050_KMI', 'Labeller', 'PCIeRootComplex', 'Visualisation_sdl2', 'ahci_sata', 'smmuv3testengine3', 'SchedulerThread', 'RootKeyStorage', 'Kits2_Privileged_to_Secure_Mapper', 'Juno_SoC_SOR'}

s = []
for i in l:
    s.append(i.instance_name)
    
in fact, we can print without list conversion,

['_Target__add_memspace_alias', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_disass_modes', '_event_manager', '_get_address_space', '_get_default_memory_space', '_get_register_by_name', '_handle_semihost_io', '_insert_breakpoint', '_memory_spaces_by_name', '_memory_spaces_by_number', '_stderr', '_stdin', '_stdout', '_table_info_by_name', 'add_bpt_mem', 'add_bpt_prog', 'add_bpt_reg', 'add_event_callback', 'all_registers_by_id', 'ambiguous_register_names', 'breakpoints', 'clear_bpts', 'client', 'component_type', 'description', 'disass_mode', 'disassemble', 'ec_IRIS_BREAKPOINT_HIT', 'ec_IRIS_SEMIHOSTING_INPUT_REQUEST', 'ec_IRIS_SEMIHOSTING_OUTPUT', 'executes_software', 'get_disass_modes', 'get_event_info', 'get_execution_state', 'get_hit_breakpoints', 'get_instruction_count', 'get_pc', 'get_register_info', 'get_steps', 'get_table_info', 'handle_semihost_io', 'has_register', 'has_table', 'instId', 'instName', 'instance_name', 'is_running', 'load_application', 'long_register_names', 'parameters', 'pc_info', 'pc_name_prefix', 'pc_space_id_info', 'properties', 'read_all_registers', 'read_memory', 'read_register', 'read_table', 'remove_bpt', 'remove_event_callback', 'restore_state', 'save_state', 'set_execution_state', 'set_steps', 'short_register_names', 'stderr', 'stdin', 'stdout', 'supports_disassembly', 'supports_per_inst_exec_control', 'supports_tables', 'target_name', 'write_memory', 'write_register', 'write_table']


t = model.get_targets()
for i in t:
    if (i.target_name is None):
        print(i.instName)
```

we can have suspend
|Version|Command|
|-------|-------|
|Python2 |raw_input("Press Enter to continue...")|
|Python 3|input("Press Enter to continue...")|

we can read memory
```
def seq():
    gap = 1
    start = 0x400003
    #end = 0x00200FFFFF
    end = sys.maxsize
    while start < end:
        yield start
        start = start + gap

gen = seq()
for i in gen:
    try:
        print(c.read_memory(i), hex(i))
        #break 
    except Exception as exception:
        print(exception, hex(i))
    #else:
        #with open('mem', 'w+') as f: f.write(i); f.write('\n') 

```
Load application for a certain CPU, The file can be ELF, Motorola S-Record, or in a gzip-compressed version

```
# 
$ file /home/zzx/Foundation_Platformpkg/examples/hello.axf
/home/zzx/Foundation_Platformpkg/examples/hello.axf: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, with debug_info, not stripped

cpu.load_application("/home/zzx/Foundation_Platformpkg/examples/hello.axf", loadData = None, verbose = None, parameters = None)
model.stop()
model.run()

```

try read memory, and save into file if ok

the memory is from spec, i.e. 0x00 2000 0000 - 0x00 200F FFFF is Processor block 0, 0x00_0600_0000 0x00_07FF_FFFF 32MB AP Non-secure RAM
```

def seq():
    gap = 1
    start = 0x0020000000
    end = 0x00200FFFFF
    while start < end:
        yield start
        start = start + gap

gen = seq()
for i in gen:
    try:
        c.read_memory(i)
        
    except Exception as exception:
        print(exception, hex(i))
    else:
        with open('mem', 'w+') as f: f.write(I, '\n')

```

:heavy_exclamation_mark: so what is the memory space in console now

```
# cat /proc/1/maps | awk {'print $1, $6'}
00400000-005dc000 /bin/busybox
005eb000-005ec000 /bin/busybox
005ec000-005ee000 /bin/busybox
005ee000-005f1000 
102c0000-102e2000 [heap]
ffffacf5b000-ffffacf5d000 [vvar]
ffffacf5d000-ffffacf5e000 [vdso]
ffffdf237000-ffffdf258000 [stack]

# cat /proc/meminfo
MemTotal:        8092044 kB
MemFree:         8033820 kB
MemAvailable:    7943800 kB
Buffers:               0 kB
Cached:             6864 kB
SwapCached:            0 kB
Active:                4 kB
Inactive:             56 kB
Active(anon):          4 kB
Inactive(anon):       56 kB
Active(file):          0 kB
Inactive(file):        0 kB
Unevictable:        6864 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:            60 kB
Mapped:             1220 kB
Shmem:                 0 kB
KReclaimable:       2988 kB
Slab:              18224 kB
SReclaimable:       2988 kB
SUnreclaim:        15236 kB
KernelStack:        1648 kB
PageTables:           60 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     4046020 kB
Committed_AS:        584 kB
VmallocTotal:   133143461888 kB
VmallocUsed:        2516 kB
VmallocChunk:          0 kB
Percpu:              832 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
FileHugePages:         0 kB
FilePmdMapped:         0 kB
CmaTotal:          32768 kB
CmaFree:           26480 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB

/proc/vmallocinfo
/proc/iomem
/proc/ioport
```


### RDN2

* `Cfg1`

- 8xMP1 Neoverse N2 CPUs
- CMN-700 interconnect (mesh size 3x3)
- Multiple AXI expansion ports for I/O Coherent PCIe, Ethernet, offload
- GIC-700.
- MMU-700
- Arm Cortex-M7 for System Control Processor (SCP) and Manageability Control Processor (MCP)

* `CPU vs Non-CPU`
- SoC
The SoC peripherals represent peripherals that might be added to a compute subsystem in an
SoC design. The RD-N2 SoC model is based on the Juno Arm® Development Platform (ADP).
- Board
The board peripherals represent peripherals that might be present on the board onto which
the SoC is mounted. The RD-N2 board model is based on the Juno ADP
- Interrupts

* `Cfg2` is a quad-chip, connected via CCG link through CMN700 CML feature. Each chip,

- 4xMP1 Neoverse N2 CPUs
- CMN-700 interconnect (mesh size 6x6)
- Multiple AXI expansion ports for I/O Coherent PCIe, Ethernet, offload
- GIC-700.
- MMU-700
- Arm Cortex-M7 for System Control Processor (SCP) and Manageability Control Processor (MCP)

* `RD-N2` in particular is based on the following hardware configuration.

- 32xMP1 Neoverse N2 CPUs
- CMN-700 interconnect
- Multiple AXI expansion ports for I/O Coherent PCIe, Ethernet, offload
- Arm Cortex-M7 for System Control Processor (SCP) and
- Manageability Control Processor (MCP)

The _ixed Virtual Platform_ of RD-N2 config supports 16xMP1 Neoverse N2 CPUs.

### CMN700

use iris to print reg value
```
x = model.get_target("component.RD_N2_Cfg1.css.cmn700")
r = x.short_register_names

# below gives error
for i in r.keys():  print(i, x.read_register(i))
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/zzx/FVP_ARM_Std_Library/Iris/Python/iris/debug/Target.py", line 365, in read_register
    raise ValueError('Register read failed: {}'.format(iris.error.IrisErrorCode(errorCode)))
ValueError: Register read failed: E_error_reading_write_only_resource

# to skip the error for write only
for i in r.keys():
    try:
            s = x.read_register(i)
    except Exception as exception:
            print(i, '----------')
            input("Press Enter to continue...")
    else:
            print(i, bin(s))

```

one can also save value into file for later use
```
f = open("cmn.txt", "w")
r = model.get_target("component.RD_N2_Cfg1.css.cmn700")
x = r.short_register_names.keys()
for i in x:
    try:
            s = r.read_register(i)
    except Exception as exc:
            print(i, exc)
            #input("Press Enter to continue...")
    else:
            f.write("%s \t %s \n" % (i, bin(s)))

f.close()

```

we can add more log and rebuild,
```
./build-scripts/rdinfra/build-test-busybox.sh -p rdn2cfg1 all

/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/../../../support/Model/FVP_RD_N2_Cfg1 --data css.scp.armcortexm7ct=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/scp_ramfw.bin@0x0BD80000 --data css.mcp.armcortexm7ct=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/mcp_ramfw.bin@0x0BF80000 -C css.mcp.ROMloader.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/mcp_romfw.bin -C css.scp.ROMloader.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/scp_romfw.bin -C css.trustedBootROMloader.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/tf-bl1.bin -C board.flashloader0.fname=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/rdn2cfg1/fip-uefi.bin -C board.flashloader1.fname=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor1_flash.img -C board.flashloader1.fnameWrite=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor1_flash.img -C board.flashloader2.fname=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor2_flash.img -C board.flashloader2.fnameWrite=/home/zzx/fvp/rdn2-cfg1/model-scripts/rdinfra/platforms/rdn2cfg1/nor2_flash.img -I -R -A -p --iris-port 7101 -C css.scp.pl011_uart_scp.out_file=rdn2cfg1/refinfra-uart-0-scp_rdn2cfg1 -C css.scp.pl011_uart_scp.unbuffered_output=1 -C css.scp.pl011_uart_scp.uart_enable=true -C css.pl011_s_uart_ap.out_file=rdn2cfg1/refinfra-uart-0-console_rdn2cfg1 -C soc.pl011_uart_mcp.out_file=rdn2cfg1/refinfra-uart-0-mcp_rdn2cfg1 -C soc.pl011_uart_mcp.unbuffered_output=1 -C soc.pl011_uart0.out_file=rdn2cfg1/refinfra-uart-0-armtf_rdn2cfg1 -C soc.pl011_uart0.unbuffered_output=1 -C soc.pl011_uart0.flow_ctrl_mask_en=1 -C soc.pl011_uart0.enable_dc4=0 -C soc.pl011_uart1.out_file=rdn2cfg1/refinfra-uart-1-mm_rdn2cfg1 -C soc.pl011_uart1.unbuffered_output=1 -C soc.pl011_uart1.flow_ctrl_mask_en=1 -C soc.pl011_uart1.enable_dc4=0 -C css.pl011_s_uart_ap.unbuffered_output=1 -C css.gic_distributor.ITS-device-bits=20 -C pcie_group_0.pciex16.hierarchy_file_name=\<default\> -C pcie_group_0.pciex16.pcie_rc.ahci0.endpoint.ats_supported=true -C soc.nonPCIe_devices_iomacro.pl330_dma_0.p_controller_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_0.p_irq_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_0.p_periph_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_1.p_controller_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_1.p_irq_nsecure=1 -C soc.nonPCIe_devices_iomacro.pl330_dma_1.p_periph_nsecure=1 -C board.virtioblockdevice.image_path=/home/zzx/fvp/rdn2-cfg1/output/rdn2cfg1/grub-busybox.img -C board.flash0.diagnostics=1 -s rdn2cfg1/ --stat --log rdn2cfg1/log -o rdn2cfg1/output -C board.virtio_net.hostbridge.interfaceName=tap0 -C board.virtio_net.enabled=1 -C board.virtio_net.transport=legacy -D --plugin /home/zzx/FVP_ARM_Std_Library/plugins/Linux64_GCC-6.4/GenericTrace.so -C TRACE.GenericTrace.trace-sources=FVP_RD_N2_Cfg1.scp.armcortexm7ct -C TRACE.GenericTrace.trace-sources=FVP_RD_N2_Cfg1.mcp.armcortexm7ct

# use ctrl + a + H to save log

FWK_LOG_INFO(
            MOD_NAME "XP (%d, %d) ID:%d, LID:%d",
            get_node_pos_x(xp),
            get_node_pos_y(xp),
            get_node_id(xp),
            get_node_logical_id(xp));

``` 

### Multichip

```
# build
./build-scripts/rdinfra/build-test-busybox.sh -p rdn2cfg2 all

# notice the xauth must match the hostname
# run via . ./multi.sh
echo "===>config model now<==="
echo $DISPLAY
export XAUTHORITY=/home/zzx/.Xauthority
xauth list
p=/home/zzx/fvp/rdn2-cfg2/model-scripts/rdinfra
ls $p
export MODEL=/home/zzx/fvp/support/Model/FVP_RD_N2_Cfg2
echo $MODEL
echo "===>run model now<==="

# use xclock to make sure xming runing ok

/home/zzx/fvp/support/Model/FVP_RD_N2_Multichip --data css0.scp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/scp_ramfw.bin@0x0BD80000 --data css0.mcp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/mcp_ramfw.bin@0x0BF80000 -C css0.mcp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin -C css0.scp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin -C css0.trustedBootROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/tf-bl1.bin -C board0.flashloader0.fname=../../../../output/rdn2cfg2/rdn2cfg2/fip-uefi.bin -C board0.flashloader1.fname=/home/davy/rdn2-cfg2/model-scripts/rdinfra/platforms/rdn2cfg2/nor1_flash.img -C board0.flashloader1.fnameWrite=/home/davy/rdn2-cfg2/model-scripts/rdinfra/platforms/rdn2cfg2/nor1_flash.img -C board0.flashloader2.fname=/home/davy/rdn2-cfg2/model-scripts/rdinfra/platforms/rdn2cfg2/nor2_flash.img -C board0.flashloader2.fnameWrite=/home/davy/rdn2-cfg2/model-scripts/rdinfra/platforms/rdn2cfg2/nor2_flash.img -I -R -A -p --iris-port 7101 -C css0.scp.pl011_uart_scp.out_file=rdn2cfg2/refinfra-3146020-uart-0-scp_2022-01-07_05.14.47 -C css0.scp.pl011_uart_scp.unbuffered_output=1 -C css0.scp.pl011_uart_scp.uart_enable=true -C css0.pl011_s_uart_ap.out_file=rdn2cfg2/refinfra-3146020-uart-0-console_2022-01-07_05.14.47 -C soc0.pl011_uart_mcp.out_file=rdn2cfg2/refinfra-3146020-uart-0-mcp_2022-01-07_05.14.47 -C soc0.pl011_uart_mcp.unbuffered_output=1 -C soc0.pl011_uart0.out_file=rdn2cfg2/refinfra-3146020-uart-0-armtf_2022-01-07_05.14.47 -C soc0.pl011_uart0.unbuffered_output=1 -C soc0.pl011_uart0.flow_ctrl_mask_en=1 -C soc0.pl011_uart0.enable_dc4=0 -C soc0.pl011_uart1.out_file=rdn2cfg2/refinfra-3146020-uart-1-mm_2022-01-07_05.14.47 -C soc0.pl011_uart1.unbuffered_output=1 -C soc0.pl011_uart1.flow_ctrl_mask_en=1 -C soc0.pl011_uart1.enable_dc4=0 -C css0.pl011_s_uart_ap.unbuffered_output=1 -C css0.gic_distributor.ITS-device-bits=20 -C css0.gic_distributor.multichip-threaded-dgi=0 -C pcie_group_css0.pciex16.hierarchy_file_name=\<default\> --data css1.scp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/scp_ramfw.bin@0x0BD80000 --data css1.mcp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/mcp_ramfw.bin@0x0BF80000 -C css1.mcp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin -C css1.scp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin -C css1.scp.pl011_uart_scp.out_file=rdn2cfg2/refinfra-3146020-uart-0-scp_2022-01-07_05.14.47_1 -C css1.scp.pl011_uart_scp.unbuffered_output=1 -C css1.scp.pl011_uart_scp.uart_enable=true -C css1.pl011_s_uart_ap.out_file=rdn2cfg2/refinfra-3146020-uart-0-console_2022-01-07_05.14.47_1 -C soc1.pl011_uart_mcp.out_file=rdn2cfg2/refinfra-3146020-uart-0-mcp_2022-01-07_05.14.47_1 -C soc1.pl011_uart_mcp.unbuffered_output=1 -C soc1.pl011_uart0.out_file=rdn2cfg2/refinfra-3146020-uart-0-armtf_2022-01-07_05.14.47_1 -C soc1.pl011_uart0.unbuffered_output=1 -C soc1.pl011_uart0.flow_ctrl_mask_en=1 -C soc1.pl011_uart0.enable_dc4=0 -C soc1.pl011_uart1.out_file=rdn2cfg2/refinfra-3146020-uart-1-mm_2022-01-07_05.14.47_1 -C soc1.pl011_uart1.unbuffered_output=1 -C soc1.pl011_uart1.flow_ctrl_mask_en=1 -C soc1.pl011_uart1.enable_dc4=0 -C css1.pl011_s_uart_ap.unbuffered_output=1 -C css1.gic_distributor.ITS-device-bits=20 -C css1.gic_distributor.multichip-threaded-dgi=0 --data css2.scp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/scp_ramfw.bin@0x0BD80000 --data css2.mcp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/mcp_ramfw.bin@0x0BF80000 -C css2.mcp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin -C css2.scp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin -C css2.scp.pl011_uart_scp.out_file=rdn2cfg2/refinfra-3146020-uart-0-scp_2022-01-07_05.14.47_2 -C css2.scp.pl011_uart_scp.unbuffered_output=1 -C css2.scp.pl011_uart_scp.uart_enable=true -C css2.pl011_s_uart_ap.out_file=rdn2cfg2/refinfra-3146020-uart-0-console_2022-01-07_05.14.47_2 -C soc2.pl011_uart_mcp.out_file=rdn2cfg2/refinfra-3146020-uart-0-mcp_2022-01-07_05.14.47_2 -C soc2.pl011_uart_mcp.unbuffered_output=1 -C soc2.pl011_uart0.out_file=rdn2cfg2/refinfra-3146020-uart-0-armtf_2022-01-07_05.14.47_2 -C soc2.pl011_uart0.unbuffered_output=1 -C soc2.pl011_uart0.flow_ctrl_mask_en=1 -C soc2.pl011_uart0.enable_dc4=0 -C soc2.pl011_uart1.out_file=rdn2cfg2/refinfra-3146020-uart-1-mm_2022-01-07_05.14.47_2 -C soc2.pl011_uart1.unbuffered_output=1 -C soc2.pl011_uart1.flow_ctrl_mask_en=1 -C soc2.pl011_uart1.enable_dc4=0 -C css2.pl011_s_uart_ap.unbuffered_output=1 -C css2.gic_distributor.ITS-device-bits=20 -C css2.gic_distributor.multichip-threaded-dgi=0 --data css3.scp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/scp_ramfw.bin@0x0BD80000 --data css3.mcp.armcortexm7ct=../../../../output/rdn2cfg2/rdn2cfg2/mcp_ramfw.bin@0x0BF80000 -C css3.mcp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin -C css3.scp.ROMloader.fname=../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin -C css3.scp.pl011_uart_scp.out_file=rdn2cfg2/refinfra-3146020-uart-0-scp_2022-01-07_05.14.47_3 -C css3.scp.pl011_uart_scp.unbuffered_output=1 -C css3.scp.pl011_uart_scp.uart_enable=true -C css3.pl011_s_uart_ap.out_file=rdn2cfg2/refinfra-3146020-uart-0-console_2022-01-07_05.14.47_3 -C soc3.pl011_uart_mcp.out_file=rdn2cfg2/refinfra-3146020-uart-0-mcp_2022-01-07_05.14.47_3 -C soc3.pl011_uart_mcp.unbuffered_output=1 -C soc3.pl011_uart0.out_file=rdn2cfg2/refinfra-3146020-uart-0-armtf_2022-01-07_05.14.47_3 -C soc3.pl011_uart0.unbuffered_output=1 -C soc3.pl011_uart0.flow_ctrl_mask_en=1 -C soc3.pl011_uart0.enable_dc4=0 -C soc3.pl011_uart1.out_file=rdn2cfg2/refinfra-3146020-uart-1-mm_2022-01-07_05.14.47_3 -C soc3.pl011_uart1.unbuffered_output=1 -C soc3.pl011_uart1.flow_ctrl_mask_en=1 -C soc3.pl011_uart1.enable_dc4=0 -C css3.pl011_s_uart_ap.unbuffered_output=1 -C css3.gic_distributor.ITS-device-bits=20 -C css3.gic_distributor.multichip-threaded-dgi=0 -C board0.virtioblockdevice.image_path=../../../../output/rdn2cfg2/grub-busybox.img


# iris
cd /home/zzx/FVP_ARM_Std_Library/Iris/Python
python3
import sys
sys.path.append('/home/zzx/FVP_ARM_Std_Library/Iris/Python/iris')
import iris.debug
model = iris.debug.NetworkModel("localhost",7101)
# cpu total 24 = 6 *  4 = scp + mcp + core0 + core1 + core2 + core3
>>> c = model.get_cpus()
>>> for i in c: print(i.instName)

# css
>>> t = model.get_targets()
>>> for i in t: print(i.instName)

# component.RD_N2_Multichip.css0.cmn700
# component.RD_N2_Multichip.css1.cmn700
# component.RD_N2_Multichip.css2.cmn700
# component.RD_N2_Multichip.css3.cmn700
f = open("css0cmn.txt", "w")
r = model.get_target("component.RD_N2_Multichip.css0.cmn700")
x = r.short_register_names.keys()
for i in x:
    try:
            s = r.read_register(i)
    except Exception as exc:
            print(i, exc)
            #input("Press Enter to continue...")
    else:
            f.write("%s \t %s \n" % (i, bin(s)))

f.close()

# binary show
#x = '11000000000000000000000000000000000000000000000110'.zfill(64)
import sys
x = sys.argv[1].zfill(64)

for i in range(len(x))[::-1]:
    print('%02d' % i, end='')
print()
for i in x:
    print(i.rjust(2), end='')
print()

# check device number, 6*6 => 0-35
# chip 0-3
r = model.get_target("component.RD_N2_Multichip.css0.cmn700")
# for i in range(0, 36): print(i, bin(r.read_register("XP%d.POR_MXP_NODE_INFO" % i)))
binary = lambda n: n>0 and [n&1]+binary(n>>1) or []
for i in range(0, 36):
    print("XP%d" % i)
    l = r.read_register("XP%d.POR_MXP_NODE_INFO" % i)
    #print([x for x in l.bit_length()[::-1]])
    y = binary(l)
    print(y)
    print(len(y))
    s = (l.bit_length())
    print([x for x in range(s)[::-1]])

# show the type
for i in range(0, 36):
    s = bin(r.read_register("XP%d.POR_MXP_NODE_INFO" % i))[2:].zfill(64)
    #print("XP%2d => %s" % (i, s))
    print("XP%2d => \x1b[1;33m%s\x1b[0m" % (i, s[64-5:64]))
    for j in range(0, 6):
        s = bin(r.read_register("XP%d.POR_MXP_DEVICE_PORT_CONNECT_INFO_P%d" % (i, j)))[2:].zfill(64)
        print("%2d%2d => \x1b[1;33m%s\x1b[0m" % (i, j, s[64-5:64]))
    
```

### Peripheral

~~~
HARDWARE----SOC----CMN--[RNFCAL]--AP----GIC
                      --[RNI/D]--MMU----PCIE----x4
                                            ----x8
                                            ----x16----Ethernet
                                                   ----SATA
                                                   ----USB
                      
                                  ----CCIX
                      --[CCG]--CCIX
                      --[TLK]--NIC----HDLCD
                                  ----I2S audio----HDMI
                                  ----I2C---------/
                      --[TLK]--IO(borad)
                      --[]--DMC----DDR
                      --[]--NIC----UART
                               ----Gtimer
                               ----WTD
                               ----BP----SRAM
                                     ----ROM
                      --[]--NIC----AXI2APB----GPIO
                      --[RNI]--MCP----NIC----AHB
                                         ----AXI2APB
                                         ----UART
                                         ----QSPI
                                         ----I2C
                             --SCP----NIC----AHB
                                         ----AXI2APB
                                         ----UART
                                         ----QSPI
                                         ----JTAG
                                         ----I2C
                      --[]--CoreSight----Debug
                                     ----Trace
                             
                      
        ----BOARD----NIC----AXI----SRAM
                               ----GIC
                               ----DDR
                               ----SD
                               ----HDLCD
                        ----AHB----SMB2AHB
                        ----APB----I3C
                               ----Meters
                               ----LED
                               ----Switch
                               ----QSPI
                               ----RTC----PLL
                               ----QSPI
                               ----I2S------HDMI
                               ----SBcon---/
                               ----PCIe
                               ----Cfg
                               
                               
all apb device connect with cpu via interrupt                        

~~~

### Reference

~~~
      PL01x – UART
ARM PrimeCell® Technical Reference Manual
PrimeCell® UART (PL011) Technical Reference Manual
PrimeCell UART (PL011) Technical Reference Manual
      PL02x – Synchronous Serial Port
ARM PrimeCell Synchronous Serial Port (PL022) Technical Reference Manual
      PL03x – Real Time Clock
ARM PrimeCell™ Technical Reference Manual
      PL050 – PS2 Keyboard/Mouse Interface
ARM PrimeCell PS2 Keyboard/Mouse Interface (PL050) Technical Reference Manual
      PL06x – General Purpose Input/Output
ARM PrimeCell™ General Purpose Input/Output (PL061) Technical Reference Manual
ARM PrimeCell General Purpose Input/Output (PL060) Technical Reference Manual
      PL08x – DMA Controller
PrimeCell DMA Controller (PL080) Technical Reference Manual
PrimeCell Single Master DMA Controller (PL081) Technical Reference Manual
      PL09x – Static Memory Controller
PrimeCell™ Synchronous Static Memory Controller (PL093) Technical Reference Manual
PrimeCell™ Static Memory Controller (PL092) Technical Reference Manual
      PL11x – Color LCD Controller
PrimeCell™ Color LCD Controller (PL110) Technical Reference Manual
PrimeCell Color LCD Controller (PL111) Technical Reference Manual
      PL13x – Smart Card Interface
ARM PrimeCell Smart Card Interface (PL131) Technical Reference Manual
ARM PrimeCell™ Smart Card Interface (PL130) Technical Reference Manual
      PL16x – DC-DC Converter Interface
ARM PrimeCell DC‑DC Converter Interface (PL160) Technical Reference Manual
      PL17x – Memory Controller
ARM PrimeCell™ MultiPort Memory Controller (PL176) Technical Reference Manual
PrimeCell™ MultiPort Memory Controller (PL175) Technical Reference Manual
PrimeCell™ MultiPort Memory Controller (PL172) Technical Reference Manual
ARM PrimeCell™ SDRAM Controller (PL170) Technical Reference Manual
      PL19x – Vectored Interrupt Controller
PrimeCell Vectored Interrupt Controller (PL190) Technical Reference Manual
ARM PrimeCell Vectored Interrupt Controller (PL192) Technical Reference Manual
      PL220 – External Bus Interface
ARM PrimeCell External Bus Interface (PL220) Technical Reference Manual
      PL23x – PrimeCell µDMA Controller
PrimeCell µDMA Controller (PL230) Technical Reference Manual
      PL24x – AHB Memory Controller
PrimeCell AHB Memory Controller (PL241) Technical Reference Manual
PrimeCell AHB SDR and NAND Memory Controller (PL242) Technical Reference Manual
PrimeCell AHB SDR and SRAM/NOR Memory Controller (PL243) Technical Reference Manual
PrimeCell AHB Memory Controller (PL244) Technical Reference Manual
PrimeCell AHB DDR and SRAM/NOR Memory Controller (PL245) Technical Reference Manual
      PL300 – AXI Configurable Interconnect
PrimeCell AXI Configurable Interconnect (PL300) Technical Reference Manual
      PL301 – High-Performance Matrix
      Revision: r2p0
AMBA Network Interconnect (NIC-301) Technical Reference Manual
      Revision: r1p2
PrimeCell® High-Performance Matrix (PL301) Technical Summary
      Revision: r1p1
PrimeCell High-Performance Matrix (PL301) Technical Summary
      Revision: r1p0
PrimeCell High-Performance Matrix (PL301) Technical Summary
      PL310 – Level 2 Cache Controller
      r3p0
AMBA Level 2 MBIST Controller (L2C-310) Technical Reference Manual
AMBA Level 2 Cache Controller (L2C-310) Technical Reference Manual
      r2p0
PrimeCell Level 2 Cache Controller (PL310) Technical Reference Manual
PrimeCell Level 2 MBIST Controller (PL310) Technical Reference Manual
      r1p0
PrimeCell Level 2 Cache Controller (PL310) Technical Reference Manual
PrimeCell Level 2 MBIST Controller (PL310) Technical Reference Manual
      r0p0
PL310 Cache Controller Technical Reference Manual
PL310 MBIST Controller Technical Reference Manual
      PL320 – Inter-Processor Communications Module
PrimeCell® Inter-Processor Communications Module (PL320) Technical Reference Manual
      DMA-330 DMA Controller
      Revision: r1p0
AMBA DMA Controller DMA-330 Technical Reference Manual
      Revision: r0p0
PrimeCell® DMA Controller (PL330) Technical Reference Manual
      DMC-34x Dynamic Memory Controllers
DMC-340 Revision: r4p0
AMBA DDR, LPDDR, and SDR Dynamic Memory Controller DMC-340 Technical Reference Manual
      PL340 Revision: r3p0
PrimeCell® Dynamic Memory Controller (PL340) Technical Reference Manual
      PL340 Revision: r2p0
PrimeCell Dynamic Memory Controller (PL340) Technical Reference Manual
      PL341 Revision: r1p0
PrimeCell DDR2 Dynamic Memory Controller (PL341) Technical Reference Manual
      PL341 Revision: r0p1
PrimeCell DDR2 Dynamic Memory Controller (PL341) Technical Reference Manual
      PL341 Revision: r0p0
PrimeCell DDR2 Dynamic Memory Controller (PL341) Technical Reference Manual
      DMC-342 Revision: r0p0
AMBA LPDDR2 Dynamic Memory Controller DMC-342 Technical Reference Manual
      PL35x – Static Memory Controller
PrimeCell Static Memory Controller (PL350 Series) Technical Reference Manual
      Revision: r2p0
PrimeCell Static Memory Controller (PL350 Series) Technical Reference Manual
      PL390 – Generic Interrupt Controller
ARM® Generic Interrupt Controller Architecture Specification
PrimeCell® Generic Interrupt Controller (PL390) Technical Reference Manual
      Peripheral Test Block
Peripheral Test Block Technical Reference Manual
      BP130 – Register Slice
PrimeCell Infratructure AMBA 3 AXI Register Slice (BP130) Technical Overview
      BP131 – Downsizer
PrimeCell Infratructure AMBA 3 AXI Downsizer (BP131) Technical Overview
      BP132-7 – Bridge
PrimeCell Infratructure AMBA 3 AXI Asynchronous Bridge (BP132) Technical Overview
PrimeCell Infratructure AMBA 3 AXI Downwards-synchronizing Bridge (BP133) Technical Overview
PrimeCell Infratructure AMBA 3 AXI Upwards-synchronizing Bridge (BP134) Technical Overview
PrimeCell Infratructure AMBA 3 AXI to AMBA 3 APB Bridge (BP135) Technical Overview
PrimeCell Infratructure AMBA 2 AHB to AMBA 3 AXI Bridges (BP136) Technical Overview
PrimeCell Infratructure AMBA 3 AXI to AMBA 2 AHB Bridges (BP137) Technical Overview
      BP140 – Internal Memory Interface
PrimeCell Infratructure AMBA 3 AXI Internal Memory Interface (BP140) Technical Overview
      BP141 – TrustZone Memory Adapter
PrimeCell Infratructure AMBA 3 AXI TrustZone Memory Adapter (BP141) Technical Overview
      BP144 – File Reader Master
PrimeCell Infratructure AMBA 3 AXI File Reader Master (BP144) Technical Overview
      BP147 – TrustZone Protection Controller
PrimeCell Infratructure AMBA 3 TrustZone Protection Controller (BP147) Technical Overview
~~~

### Image

~~~
Info: RD_N2_Multichip: RD_N2_Multichip.css0.scp.ROMloader: FlashLoader: Loaded 44 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css0.mcp.ROMloader: FlashLoader: Loaded 43 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css0.trustedBootROMloader: FlashLoader: Loaded 30 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/tf-bl1.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css1.scp.ROMloader: FlashLoader: Loaded 44 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css1.mcp.ROMloader: FlashLoader: Loaded 43 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css2.scp.ROMloader: FlashLoader: Loaded 44 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css2.mcp.ROMloader: FlashLoader: Loaded 43 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css3.scp.ROMloader: FlashLoader: Loaded 44 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/scp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.css3.mcp.ROMloader: FlashLoader: Loaded 43 kB from file '../../../../output/rdn2cfg2/rdn2cfg2/mcp_romfw.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.board0.flashloader0: FlashLoader: Loaded 5 MB from file '../../../../output/rdn2cfg2/rdn2cfg2/fip-uefi.bin'

Info: RD_N2_Multichip: RD_N2_Multichip.board0.flashloader1: FlashLoader: Loaded 64 MB from file '../../../../model-scripts/rdinfra/platforms/rdn2cfg2/nor1_flash.img'

Info: RD_N2_Multichip: RD_N2_Multichip.board0.flashloader2: FlashLoader: Loaded 49 MB from file '../../../../model-scripts/rdinfra/platforms/rdn2cfg2/nor2_flash.img'

~~~


<a href="#top">Back to top</a>
