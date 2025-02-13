---
date: 2015-12-24T00:41:00.002+01:00
description: ''
published: true
slug: 2015-12-ubuntu-hp-pavilion-x2-10-n109nc
tags:
- Linux
- HW
- Ubuntu
- Linux kernel
- legacy-blogger
readtime: 5
title: Ubuntu @ HP Pavilion X2 10-n109nc
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2015/12/ubuntu-hp-pavilion-x2-10-n109nc.html)*.

<div class="separator" style="clear: both; text-align: center;">
<a href="http://4.bp.blogspot.com/-03M9vwerP58/VnssDHllLxI/AAAAAAAAAqI/E6XW91CkO3Y/s1600/2015-12-23%2B23.45.03.jpg" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="180" src="http://4.bp.blogspot.com/-03M9vwerP58/VnssDHllLxI/AAAAAAAAAqI/E6XW91CkO3Y/s320/2015-12-23%2B23.45.03.jpg" width="320" /></a></div>
<br />
<br />
I just bought this laptop/tablet. And I wanted to run it with Ubuntu from the first moment. I knew that it is a bold aim...<br />
<br />
As I expected: Ordinary Ubuntu 15.10 desktop installer just hangs during startup. I had a bit more luck with the server installer (though I had to use USB NIC to connect to the Internet) and then I was able to install the system and after that I replaced kernel with 4.4-rc6 from&nbsp;kernel-ppa/mainline and X.Org with edgers PPA. Since then the wireless NIC runs and the system seems to be stable now.<br />
<br />
But ACPI does not work well (see the dmeg below), GPU gives a lot of errors into dmesg (not displayed), sound does not work at all, no battery indicator is available and suspend does not work.<br />
<br />
dmesg:
<br />
<br />
<pre>[    0.000000] Initializing cgroup subsys cpuset
[    0.000000] Initializing cgroup subsys cpu
[    0.000000] Initializing cgroup subsys cpuacct
[    0.000000] Linux version 4.4.0-040400rc6-generic (kernel@tangerine) (gcc version 5.2.1 20151010 (Ubuntu 5.2.1-22ubuntu2) ) #201512202030 SMP Mon Dec 21 01:32:09 UTC 2015
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-4.4.0-040400rc6-generic root=UUID=cacaa7c0-4b0d-4747-85d3-ef0cc2a17836 ro
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Centaur CentaurHauls
[    0.000000] x86/fpu: Legacy x87 FPU detected.
[    0.000000] x86/fpu: Using 'lazy' FPU context switches.
[    0.000000] e820: BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000008efff] usable
[    0.000000] BIOS-e820: [mem 0x000000000008f000-0x000000000008ffff] ACPI NVS
[    0.000000] BIOS-e820: [mem 0x0000000000090000-0x000000000009ffff] usable
[    0.000000] BIOS-e820: [mem 0x0000000000100000-0x000000001fffffff] usable
[    0.000000] BIOS-e820: [mem 0x0000000020000000-0x00000000201fffff] reserved
[    0.000000] BIOS-e820: [mem 0x0000000020200000-0x0000000079b5ffff] usable
[    0.000000] BIOS-e820: [mem 0x0000000079b60000-0x0000000079caffff] type 20
[    0.000000] BIOS-e820: [mem 0x0000000079cb0000-0x000000007a5affff] reserved
[    0.000000] BIOS-e820: [mem 0x000000007a5b0000-0x000000007a6affff] ACPI NVS
[    0.000000] BIOS-e820: [mem 0x000000007a6b0000-0x000000007a6effff] ACPI data
[    0.000000] BIOS-e820: [mem 0x000000007a6f0000-0x000000007bffffff] usable
[    0.000000] BIOS-e820: [mem 0x00000000e0000000-0x00000000e3ffffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fea00000-0x00000000feafffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fec00000-0x00000000fec00fff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fed01000-0x00000000fed01fff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fed03000-0x00000000fed03fff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fed06000-0x00000000fed06fff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fed08000-0x00000000fed09fff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fed1c000-0x00000000fed1cfff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fed80000-0x00000000fedbffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000fee00000-0x00000000fee00fff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000ffb00000-0x00000000ffffffff] reserved
[    0.000000] NX (Execute Disable) protection: active
[    0.000000] efi: EFI v2.40 by INSYDE Corp.
[    0.000000] efi:  ACPI 2.0=0x7a6ef014  SMBIOS=0x79d3d000  ESRT=0x79d3aa18 
[    0.000000] esrt: Reserving ESRT space from 0x0000000079d3aa18 to 0x0000000079d3aa50.
[    0.000000] SMBIOS 2.8 present.
[    0.000000] DMI: HP HP Pavilion x2 Detachable/813E, BIOS F.06 11/02/2015
[    0.000000] e820: update [mem 0x00000000-0x00000fff] usable ==&gt; reserved
[    0.000000] e820: remove [mem 0x000a0000-0x000fffff] usable
[    0.000000] e820: last_pfn = 0x7c000 max_arch_pfn = 0x400000000
[    0.000000] MTRR default type: uncachable
[    0.000000] MTRR fixed ranges enabled:
[    0.000000]   00000-9FFFF write-back
[    0.000000]   A0000-FFFFF write-protect
[    0.000000] MTRR variable ranges enabled:
[    0.000000]   0 base 0FFC00000 mask FFFC00000 write-protect
[    0.000000]   1 base 0FFB00000 mask FFFF00000 write-protect
[    0.000000]   2 base 000000000 mask F80000000 write-back
[    0.000000]   3 base 07E000000 mask FFE000000 uncachable
[    0.000000]   4 base 07D000000 mask FFF000000 uncachable
[    0.000000]   5 base 07C800000 mask FFF800000 uncachable
[    0.000000]   6 base 07C400000 mask FFFC00000 uncachable
[    0.000000]   7 disabled
[    0.000000] x86/PAT: Configuration [0-7]: WB  WC  UC- UC  WB  WC  UC- WT  
[    0.000000] Scanning 1 areas for low memory corruption
[    0.000000] Base memory trampoline at [ffff880000099000] 99000 size 24576
[    0.000000] BRK [0x031f6000, 0x031f6fff] PGTABLE
[    0.000000] BRK [0x031f7000, 0x031f7fff] PGTABLE
[    0.000000] BRK [0x031f8000, 0x031f8fff] PGTABLE
[    0.000000] BRK [0x031f9000, 0x031f9fff] PGTABLE
[    0.000000] BRK [0x031fa000, 0x031fafff] PGTABLE
[    0.000000] BRK [0x031fb000, 0x031fbfff] PGTABLE
[    0.000000] RAMDISK: [mem 0x33fc2000-0x35fd8fff]
[    0.000000] ACPI: Early table checksum verification disabled
[    0.000000] ACPI: RSDP 0x000000007A6EF014 000024 (v02 HPQOEM)
[    0.000000] ACPI: XSDT 0x000000007A6EF120 0000BC (v01 HPQOEM SLIC-MPC 00000003 HP   01000013)
[    0.000000] ACPI: FACP 0x000000007A6E5000 00010C (v05 HPQOEM SLIC-MPC 00000003 HP   00040000)
[    0.000000] ACPI: DSDT 0x000000007A6C1000 01E8D9 (v02 HPQOEM SLIC-MPC 00000003 ACPI 00040000)
[    0.000000] ACPI: FACS 0x000000007A651000 000040
[    0.000000] ACPI: UEFI 0x000000007A6EE000 000236 (v01 HPQOEM 813E     00000001 HP   00040000)
[    0.000000] ACPI: MSDM 0x000000007A6ED000 000055 (v03 HPQOEM SLIC-MPC 00000001 HP   00040000)
[    0.000000] ACPI: UEFI 0x000000007A6EC000 000042 (v01 HPQOEM 813E     00000000 HP   00040000)
[    0.000000] ACPI: SSDT 0x000000007A6E7000 0044CC (v01 HPQOEM 813E     00001000 ACPI 00040000)
[    0.000000] ACPI: HPET 0x000000007A6E4000 000038 (v01 HPQOEM 813E     00000003 HP   00040000)
[    0.000000] ACPI: LPIT 0x000000007A6E3000 000104 (v01 HPQOEM 813E     00000003 HP   00040000)
[    0.000000] ACPI: APIC 0x000000007A6E2000 00006C (v03 HPQOEM SLIC-MPC 00000003 HP   00040000)
[    0.000000] ACPI: MCFG 0x000000007A6E1000 00003C (v01 HPQOEM 813E     00000003 HP   00040000)
[    0.000000] ACPI: PRAM 0x000000007A6E0000 000030 (v01 HPQOEM 813E     00000003 HP   00040000)
[    0.000000] ACPI: SSDT 0x000000007A6C0000 0005E3 (v01 HPQOEM CpuDptf  00000003 ACPI 00040000)
[    0.000000] ACPI: SSDT 0x000000007A6BC000 003CAF (v01 HPQOEM DptfTab  00000003 ACPI 00040000)
[    0.000000] ACPI: SSDT 0x000000007A6BB000 000058 (v01 HPQOEM LowPwrM  00000003 ACPI 00040000)
[    0.000000] ACPI: SSDT 0x000000007A6BA000 000763 (v01 HPQOEM 813E     00003000 ACPI 00040000)
[    0.000000] ACPI: SSDT 0x000000007A6B9000 000290 (v01 HPQOEM 813E     00003000 ACPI 00040000)
[    0.000000] ACPI: SSDT 0x000000007A6B8000 00017A (v01 HPQOEM 813E     00003000 ACPI 00040000)
[    0.000000] ACPI: CSRT 0x000000007A6E6000 00014C (v00 HPQOEM 813E     00000003 HP   00040000)
[    0.000000] ACPI: FPDT 0x000000007A6B7000 000044 (v01 HPQOEM SLIC-MPC 00000002 HP   00040000)
[    0.000000] ACPI: BGRT 0x000000007A6B6000 000038 (v01 HPQOEM 813E     00000001 HP   00040000)
[    0.000000] ACPI: Local APIC address 0xfee00000
[    0.000000] No NUMA configuration found
[    0.000000] Faking a node at [mem 0x0000000000000000-0x000000007bffffff]
[    0.000000] NODE_DATA(0) allocated [mem 0x793f0000-0x793f3fff]
[    0.000000] Zone ranges:
[    0.000000]   DMA      [mem 0x0000000000001000-0x0000000000ffffff]
[    0.000000]   DMA32    [mem 0x0000000001000000-0x000000007bffffff]
[    0.000000]   Normal   empty
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000000001000-0x000000000008efff]
[    0.000000]   node   0: [mem 0x0000000000090000-0x000000000009ffff]
[    0.000000]   node   0: [mem 0x0000000000100000-0x000000001fffffff]
[    0.000000]   node   0: [mem 0x0000000020200000-0x0000000079b5ffff]
[    0.000000]   node   0: [mem 0x000000007a6f0000-0x000000007bffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000000001000-0x000000007bffffff]
[    0.000000] On node 0 totalpages: 504334
[    0.000000]   DMA zone: 64 pages used for memmap
[    0.000000]   DMA zone: 23 pages reserved
[    0.000000]   DMA zone: 3998 pages, LIFO batch:0
[    0.000000]   DMA32 zone: 7872 pages used for memmap
[    0.000000]   DMA32 zone: 500336 pages, LIFO batch:31
[    0.000000] Reserving Intel graphics stolen memory at 0x7cc00000-0x7ebfffff
[    0.000000] ACPI: PM-Timer IO Port: 0x408
[    0.000000] ACPI: Local APIC address 0xfee00000
[    0.000000] IOAPIC[0]: apic_id 1, version 32, address 0xfec00000, GSI 0-114
[    0.000000] ACPI: INT_SRC_OVR (bus 0 bus_irq 0 global_irq 2 dfl dfl)
[    0.000000] ACPI: INT_SRC_OVR (bus 0 bus_irq 9 global_irq 9 high level)
[    0.000000] ACPI: IRQ0 used by override.
[    0.000000] ACPI: IRQ9 used by override.
[    0.000000] Using ACPI (MADT) for SMP configuration information
[    0.000000] ACPI: HPET id: 0x8086a201 base: 0xfed00000
[    0.000000] smpboot: Allowing 4 CPUs, 0 hotplug CPUs
[    0.000000] PM: Registered nosave memory: [mem 0x00000000-0x00000fff]
[    0.000000] PM: Registered nosave memory: [mem 0x0008f000-0x0008ffff]
[    0.000000] PM: Registered nosave memory: [mem 0x000a0000-0x000fffff]
[    0.000000] PM: Registered nosave memory: [mem 0x20000000-0x201fffff]
[    0.000000] PM: Registered nosave memory: [mem 0x79b60000-0x79caffff]
[    0.000000] PM: Registered nosave memory: [mem 0x79cb0000-0x7a5affff]
[    0.000000] PM: Registered nosave memory: [mem 0x7a5b0000-0x7a6affff]
[    0.000000] PM: Registered nosave memory: [mem 0x7a6b0000-0x7a6effff]
[    0.000000] e820: [mem 0x7ec00000-0xdfffffff] available for PCI devices
[    0.000000] Booting paravirtualized kernel on bare hardware
[    0.000000] clocksource: refined-jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645519600211568 ns
[    0.000000] setup_percpu: NR_CPUS:256 nr_cpumask_bits:256 nr_cpu_ids:4 nr_node_ids:1
[    0.000000] PERCPU: Embedded 33 pages/cpu @ffff880079000000 s97496 r8192 d29480 u524288
[    0.000000] pcpu-alloc: s97496 r8192 d29480 u524288 alloc=1*2097152
[    0.000000] pcpu-alloc: [0] 0 1 2 3 
[    0.000000] Built 1 zonelists in Node order, mobility grouping on.  Total pages: 496375
[    0.000000] Policy zone: DMA32
[    0.000000] Kernel command line: BOOT_IMAGE=/boot/vmlinuz-4.4.0-040400rc6-generic root=UUID=cacaa7c0-4b0d-4747-85d3-ef0cc2a17836 ro
[    0.000000] PID hash table entries: 4096 (order: 3, 32768 bytes)
[    0.000000] Calgary: detecting Calgary via BIOS EBDA area
[    0.000000] Calgary: Unable to locate Rio Grande table in EBDA - bailing!
[    0.000000] Memory: 1876068K/2017336K available (8198K kernel code, 1262K rwdata, 3948K rodata, 1460K init, 1292K bss, 141268K reserved, 0K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=4, Nodes=1
[    0.000000] Hierarchical RCU implementation.
[    0.000000]  Build-time adjustment of leaf fanout to 64.
[    0.000000]  RCU restricting CPUs from NR_CPUS=256 to nr_cpu_ids=4.
[    0.000000] RCU: Adjusting geometry for rcu_fanout_leaf=64, nr_cpu_ids=4
[    0.000000] NR_IRQS:16640 nr_irqs:1024 16
[    0.000000] Console: colour dummy device 80x25
[    0.000000] console [tty0] enabled
[    0.000000] clocksource: hpet: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 133484882848 ns
[    0.000000] hpet clockevent registered
[    0.000000] tsc: PIT calibration matches HPET. 1 loops
[    0.000000] tsc: Detected 1439.974 MHz processor
[    0.000051] Calibrating delay loop (skipped), value calculated using timer frequency.. 2879.94 BogoMIPS (lpj=5759896)
[    0.000074] pid_max: default: 32768 minimum: 301
[    0.000098] ACPI: Core revision 20150930
[    0.068349] ACPI: 8 ACPI AML tables successfully acquired and loaded
[    0.070445] Security Framework initialized
[    0.070464] Yama: becoming mindful.
[    0.070489] AppArmor: AppArmor initialized
[    0.070879] Dentry cache hash table entries: 262144 (order: 9, 2097152 bytes)
[    0.072170] Inode-cache hash table entries: 131072 (order: 8, 1048576 bytes)
[    0.072782] Mount-cache hash table entries: 4096 (order: 3, 32768 bytes)
[    0.072807] Mountpoint-cache hash table entries: 4096 (order: 3, 32768 bytes)
[    0.073284] Initializing cgroup subsys io
[    0.073305] Initializing cgroup subsys memory
[    0.073329] Initializing cgroup subsys devices
[    0.073344] Initializing cgroup subsys freezer
[    0.073358] Initializing cgroup subsys net_cls
[    0.073370] Initializing cgroup subsys perf_event
[    0.073384] Initializing cgroup subsys net_prio
[    0.073397] Initializing cgroup subsys hugetlb
[    0.073414] Initializing cgroup subsys pids
[    0.073458] CPU: Physical Processor ID: 0
[    0.073507] CPU: Processor Core ID: 0
[    0.073522] ENERGY_PERF_BIAS: Set to 'normal', was 'performance'
[    0.073532] ENERGY_PERF_BIAS: View and update with x86_energy_perf_policy(8)
[    0.079016] mce: CPU supports 6 MCE banks
[    0.079040] CPU0: Thermal monitoring enabled (TM1)
[    0.079053] process: using mwait in idle threads
[    0.079068] Last level iTLB entries: 4KB 48, 2MB 0, 4MB 0
[    0.079078] Last level dTLB entries: 4KB 256, 2MB 16, 4MB 16, 1GB 0
[    0.079602] Freeing SMP alternatives memory: 28K (ffffffff820aa000 - ffffffff820b1000)
[    0.088425] ftrace: allocating 31491 entries in 124 pages
[    0.115925] ..TIMER: vector=0x30 apic1=0 pin1=2 apic2=-1 pin2=-1
[    0.155636] TSC deadline timer enabled
[    0.155646] smpboot: CPU0: Intel(R) Atom(TM) x5-Z8300  CPU @ 1.44GHz (family: 0x6, model: 0x4c, stepping: 0x3)
[    0.155733] Performance Events: PEBS fmt2+, 8-deep LBR, Silvermont events, full-width counters, Intel PMU driver.
[    0.155771] ... version:                3
[    0.155780] ... bit width:              40
[    0.155788] ... generic registers:      2
[    0.155797] ... value mask:             000000ffffffffff
[    0.155806] ... max period:             000000ffffffffff
[    0.155814] ... fixed-purpose events:   3
[    0.155822] ... event mask:             0000000700000003
[    0.157848] x86: Booting SMP configuration:
[    0.157864] .... node  #0, CPUs:      #1
[    0.165733] NMI watchdog: enabled on all CPUs, permanently consumes one hw-PMU counter.
[    0.166045]  #2 #3
[    0.181544] x86: Booted up 1 node, 4 CPUs
[    0.181562] smpboot: Total of 4 processors activated (11519.79 BogoMIPS)
[    0.182779] devtmpfs: initialized
[    0.191480] evm: security.selinux
[    0.191492] evm: security.SMACK64
[    0.191500] evm: security.SMACK64EXEC
[    0.191508] evm: security.SMACK64TRANSMUTE
[    0.191518] evm: security.SMACK64MMAP
[    0.191525] evm: security.ima
[    0.191533] evm: security.capability
[    0.191676] PM: Registering ACPI NVS region [mem 0x0008f000-0x0008ffff] (4096 bytes)
[    0.191694] PM: Registering ACPI NVS region [mem 0x7a5b0000-0x7a6affff] (1048576 bytes)
[    0.191957] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
[    0.192208] pinctrl core: initialized pinctrl subsystem
[    0.192524] RTC time: 23:34:39, date: 12/23/15
[    0.192904] NET: Registered protocol family 16
[    0.202322] cpuidle: using governor ladder
[    0.214334] cpuidle: using governor menu
[    0.214351] PCCT header not found.
[    0.214507] ACPI FADT declares the system doesn't support PCIe ASPM, so disable it
[    0.214525] ACPI: bus type PCI registered
[    0.214536] acpiphp: ACPI Hot Plug PCI Controller Driver version: 0.5
[    0.214734] PCI: MMCONFIG for domain 0000 [bus 00-3f] at [mem 0xe0000000-0xe3ffffff] (base 0xe0000000)
[    0.214754] PCI: MMCONFIG at [mem 0xe0000000-0xe3ffffff] reserved in E820
[    0.214776] PCI: Using configuration type 1 for base access
[    0.227426] ACPI: Added _OSI(Module Device)
[    0.227441] ACPI: Added _OSI(Processor Device)
[    0.227450] ACPI: Added _OSI(3.0 _SCP Extensions)
[    0.227459] ACPI: Added _OSI(Processor Aggregator Device)
[    0.269597] ACPI: Dynamic OEM Table Load:
[    0.269630] ACPI: SSDT 0xFFFF880074762000 00057B (v01 PmRef  Cpu0Ist  00003000 INTL 20130117)
[    0.272170] ACPI: Dynamic OEM Table Load:
[    0.272195] ACPI: SSDT 0xFFFF880074796000 0003A5 (v01 PmRef  Cpu0Cst  00003001 INTL 20130117)
[    0.275207] ACPI: Dynamic OEM Table Load:
[    0.275230] ACPI: SSDT 0xFFFF88007B806200 00015F (v01 PmRef  ApIst    00003000 INTL 20130117)
[    0.277668] ACPI: Dynamic OEM Table Load:
[    0.277691] ACPI: SSDT 0xFFFF8800747EE840 00008D (v01 PmRef  ApCst    00003000 INTL 20130117)
[    0.282507] ACPI: Interpreter enabled
[    0.282532] ACPI Exception: AE_NOT_FOUND, While evaluating Sleep State [\_S1_] (20150930/hwxface-580)
[    0.282559] ACPI Exception: AE_NOT_FOUND, While evaluating Sleep State [\_S2_] (20150930/hwxface-580)
[    0.282583] ACPI Exception: AE_NOT_FOUND, While evaluating Sleep State [\_S3_] (20150930/hwxface-580)
[    0.282629] ACPI: (supports S0 S4 S5)
[    0.282640] ACPI: Using IOAPIC for interrupt routing
[    0.282731] PCI: Using host bridge windows from ACPI; if necessary, use "pci=nocrs" and report a bug
[    0.284774] ACPI: Power Resource [USBC] (on)
[    0.286036] ACPI: Power Resource [WWPR] (off)
[    0.286879] ACPI: Power Resource [WWPR] (off)
[    0.288323] ACPI: Power Resource [WWPR] (off)
[    0.289134] ACPI: Power Resource [WWPR] (off)
[    0.289952] ACPI: Power Resource [WWPR] (off)
[    0.290858] ACPI: Power Resource [WWPR] (off)
[    0.305068] ACPI: Power Resource [CLK3] (on)
[    0.305199] ACPI: Power Resource [CLK4] (on)
[    0.315641] ACPI: Power Resource [CLK2] (on)
[    0.315777] ACPI: Power Resource [CLK1] (on)
[    0.315877] ACPI Error: No handler for Region [GMMR] (ffff880074cc65e8) [UserDefinedRegion] (20150930/evregion-163)
[    0.315903] ACPI Error: Region UserDefinedRegion (ID=146) has no handler (20150930/exfldio-297)
[    0.315928] ACPI Error: Method parse/execution failed [\_SB.PCI0.I2C3.CAMC._STA] (Node ffff880074cbbf28), AE_NOT_EXIST (20150930/psparse-542)
[    0.315999] ACPI Error: No handler for Region [GMMR] (ffff880074cc65e8) [UserDefinedRegion] (20150930/evregion-163)
[    0.316020] ACPI Error: Region UserDefinedRegion (ID=146) has no handler (20150930/exfldio-297)
[    0.316041] ACPI Error: Method parse/execution failed [\_SB.PCI0.I2C3.CAML._STA] (Node ffff880074cbd1b8), AE_NOT_EXIST (20150930/psparse-542)
[    0.317347] ACPI: Power Resource [CLK0] (on)
[    0.317472] ACPI: Power Resource [CLK1] (on)
[    0.318265] ACPI: Power Resource [P28X] (off)
[    0.318390] ACPI: Power Resource [P18X] (off)
[    0.321213] ACPI: Power Resource [P19X] (off)
[    0.329724] ACPI: Power Resource [ID3C] (on)
[    0.329920] ACPI: Power Resource [P06X] (off)
[    0.336448] ACPI: Power Resource [P12X] (off)
[    0.336578] ACPI: Power Resource [P28P] (off)
[    0.336712] ACPI: Power Resource [P18P] (off)
[    0.336847] ACPI: Power Resource [P28T] (off)
[    0.336972] ACPI: Power Resource [P18D] (off)
[    0.337098] ACPI: Power Resource [P18T] (off)
[    0.337218] ACPI: Power Resource [P3P3] (off)
[    0.337345] ACPI: Power Resource [P12T] (off)
[    0.337467] ACPI: Power Resource [P28W] (off)
[    0.337596] ACPI: Power Resource [P18W] (off)
[    0.337719] ACPI: Power Resource [P12W] (off)
[    0.337846] ACPI: Power Resource [P33W] (off)
[    0.337978] ACPI: Power Resource [P33X] (off)
[    0.345721] ACPI: PCI Root Bridge [PCI0] (domain 0000 [bus 00-ff])
[    0.345747] acpi PNP0A08:00: _OSC: OS supports [ExtendedConfig ASPM ClockPM Segments MSI]
[    0.346335] acpi PNP0A08:00: _OSC: OS now controls [PCIeHotplug PME AER PCIeCapability]
[    0.346353] acpi PNP0A08:00: FADT indicates ASPM is unsupported, using BIOS configuration
[    0.346399] acpi PNP0A08:00: [Firmware Info]: MMCONFIG for domain 0000 [bus 00-3f] only partially covers this bridge
[    0.347421] PCI host bridge to bus 0000:00
[    0.347437] pci_bus 0000:00: root bus resource [io  0x0070-0x0077]
[    0.347451] pci_bus 0000:00: root bus resource [io  0x0000-0x006f window]
[    0.347463] pci_bus 0000:00: root bus resource [io  0x0078-0x0cf7 window]
[    0.347475] pci_bus 0000:00: root bus resource [io  0x0d00-0xffff window]
[    0.347487] pci_bus 0000:00: root bus resource [mem 0x000a0000-0x000bffff window]
[    0.347503] pci_bus 0000:00: root bus resource [mem 0x000c0000-0x000dffff window]
[    0.347519] pci_bus 0000:00: root bus resource [mem 0x000e0000-0x000fffff window]
[    0.347535] pci_bus 0000:00: root bus resource [mem 0x20000000-0x201fffff window]
[    0.347550] pci_bus 0000:00: root bus resource [mem 0x7cc00001-0x7ec00000 window]
[    0.347566] pci_bus 0000:00: root bus resource [mem 0x80000000-0xdfffffff window]
[    0.347583] pci_bus 0000:00: root bus resource [bus 00-ff]
[    0.347610] pci 0000:00:00.0: [8086:2280] type 00 class 0x060000
[    0.347914] pci 0000:00:02.0: [8086:22b0] type 00 class 0x030000
[    0.347959] pci 0000:00:02.0: reg 0x10: [mem 0x90000000-0x90ffffff 64bit]
[    0.347977] pci 0000:00:02.0: reg 0x18: [mem 0x80000000-0x8fffffff 64bit pref]
[    0.347989] pci 0000:00:02.0: reg 0x20: [io  0x1000-0x103f]
[    0.348270] pci 0000:00:03.0: [8086:22b8] type 00 class 0x048000
[    0.348304] pci 0000:00:03.0: reg 0x10: [mem 0x91000000-0x913fffff]
[    0.348598] pci 0000:00:0a.0: [8086:22d8] type 00 class 0x000000
[    0.348642] pci 0000:00:0a.0: reg 0x10: [mem 0x9193c000-0x9193cfff]
[    0.349028] pci 0000:00:0b.0: [8086:22dc] type 00 class 0x118000
[    0.349077] pci 0000:00:0b.0: reg 0x10: [mem 0x91918000-0x91918fff 64bit]
[    0.349384] pci 0000:00:14.0: [8086:22b5] type 00 class 0x0c0330
[    0.349442] pci 0000:00:14.0: reg 0x10: [mem 0x91900000-0x9190ffff 64bit]
[    0.349552] pci 0000:00:14.0: PME# supported from D3hot D3cold
[    0.349839] pci 0000:00:1a.0: [8086:2298] type 00 class 0x108000
[    0.349882] pci 0000:00:1a.0: reg 0x10: [mem 0x91800000-0x918fffff]
[    0.349898] pci 0000:00:1a.0: reg 0x14: [mem 0x91700000-0x917fffff]
[    0.349990] pci 0000:00:1a.0: PME# supported from D0 D3hot
[    0.350444] pci 0000:00:1c.0: [8086:22c8] type 01 class 0x060400
[    0.352035] pci 0000:00:1c.0: PME# supported from D0 D3hot D3cold
[    0.352564] pci 0000:00:1f.0: [8086:229c] type 00 class 0x060100
[    0.354276] pci 0000:01:00.0: [8086:3165] type 00 class 0x028000
[    0.355845] pci 0000:01:00.0: reg 0x10: [mem 0x91600000-0x91601fff 64bit]
[    0.359412] pci 0000:01:00.0: PME# supported from D0 D3hot D3cold
[    0.367157] pci 0000:00:1c.0: PCI bridge to [bus 01]
[    0.367257] pci 0000:00:1c.0:   bridge window [mem 0x91600000-0x916fffff]
[    0.380404] ACPI: PCI Interrupt Link [LNKA] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.380603] ACPI: PCI Interrupt Link [LNKB] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.380794] ACPI: PCI Interrupt Link [LNKC] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.380986] ACPI: PCI Interrupt Link [LNKD] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.381178] ACPI: PCI Interrupt Link [LNKE] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.381368] ACPI: PCI Interrupt Link [LNKF] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.381558] ACPI: PCI Interrupt Link [LNKG] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.381748] ACPI: PCI Interrupt Link [LNKH] (IRQs 3 4 5 6 10 11 12 14 15) *0, disabled.
[    0.386176] vgaarb: setting as boot device: PCI:0000:00:02.0
[    0.386195] vgaarb: device added: PCI:0000:00:02.0,decodes=io+mem,owns=io+mem,locks=none
[    0.386213] vgaarb: loaded
[    0.386223] vgaarb: bridge control possible 0000:00:02.0
[    0.386954] SCSI subsystem initialized
[    0.387098] libata version 3.00 loaded.
[    0.387163] ACPI: bus type USB registered
[    0.387228] usbcore: registered new interface driver usbfs
[    0.387262] usbcore: registered new interface driver hub
[    0.387320] usbcore: registered new device driver usb
[    0.388018] PCI: Using ACPI for IRQ routing
[    0.390755] PCI: pci_cache_line_size set to 64 bytes
[    0.391176] Expanded resource reserved due to conflict with PCI Bus 0000:00
[    0.391191] e820: reserve RAM buffer [mem 0x0008f000-0x0008ffff]
[    0.391196] e820: reserve RAM buffer [mem 0x79b60000-0x7bffffff]
[    0.391498] NetLabel: Initializing
[    0.391509] NetLabel:  domain hash size = 128
[    0.391517] NetLabel:  protocols = UNLABELED CIPSOv4
[    0.391555] NetLabel:  unlabeled traffic allowed by default
[    0.391748] hpet0: at MMIO 0xfed00000, IRQs 2, 8, 0
[    0.391766] hpet0: 3 comparators, 64-bit 14.318180 MHz counter
[    0.393845] clocksource: Switched to clocksource hpet
[    0.409465] AppArmor: AppArmor Filesystem Enabled
[    0.409686] pnp: PnP ACPI init
[    0.409951] system 00:00: [io  0x0680-0x069f] has been reserved
[    0.409966] system 00:00: [io  0x0400-0x047f] has been reserved
[    0.409979] system 00:00: [io  0x0500-0x05fe] has been reserved
[    0.409998] system 00:00: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.410237] pnp 00:01: Plug and Play ACPI device, IDs PNP0501 (active)
[    0.413690] ACPI Error: No handler for Region [GMMR] (ffff880074cc65e8) [UserDefinedRegion] (20150930/evregion-163)
[    0.413716] ACPI Error: Region UserDefinedRegion (ID=146) has no handler (20150930/exfldio-297)
[    0.413737] ACPI Error: Method parse/execution failed [\_SB.PCI0.I2C3.CAMC._STA] (Node ffff880074cbbf28), AE_NOT_EXIST (20150930/psparse-542)
[    0.413771] ACPI Error: Method execution failed [\_SB.PCI0.I2C3.CAMC._STA] (Node ffff880074cbbf28), AE_NOT_EXIST (20150930/uteval-103)
[    0.413819] ACPI Error: No handler for Region [GMMR] (ffff880074cc65e8) [UserDefinedRegion] (20150930/evregion-163)
[    0.413866] ACPI Error: Region UserDefinedRegion (ID=146) has no handler (20150930/exfldio-297)
[    0.413887] ACPI Error: Method parse/execution failed [\_SB.PCI0.I2C3.CAML._STA] (Node ffff880074cbd1b8), AE_NOT_EXIST (20150930/psparse-542)
[    0.413918] ACPI Error: Method execution failed [\_SB.PCI0.I2C3.CAML._STA] (Node ffff880074cbd1b8), AE_NOT_EXIST (20150930/uteval-103)
[    0.417069] system 00:02: [mem 0x9193a000-0x9193afff] has been reserved
[    0.417085] system 00:02: [mem 0x91938000-0x91938fff] has been reserved
[    0.417098] system 00:02: [mem 0x91936000-0x91936fff] has been reserved
[    0.417111] system 00:02: [mem 0x91925000-0x91925fff] has been reserved
[    0.417124] system 00:02: [mem 0x91923000-0x91923fff] has been reserved
[    0.417138] system 00:02: [mem 0x91921000-0x91921fff] has been reserved
[    0.417150] system 00:02: [mem 0x9191f000-0x9191ffff] has been reserved
[    0.417163] system 00:02: [mem 0x9191d000-0x9191dfff] has been reserved
[    0.417175] system 00:02: [mem 0x9191b000-0x9191bfff] has been reserved
[    0.417188] system 00:02: [mem 0x91919000-0x91919fff] has been reserved
[    0.417200] system 00:02: [mem 0x91934000-0x91934fff] has been reserved
[    0.417213] system 00:02: [mem 0x91932000-0x91932fff] has been reserved
[    0.417225] system 00:02: [mem 0x91930000-0x91930fff] has been reserved
[    0.417238] system 00:02: [mem 0x9192e000-0x9192efff] has been reserved
[    0.417251] system 00:02: [mem 0x9192c000-0x9192cfff] has been reserved
[    0.417263] system 00:02: [mem 0x9192a000-0x9192afff] has been reserved
[    0.417276] system 00:02: [mem 0x91928000-0x91928fff] has been reserved
[    0.417288] system 00:02: [mem 0x91926000-0x91926fff] has been reserved
[    0.417305] system 00:02: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.417632] system 00:03: [mem 0xe0000000-0xefffffff] could not be reserved
[    0.417647] system 00:03: [mem 0xfea00000-0xfeafffff] has been reserved
[    0.417660] system 00:03: [mem 0xfed01000-0xfed01fff] has been reserved
[    0.417672] system 00:03: [mem 0xfed03000-0xfed03fff] has been reserved
[    0.417685] system 00:03: [mem 0xfed06000-0xfed06fff] has been reserved
[    0.417697] system 00:03: [mem 0xfed08000-0xfed09fff] has been reserved
[    0.417710] system 00:03: [mem 0xfed80000-0xfedbffff] could not be reserved
[    0.417722] system 00:03: [mem 0xfed1c000-0xfed1cfff] has been reserved
[    0.417735] system 00:03: [mem 0xfee00000-0xfeefffff] could not be reserved
[    0.417750] system 00:03: Plug and Play ACPI device, IDs PNP0c02 (active)
[    0.418208] pnp 00:04: Plug and Play ACPI device, IDs PNP0b00 (active)
[    0.419467] pnp: PnP ACPI: found 5 devices
[    0.430576] clocksource: acpi_pm: mask: 0xffffff max_cycles: 0xffffff, max_idle_ns: 2085701024 ns
[    0.430756] pci 0000:00:1c.0: PCI bridge to [bus 01]
[    0.430836] pci 0000:00:1c.0:   bridge window [mem 0x91600000-0x916fffff]
[    0.430978] pci_bus 0000:00: resource 4 [io  0x0070-0x0077]
[    0.430984] pci_bus 0000:00: resource 5 [io  0x0000-0x006f window]
[    0.430989] pci_bus 0000:00: resource 6 [io  0x0078-0x0cf7 window]
[    0.430994] pci_bus 0000:00: resource 7 [io  0x0d00-0xffff window]
[    0.430999] pci_bus 0000:00: resource 8 [mem 0x000a0000-0x000bffff window]
[    0.431004] pci_bus 0000:00: resource 9 [mem 0x000c0000-0x000dffff window]
[    0.431010] pci_bus 0000:00: resource 10 [mem 0x000e0000-0x000fffff window]
[    0.431015] pci_bus 0000:00: resource 11 [mem 0x20000000-0x201fffff window]
[    0.431020] pci_bus 0000:00: resource 12 [mem 0x7cc00001-0x7ec00000 window]
[    0.431026] pci_bus 0000:00: resource 13 [mem 0x80000000-0xdfffffff window]
[    0.431031] pci_bus 0000:01: resource 1 [mem 0x91600000-0x916fffff]
[    0.431109] NET: Registered protocol family 2
[    0.431503] TCP established hash table entries: 16384 (order: 5, 131072 bytes)
[    0.431622] TCP bind hash table entries: 16384 (order: 6, 262144 bytes)
[    0.431740] TCP: Hash tables configured (established 16384 bind 16384)
[    0.431816] UDP hash table entries: 1024 (order: 3, 32768 bytes)
[    0.431854] UDP-Lite hash table entries: 1024 (order: 3, 32768 bytes)
[    0.431984] NET: Registered protocol family 1
[    0.432038] pci 0000:00:02.0: Video device with shadowed ROM
[    0.434193] PCI: CLS 64 bytes, default 64
[    0.434333] Trying to unpack rootfs image as initramfs...
[    1.670316] Freeing initrd memory: 32860K (ffff880033fc2000 - ffff880035fd9000)
[    1.670799] Scanning for low memory corruption every 60 seconds
[    1.671844] futex hash table entries: 1024 (order: 4, 65536 bytes)
[    1.671933] audit: initializing netlink subsys (disabled)
[    1.671982] audit: type=2000 audit(1450913680.660:1): initialized
[    1.672700] Initialise system trusted keyring
[    1.672996] HugeTLB registered 2 MB page size, pre-allocated 0 pages
[    1.677449] zbud: loaded
[    1.678002] VFS: Disk quotas dquot_6.6.0
[    1.678111] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
[    1.679542] fuse init (API version 7.23)
[    1.679955] Key type big_key registered
[    1.681220] Key type asymmetric registered
[    1.681239] Asymmetric key parser 'x509' registered
[    1.681379] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 249)
[    1.681491] io scheduler noop registered
[    1.681507] io scheduler deadline registered (default)
[    1.681614] io scheduler cfq registered
[    1.683895] pcieport 0000:00:1c.0: Signaling PME through PCIe PME interrupt
[    1.683914] pci 0000:01:00.0: Signaling PME through PCIe PME interrupt
[    1.683969] pcie_pme 0000:00:1c.0:pcie01: service driver pcie_pme loaded
[    1.683991] pci_hotplug: PCI Hot Plug PCI Core version: 0.5
[    1.684017] pciehp: PCI Express Hot Plug Controller Driver version: 0.4
[    1.684132] efifb: probing for efifb
[    1.684179] efifb: framebuffer at 0x80000000, mapped to 0xffffc90000800000, using 4032k, total 4032k
[    1.684196] efifb: mode is 1280x800x32, linelength=5120, pages=1
[    1.684205] efifb: scrolling: redraw
[    1.684215] efifb: Truecolor: size=8:8:8:8, shift=24:16:8:0
[    1.693384] Console: switching to colour frame buffer device 160x50
[    1.702316] fb0: EFI VGA frame buffer device
[    1.702403] intel_idle: MWAIT substates: 0x33000020
[    1.702408] intel_idle: v0.4 model 0x4C
[    1.702412] intel_idle: lapic_timer_reliable_states 0xffffffff
[    1.703141] ACPI Error: No handler for Region [DSMB] (ffff880074cd0048) [GenericSerialBus] (20150930/evregion-163)
[    1.703306] ACPI Error: Region GenericSerialBus (ID=9) has no handler (20150930/exfldio-297)
[    1.703437] ACPI Error: Method parse/execution failed [\_SB.ADP1._PSR] (Node ffff880074ccff50), AE_NOT_EXIST (20150930/psparse-542)
[    1.703637] ACPI Exception: AE_NOT_EXIST, Error reading AC Adapter state (20150930/ac-128)
[    1.703966] input: Power Button as /devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0C:00/input/input0
[    1.704090] ACPI: Power Button [PWRB]
[    1.704258] input: Lid Switch as /devices/LNXSYSTM:00/LNXSYBUS:00/PNP0C0D:00/input/input1
[    1.704461] ACPI: Lid Switch [LID0]
[    1.704633] input: Power Button as /devices/LNXSYSTM:00/LNXPWRBN:00/input/input2
[    1.704742] ACPI: Power Button [PWRF]
[    1.704926] input: Sleep Button as /devices/LNXSYSTM:00/LNXSLPBN:00/input/input3
[    1.705035] ACPI: Sleep Button [SLPF]
[    1.705113] ACPI Error: Could not enable SleepButton event (20150930/evxfevnt-212)
[    1.705233] ACPI Warning: Could not enable fixed event - SleepButton (3) (20150930/evxface-654)
[    1.722291] button: probe of LNXSLPBN:00 failed with error -22
[    1.728169] thermal LNXTHERM:00: registered as thermal_zone0
[    1.728264] ACPI: Thermal Zone [TZ00] (0 C)
[    1.728536] ACPI: Invalid active0 threshold
[    1.728841] thermal LNXTHERM:01: registered as thermal_zone1
[    1.728927] ACPI: Thermal Zone [TZ01] (27 C)
[    1.729097] GHES: HEST is not enabled!
[    1.729584] Serial: 8250/16550 driver, 32 ports, IRQ sharing enabled
[    1.750063] 00:01: ttyS0 at I/O 0x3f8 (irq = 4, base_baud = 115200) is a 16550A
[    1.758084] Linux agpgart interface v0.103
[    1.773273] brd: module loaded
[    1.784221] loop: module loaded
[    1.788393] libphy: Fixed MDIO Bus: probed
[    1.791936] tun: Universal TUN/TAP device driver, 1.6
[    1.795463] tun: (C) 1999-2004 Max Krasnyansky 
[    1.799261] PPP generic driver version 2.4.2
[    1.803112] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    1.806757] ehci-pci: EHCI PCI platform driver
[    1.810406] ehci-platform: EHCI generic platform driver
[    1.814068] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
[    1.817644] ohci-pci: OHCI PCI platform driver
[    1.821251] ohci-platform: OHCI generic platform driver
[    1.824881] uhci_hcd: USB Universal Host Controller Interface driver
[    1.828878] xhci_hcd 0000:00:14.0: xHCI Host Controller
[    1.832500] xhci_hcd 0000:00:14.0: new USB bus registered, assigned bus number 1
[    1.837311] xhci_hcd 0000:00:14.0: hcc params 0x200077c1 hci version 0x100 quirks 0x00109810
[    1.840957] xhci_hcd 0000:00:14.0: cache line size of 64 is not supported
[    1.841326] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002
[    1.844982] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    1.848613] usb usb1: Product: xHCI Host Controller
[    1.852221] usb usb1: Manufacturer: Linux 4.4.0-040400rc6-generic xhci-hcd
[    1.855831] usb usb1: SerialNumber: 0000:00:14.0
[    1.859900] hub 1-0:1.0: USB hub found
[    1.863484] hub 1-0:1.0: 7 ports detected
[    1.868876] xhci_hcd 0000:00:14.0: xHCI Host Controller
[    1.872422] xhci_hcd 0000:00:14.0: new USB bus registered, assigned bus number 2
[    1.876301] usb usb2: New USB device found, idVendor=1d6b, idProduct=0003
[    1.879807] usb usb2: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    1.883281] usb usb2: Product: xHCI Host Controller
[    1.886648] usb usb2: Manufacturer: Linux 4.4.0-040400rc6-generic xhci-hcd
[    1.889989] usb usb2: SerialNumber: 0000:00:14.0
[    1.893670] hub 2-0:1.0: USB hub found
[    1.896909] hub 2-0:1.0: 6 ports detected
[    1.900604] usb: port power management may be unreliable
[    1.905000] i8042: PNP: No PS/2 controller found. Probing ports directly.
[    2.439397] i8042: Can't read CTR while initializing i8042
[    2.442516] i8042: probe of i8042 failed with error -5
[    2.446020] mousedev: PS/2 mouse device common for all mice
[    2.450305] rtc_cmos 00:04: rtc core: registered rtc_cmos as rtc0
[    2.453388] rtc_cmos 00:04: alarms up to one month, y3k, 242 bytes nvram, hpet irqs
[    2.456432] i2c /dev entries driver
[    2.459562] device-mapper: uevent: version 1.0.3
[    2.463037] device-mapper: ioctl: 4.34.0-ioctl (2015-10-28) initialised: dm-devel@redhat.com
[    2.466103] Intel P-state driver initializing.
[    2.469765] ledtrig-cpu: registered to indicate activity on CPUs
[    2.478588] EFI Variables Facility v0.08 2004-May-17
[    2.536024] NET: Registered protocol family 10
[    2.539379] NET: Registered protocol family 17
[    2.541754] Key type dns_resolver registered
[    2.544790] microcode: CPU0 sig=0x406c3, pf=0x1, revision=0x359
[    2.547461] microcode: CPU1 sig=0x406c3, pf=0x1, revision=0x359
[    2.556149] microcode: CPU2 sig=0x406c3, pf=0x1, revision=0x359
[    2.558575] microcode: CPU3 sig=0x406c3, pf=0x1, revision=0x359
[    2.561053] microcode: Microcode Update Driver: v2.01 , Peter Oruba
[    2.563988] registered taskstats version 1
[    2.566335] Loading compiled-in X.509 certificates
[    2.570674] Loaded X.509 cert 'Build time autogenerated kernel key: 4e47d338fce2694b68505af61d12b2a72742b92a'
[    2.573096] zswap: loaded using pool lzo/zbud
[    2.584323] Key type trusted registered
[    2.606339] Key type encrypted registered
[    2.608992] AppArmor: AppArmor sha1 policy hashing enabled
[    2.611365] ima: No TPM chip found, activating TPM-bypass!
[    2.613781] evm: HMAC attrs: 0x1
[    2.617730]   Magic number: 11:237:606
[    2.620626] rtc_cmos 00:04: setting system clock to 2015-12-23 23:34:41 UTC (1450913681)
[    2.624189] BIOS EDD facility v0.16 2004-Jun-25, 0 devices found
[    2.626803] EDD information not available.
[    2.629559] PM: Hibernation image not present or could not be loaded.
[    2.633002] Freeing unused kernel memory: 1460K (ffffffff81f3d000 - ffffffff820aa000)
[    2.635437] Write protecting the kernel read-only data: 14336k
[    2.640313] Freeing unused kernel memory: 2028K (ffff880002805000 - ffff880002a00000)
[    2.644216] Freeing unused kernel memory: 148K (ffff880002ddb000 - ffff880002e00000)
[    2.658031] usb 1-3: new full-speed USB device number 2 using xhci_hcd
[    2.673878] tsc: Refined TSC clocksource calibration: 1439.953 MHz
[    2.676619] clocksource: tsc: mask: 0xffffffffffffffff max_cycles: 0x14c18e756e4, max_idle_ns: 440795259570 ns
[    2.706216] random: systemd-udevd urandom read with 2 bits of entropy available
[    2.836933] sdhci: Secure Digital Host Controller Interface driver
[    2.840009] sdhci: Copyright(c) Pierre Ossman
[    2.846824] usb 1-3: New USB device found, idVendor=8087, idProduct=0a2a
[    2.847698] sdhci-acpi 80860F14:00: No vmmc regulator found
[    2.847701] sdhci-acpi 80860F14:00: No vqmmc regulator found
[    2.852089] mmc0: SDHCI controller on ACPI [80860F14:00] using ADMA
[    2.853848] sdhci-acpi 80860F14:03: No vmmc regulator found
[    2.853850] sdhci-acpi 80860F14:03: No vqmmc regulator found
[    2.864808] usb 1-3: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[    2.865978] wmi: Mapper loaded
[    2.871070] mmc1: SDHCI controller on ACPI [80860F14:03] using ADMA
[    2.878063] hidraw: raw HID events driver (C) Jiri Kosina
[    2.882299] FUJITSU Extended Socket Network Device Driver - version 1.0 - Copyright (c) 2015 FUJITSU LIMITED
[    2.929111] [drm] Initialized drm 1.1.0 20060810
[    2.964810] mmc0: MAN_BKOPS_EN bit is not set
[    2.980706] mmc0: new HS200 MMC card at address 0001
[    2.989880] usb 1-4: new low-speed USB device number 3 using xhci_hcd
[    3.008135] [drm] Memory usable by graphics device = 2048M
[    3.011470] checking generic (80000000 3f0000) vs hw (80000000 10000000)
[    3.011476] fb: switching to inteldrmfb from EFI VGA
[    3.014287] Console: switching to colour dummy device 80x25
[    3.014507] [drm] Replacing VGA console driver
[    3.014634] mmcblk0: mmc0:0001 SDW64G 58.2 GiB 
[    3.014977] mmcblk0boot0: mmc0:0001 SDW64G partition 1 4.00 MiB
[    3.015626] mmcblk0boot1: mmc0:0001 SDW64G partition 2 4.00 MiB
[    3.016104] mmcblk0rpmb: mmc0:0001 SDW64G partition 3 4.00 MiB
[    3.016868] [drm] Supports vblank timestamp caching Rev 2 (21.10.2013).
[    3.016882] [drm] Driver supports precise vblank timestamp query.
[    3.021619]  mmcblk0: p1 p2 p3
[    3.090162] vgaarb: device changed decodes: PCI:0000:00:02.0,olddecodes=io+mem,decodes=io+mem:owns=io+mem
[    3.143686] ACPI: Video Device [GFX0] (multi-head: yes  rom: no  post: no)
[    3.146883] input: Video Bus as /devices/LNXSYSTM:00/LNXSYBUS:00/PNP0A08:00/LNXVIDEO:00/input/input4
[    3.148377] [drm] Initialized i915 1.6.0 20151010 for 0000:00:02.0 on minor 0
[    3.193412] usb 1-4: New USB device found, idVendor=04f3, idProduct=074d
[    3.193464] usb 1-4: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[    3.194001] usb 1-4: ep 0x81 - rounding interval to 64 microframes, ep desc says 80 microframes
[    3.194028] usb 1-4: ep 0x82 - rounding interval to 64 microframes, ep desc says 80 microframes
[    3.196252] fbcon: inteldrmfb (fb0) is primary device
[    3.226369] usbcore: registered new interface driver usbhid
[    3.226371] usbhid: USB HID core driver
[    3.230791] input: HID 04f3:074d as /devices/pci0000:00/0000:00:14.0/usb1/1-4/1-4:1.0/0003:04F3:074D.0001/input/input5
[    3.231461] hid-generic 0003:04F3:074D.0001: input,hidraw0: USB HID v1.11 Keyboard [HID 04f3:074d] on usb-0000:00:14.0-4/input0
[    3.245711] input: HID 04f3:074d as /devices/pci0000:00/0000:00:14.0/usb1/1-4/1-4:1.1/0003:04F3:074D.0002/input/input6
[    3.247914] hid-generic 0003:04F3:074D.0002: input,hiddev0,hidraw1: USB HID v1.11 Mouse [HID 04f3:074d] on usb-0000:00:14.0-4/input1
[    3.679380] clocksource: Switched to clocksource tsc
[    4.753288] Console: switching to colour frame buffer device 160x50
[    4.761312] i915 0000:00:02.0: fb0: inteldrmfb frame buffer device
[    4.870454] EXT4-fs (mmcblk0p2): mounted filesystem with ordered data mode. Opts: (null)
[    5.121474] systemd[1]: Failed to insert module 'kdbus': Function not implemented
[    5.203043] systemd[1]: systemd 225 running in system mode. (+PAM +AUDIT +SELINUX +IMA +APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ -LZ4 +SECCOMP +BLKID -ELFUTILS +KMOD -IDN)
[    5.203731] systemd[1]: Detected architecture x86-64.
[    5.215504] systemd[1]: Set hostname to .
[    5.463297] systemd[1]: Reached target Encrypted Volumes.
[    5.469672] systemd[1]: Reached target User and Group Name Lookups.
[    5.475970] systemd[1]: Reached target Remote File Systems (Pre).
[    5.482688] systemd[1]: Set up automount Arbitrary Executable File Formats File System Automount Point.
[    5.489348] systemd[1]: Started Forward Password Requests to Wall Directory Watch.
[    5.496012] systemd[1]: Created slice Root Slice.
[    5.502587] systemd[1]: Listening on udev Kernel Socket.
[    5.509089] systemd[1]: Listening on udev Control Socket.
[    5.515549] systemd[1]: Listening on fsck to fsckd communication Socket.
[    5.522255] systemd[1]: Created slice User and Session Slice.
[    5.528774] systemd[1]: Listening on Journal Socket (/dev/log).
[    5.535417] systemd[1]: Listening on Journal Audit Socket.
[    5.541972] systemd[1]: Listening on /dev/initctl Compatibility Named Pipe.
[    5.548514] systemd[1]: Created slice System Slice.
[    5.567134] systemd[1]: Starting Increase datagram queue length...
[    5.574371] systemd[1]: Reached target Slices.
[    5.581217] systemd[1]: Created slice system-systemd\x2dfsck.slice.
[    5.588350] systemd[1]: Created slice system-getty.slice.
[    5.594671] systemd[1]: Listening on Journal Socket.
[    5.615206] systemd[1]: Started Braille Device Support.
[    5.626681] systemd[1]: Starting Create list of required static device nodes for the current kernel...
[    5.635585] systemd[1]: Mounting POSIX Message Queue File System...
[    5.644226] systemd[1]: Started Read required files in advance.
[    5.652380] systemd[1]: Starting Setup Virtual Console...
[    5.662148] systemd[1]: Starting udev Coldplug all Devices...
[    5.670673] systemd[1]: Mounting Huge Pages File System...
[    5.683028] systemd[1]: Starting Load Kernel Modules...
[    5.692004] systemd[1]: Starting Uncomplicated firewall...
[    5.701109] systemd[1]: Mounting Debug File System...
[    5.710757] systemd[1]: Mounted Debug File System.
[    5.717512] systemd[1]: Mounted POSIX Message Queue File System.
[    5.723798] systemd[1]: Mounted Huge Pages File System.
[    5.724446] lp: driver loaded but no devices found
[    5.733216] systemd[1]: Started Increase datagram queue length.
[    5.740153] ppdev: user-space parallel port driver
[    5.741304] systemd[1]: Started Create list of required static device nodes for the current kernel.
[    5.750097] systemd[1]: ureadahead.service: Main process exited, code=exited, status=5/NOTINSTALLED
[    5.753937] systemd[1]: ureadahead.service: Unit entered failed state.
[    5.756988] systemd[1]: ureadahead.service: Failed with result 'exit-code'.
[    5.760271] systemd[1]: Started Setup Virtual Console.
[    5.780484] systemd[1]: Started Load Kernel Modules.
[    5.787241] systemd[1]: Started Uncomplicated firewall.
[    5.890524] systemd[1]: Mounting FUSE Control File System...
[    5.899389] systemd[1]: Starting Apply Kernel Variables...
[    5.908096] systemd[1]: Starting Create Static Device Nodes in /dev...
[    5.914953] systemd[1]: Listening on Syslog Socket.
[    5.923370] systemd[1]: Starting Journal Service...
[    5.930126] systemd[1]: Mounted FUSE Control File System.
[    5.933790] systemd[1]: Started udev Coldplug all Devices.
[    5.937394] systemd[1]: Started Apply Kernel Variables.
[    5.953526] systemd[1]: Started Create Static Device Nodes in /dev.
[    5.986803] systemd[1]: Starting udev Kernel Device Manager...
[    5.996061] systemd[1]: Started Journal Service.
[    6.114276] EXT4-fs (mmcblk0p2): re-mounted. Opts: errors=remount-ro
[    6.160453] systemd-journald[288]: Received request to flush runtime journal from PID 1
[    6.264743] 8086228A:00: ttyS4 at MMIO 0x91922000 (irq = 39, base_baud = 2764800) is a 16550A
[    6.272524] 8086228A:01: ttyS5 at MMIO 0x91920000 (irq = 40, base_baud = 2764800) is a 16550A
[    6.272881] ACPI Error: No handler for Region [GMMR] (ffff880074cc65e8) [UserDefinedRegion] (20150930/evregion-163)
[    6.272893] ACPI Error: Region UserDefinedRegion (ID=146) has no handler (20150930/exfldio-297)
[    6.272903] ACPI Error: Method parse/execution failed [\_SB.PCI0.URT2._PS3] (Node ffff880074cb4bb8), AE_NOT_EXIST (20150930/psparse-542)
[    6.272924] acpi 8086228A:01: Failed to change power state to D3hot
[    6.323519] Bluetooth: Core ver 2.21
[    6.323558] NET: Registered protocol family 31
[    6.323562] Bluetooth: HCI device and connection manager initialized
[    6.323571] Bluetooth: HCI socket layer initialized
[    6.323578] Bluetooth: L2CAP socket layer initialized
[    6.323591] Bluetooth: SCO socket layer initialized
[    6.346940] Bluetooth: HCI UART driver ver 2.3
[    6.346949] Bluetooth: HCI UART protocol H4 registered
[    6.346952] Bluetooth: HCI UART protocol BCSP registered
[    6.346955] Bluetooth: HCI UART protocol LL registered
[    6.346958] Bluetooth: HCI UART protocol ATH3K registered
[    6.346960] Bluetooth: HCI UART protocol Three-wire (H5) registered
[    6.347072] Bluetooth: HCI UART protocol Intel registered
[    6.347113] Bluetooth: HCI UART protocol BCM registered
[    6.347116] Bluetooth: HCI UART protocol QCA registered
[    6.474374] usbcore: registered new interface driver btusb
[    6.490666] Bluetooth: hci0: read Intel version: 370810011003110e00
[    6.495924] Bluetooth: hci0: Intel Bluetooth firmware file: intel/ibt-hw-37.8.10-fw-1.10.3.11.e.bseq
[    6.595556] shpchp: Standard Hot Plug PCI Controller Driver version: 0.4
[    6.619913] intel_sst_acpi 808622A8:00: No matching machine driver found
[    6.651727] i2c_designware 808622C1:06: I2C bus managed by PUNIT
[    6.715765] Bluetooth: hci0: Intel Bluetooth firmware patch completed and activated
[    6.719187] Intel(R) Wireless WiFi driver for Linux
[    6.719195] Copyright(c) 2003- 2015 Intel Corporation
[    6.722297] iwlwifi 0000:01:00.0: Direct firmware load for iwlwifi-7265D-19.ucode failed with error -2
[    6.723126] iwlwifi 0000:01:00.0: Direct firmware load for iwlwifi-7265D-18.ucode failed with error -2
[    6.723182] iwlwifi 0000:01:00.0: Direct firmware load for iwlwifi-7265D-17.ucode failed with error -2
[    6.724027] iwlwifi 0000:01:00.0: Direct firmware load for iwlwifi-7265D-16.ucode failed with error -2
[    6.724087] iwlwifi 0000:01:00.0: Direct firmware load for iwlwifi-7265D-15.ucode failed with error -2
[    6.724130] iwlwifi 0000:01:00.0: Direct firmware load for iwlwifi-7265D-14.ucode failed with error -2
[    6.741362] iwlwifi 0000:01:00.0: loaded firmware version 25.30.13.0 op_mode iwlmvm
[    6.750252] i2c_designware 808622C1:06: punit semaphore timed out, resetting
[    6.750267] i2c_designware 808622C1:06: PUNIT SEM: 2
[    6.750269] ------------[ cut here ]------------
[    6.750285] WARNING: CPU: 2 PID: 340 at /home/kernel/COD/linux/drivers/i2c/busses/i2c-designware-baytrail.c:112 baytrail_i2c_acquire+0x139/0x1e0 [i2c_designware_platform]()
[    6.750356] Modules linked in: nls_iso8859_1 iwlwifi cfg80211 joydev input_leds snd_intel_sst_acpi snd_intel_sst_core snd_soc_sst_mfld_platform snd_soc_core snd_compress btusb mei_txe ac97_bus btrtl snd_pcm_dmaengine mei lpc_ich shpchp snd_pcm processor_thermal_device intel_soc_dts_iosf snd_seq_midi snd_seq_midi_event snd_rawmidi snd_seq hci_uart btbcm snd_seq_device btqca btintel snd_timer 8250_fintek bluetooth dw_dmac dw_dmac_core snd pwm_lpss_platform rfkill_gpio i2c_designware_platform(+) i2c_designware_core soundcore 8250_dw spi_pxa2xx_platform pwm_lpss soc_button_array int3400_thermal acpi_thermal_rel int3403_thermal int340x_thermal_zone acpi_pad mac_hid parport_pc ppdev lp parport autofs4 hid_generic usbhid mmc_block i915 i2c_algo_bit drm_kms_helper syscopyarea sysfillrect sysimgblt fb_sys_fops
[    6.750366]  drm i2c_hid fjes video hid wmi sdhci_acpi sdhci pinctrl_cherryview
[    6.750372] CPU: 2 PID: 340 Comm: systemd-udevd Not tainted 4.4.0-040400rc6-generic #201512202030
[    6.750374] Hardware name: HP HP Pavilion x2 Detachable/813E, BIOS F.06 11/02/2015
[    6.750379]  0000000000000000 000000002e5d75ef ffff88007bbafa28 ffffffff813c9124
[    6.750382]  0000000000000000 ffff88007bbafa60 ffffffff8107db92 ffff88007b1ae018
[    6.750386]  00000000ffffff92 00000000fffee185 ffff88007b958810 ffff88007b1b0120
[    6.750387] Call Trace:
[    6.750397]  [] dump_stack+0x44/0x60
[    6.750403]  [] warn_slowpath_common+0x82/0xc0
[    6.750406]  [] warn_slowpath_null+0x1a/0x20
[    6.750411]  [] baytrail_i2c_acquire+0x139/0x1e0 [i2c_designware_platform]
[    6.750417]  [] i2c_dw_init+0x27/0x350 [i2c_designware_core]
[    6.750421]  [] i2c_dw_probe+0x52/0x180 [i2c_designware_core]
[    6.750426]  [] dw_i2c_plat_probe+0x195/0x330 [i2c_designware_platform]
[    6.750432]  [] platform_drv_probe+0x3b/0x90
[    6.750436]  [] driver_probe_device+0x222/0x4a0
[    6.750440]  [] __driver_attach+0x84/0x90
[    6.750444]  [] ? driver_probe_device+0x4a0/0x4a0
[    6.750447]  [] bus_for_each_dev+0x6c/0xc0
[    6.750451]  [] driver_attach+0x1e/0x20
[    6.750454]  [] bus_add_driver+0x1eb/0x280
[    6.750457]  [] ? 0xffffffffc02e3000
[    6.750460]  [] driver_register+0x60/0xe0
[    6.750463]  [] __platform_driver_register+0x36/0x40
[    6.750468]  [] dw_i2c_init_driver+0x17/0x1000 [i2c_designware_platform]
[    6.750472]  [] do_one_initcall+0xb3/0x200
[    6.750478]  [] ? kmem_cache_alloc_trace+0x16b/0x1d0
[    6.750484]  [] do_init_module+0x5f/0x1e5
[    6.750488]  [] load_module+0x1603/0x1b70
[    6.750493]  [] ? __symbol_put+0x60/0x60
[    6.750499]  [] ? kernel_read+0x50/0x80
[    6.750502]  [] SyS_finit_module+0xb9/0xf0
[    6.750509]  [] entry_SYSCALL_64_fastpath+0x16/0x75
[    6.750512] ---[ end trace 331d8eb3ad32bcae ]---
[    6.750516] i2c_designware 808622C1:06: couldn't acquire bus ownership
[    6.750619] i2c_designware: probe of 808622C1:06 failed with error -110
[    6.838522] iwlwifi 0000:01:00.0: Detected Intel(R) Dual Band Wireless AC 3165, REV=0x210
[    6.839164] iwlwifi 0000:01:00.0: L1 Enabled - LTR Enabled
[    6.839762] iwlwifi 0000:01:00.0: L1 Enabled - LTR Enabled
[    6.865960] Adding 2015228k swap on /dev/mmcblk0p3.  Priority:-1 extents:1 across:2015228k SSFS
[    6.944560] ieee80211 phy0: Selected rate control algorithm 'iwl-mvm-rs'
[    6.994248] audit: type=1400 audit(1450913685.871:2): apparmor="STATUS" operation="profile_load" name="/usr/lib/lightdm/lightdm-guest-session" pid=718 comm="apparmor_parser"
[    6.994268] audit: type=1400 audit(1450913685.871:3): apparmor="STATUS" operation="profile_load" name="chromium" pid=718 comm="apparmor_parser"
[    7.003086] audit: type=1400 audit(1450913685.879:4): apparmor="STATUS" operation="profile_load" name="/sbin/dhclient" pid=718 comm="apparmor_parser"
[    7.003106] audit: type=1400 audit(1450913685.879:5): apparmor="STATUS" operation="profile_load" name="/usr/lib/NetworkManager/nm-dhcp-client.action" pid=718 comm="apparmor_parser"
[    7.003119] audit: type=1400 audit(1450913685.879:6): apparmor="STATUS" operation="profile_load" name="/usr/lib/NetworkManager/nm-dhcp-helper" pid=718 comm="apparmor_parser"
[    7.003131] audit: type=1400 audit(1450913685.879:7): apparmor="STATUS" operation="profile_load" name="/usr/lib/connman/scripts/dhclient-script" pid=718 comm="apparmor_parser"
[    7.050299] SSE version of gcm_enc/dec engaged.
[    7.095823] audit: type=1400 audit(1450913685.971:8): apparmor="STATUS" operation="profile_load" name="/usr/bin/evince" pid=718 comm="apparmor_parser"
[    7.095844] audit: type=1400 audit(1450913685.971:9): apparmor="STATUS" operation="profile_load" name="sanitized_helper" pid=718 comm="apparmor_parser"
[    7.095858] audit: type=1400 audit(1450913685.971:10): apparmor="STATUS" operation="profile_load" name="/usr/bin/evince-previewer" pid=718 comm="apparmor_parser"
[    7.095871] audit: type=1400 audit(1450913685.971:11): apparmor="STATUS" operation="profile_load" name="sanitized_helper" pid=718 comm="apparmor_parser"
[    7.295815] intel_rapl: Found RAPL domain package
[    7.295825] intel_rapl: Found RAPL domain core
[    7.413203] iwlwifi 0000:01:00.0 wlp1s0: renamed from wlan0
[    7.440312] cfg80211: World regulatory domain updated:
[    7.440322] cfg80211:  DFS Master region: unset
[    7.440325] cfg80211:   (start_freq - end_freq @ bandwidth), (max_antenna_gain, max_eirp), (dfs_cac_time)
[    7.440331] cfg80211:   (2402000 KHz - 2472000 KHz @ 40000 KHz), (N/A, 2000 mBm), (N/A)
[    7.440335] cfg80211:   (2457000 KHz - 2482000 KHz @ 40000 KHz), (N/A, 2000 mBm), (N/A)
[    7.440339] cfg80211:   (2474000 KHz - 2494000 KHz @ 20000 KHz), (N/A, 2000 mBm), (N/A)
[    7.440344] cfg80211:   (5170000 KHz - 5250000 KHz @ 80000 KHz, 160000 KHz AUTO), (N/A, 2000 mBm), (N/A)
[    7.440348] cfg80211:   (5250000 KHz - 5330000 KHz @ 80000 KHz, 160000 KHz AUTO), (N/A, 2000 mBm), (0 s)
[    7.440353] cfg80211:   (5490000 KHz - 5730000 KHz @ 160000 KHz), (N/A, 2000 mBm), (0 s)
[    7.440356] cfg80211:   (5735000 KHz - 5835000 KHz @ 80000 KHz), (N/A, 2000 mBm), (N/A)
[    7.440360] cfg80211:   (57240000 KHz - 63720000 KHz @ 2160000 KHz), (N/A, 0 mBm), (N/A)
[    7.521405] input: SYNA7508:00 06CB:1613 Pen as /devices/pci0000:00/808622C1:05/i2c-13/i2c-SYNA7508:00/0018:06CB:1613.0003/input/input8
[    7.521849] input: SYNA7508:00 06CB:1613 as /devices/pci0000:00/808622C1:05/i2c-13/i2c-SYNA7508:00/0018:06CB:1613.0003/input/input9
[    7.522338] hid-multitouch 0018:06CB:1613.0003: input,hidraw2: I2C HID v1.00 Device [SYNA7508:00 06CB:1613] on i2c-SYNA7508:00
[    7.658762] NMI watchdog: enabled on all CPUs, permanently consumes one hw-PMU counter.
[    7.906121] cgroup: new mount options do not match the existing superblock, will be ignored
[    7.985927] input: HP WMI hotkeys as /devices/virtual/input/input7
[    8.060040] Bluetooth: BNEP (Ethernet Emulation) ver 1.3
[    8.060049] Bluetooth: BNEP filters: protocol multicast
[    8.060059] Bluetooth: BNEP socket layer initialized
[    8.125592] random: nonblocking pool is initialized
[    8.494767] IPv6: ADDRCONF(NETDEV_UP): wlp1s0: link is not ready
[    8.495543] iwlwifi 0000:01:00.0: L1 Enabled - LTR Enabled
[    8.496347] iwlwifi 0000:01:00.0: L1 Enabled - LTR Enabled
[    8.559499] iwlwifi 0000:01:00.0: L1 Enabled - LTR Enabled
[    8.560157] iwlwifi 0000:01:00.0: L1 Enabled - LTR Enabled
[    8.590317] IPv6: ADDRCONF(NETDEV_UP): wlp1s0: link is not ready
[    8.740744] IPv6: ADDRCONF(NETDEV_UP): wlp1s0: link is not ready
[   11.409798] Bluetooth: RFCOMM TTY layer initialized
[   11.409820] Bluetooth: RFCOMM socket layer initialized
[   11.409838] Bluetooth: RFCOMM ver 1.11
[   12.067751] wlp1s0: authenticate with f8:8e:85:af:b8:47
[   12.081718] wlp1s0: send auth to f8:8e:85:af:b8:47 (try 1/3)
[   12.179067] wlp1s0: authenticated
[   12.182598] wlp1s0: associate with f8:8e:85:af:b8:47 (try 1/3)
[   12.190899] wlp1s0: RX AssocResp from f8:8e:85:af:b8:47 (capab=0x411 status=0 aid=2)
[   12.196734] wlp1s0: associated
[   12.196817] IPv6: ADDRCONF(NETDEV_CHANGE): wlp1s0: link becomes ready
</pre>
<br />
<br />

---

## 3 comments captured from [original post](https://snarkybrill.blogspot.com/2015/12/ubuntu-hp-pavilion-x2-10-n109nc.html) on Blogger

**Tomas Hlavacek said on 2016-01-04**

There are photos from the laptop disassembly and all captured outputs (lspci, dmesg, dmidecode, ...) here: http://aule.elfove.cz/~brill/hpx2/<br /><br />And there is also a thread on this laptop here: http://ubuntuforums.org/showthread.php?t=2307493<br /><br />I found that the audio codec is probably some Conexant CX2072x chip but inside the laptop there was only a chip with markings CX7601-11Z. And I was unable to find a datasheet for this guy. I am most likely giving it up...

**Unknown said on 2016-10-26**

This same codec (cx2072x) is on the Asus e200HA and the Asus T100HA.

**Tomas Hlavacek said on 2016-10-26**

Which is most likely the true reason for &quot;audio doesn't work on Linux&quot;: https://wiki.debian.org/InstallingDebianOn/Asus/E200HA

