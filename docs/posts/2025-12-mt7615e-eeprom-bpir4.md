---
date: 2025-12-04T23:00:00.000+01:00
description: 'How to fix MT7615E PCIe WiFi card with missing EEPROM on Banana Pi R4 running Armbian'
published: true
slug: 2025-12-mt7615e-eeprom-bpir4
tags:
- Linux
- Networking
- WiFi
- Armbian
- BananaPi
readtime: 8
title: Fixing MT7615E WiFi card with missing EEPROM on Banana Pi R4
---

## The Problem

I have a [Banana Pi BPI-MT7615 802.11 ac WiFi card](https://docs.banana-pi.org/en/BPI-MT7615/BananaPi_MT7615) installed in my Banana Pi R4 running Armbian:

```
$ lspci
0000:00:00.0 PCI bridge: MEDIATEK Corp. MT7988 PCI Express Host Bridge [Filogic 880] (rev 01)
0000:01:00.0 Unclassified device [0002]: MEDIATEK Corp. MT7615E 802.11ac PCI Express Wireless Network Adapter
```

The card was recognized and the driver loaded, but there was a serious issue: TX power was stuck at 3 dBm regardless of what I tried to set:

```
$ sudo iw phy0 set txpower fixed 2000  # trying to set 20 dBm
$ sudo iw dev
phy#0
        Interface wlp1s0
...
                txpower 3.00 dBm    # still 3 dBm!
```

This made the card practically useless - 3 dBm is about 2 milliwatts, barely enough to reach the next room.

## Root Cause

After some research, I found the culprit: My MT7615E card lacks an onboard EEPROM chip. The EEPROM contains calibration data including TX power limits for each channel and band. Without it, the mt76 driver falls back to minimal safe defaults - hence the 3 dBm limit.

This is a known issue with certain MT7615E development boards and cheap cards. The relevant discussions:

- [OpenWRT Forum: TX power with mt76 driver](https://forum.openwrt.org/t/tx-power-with-mt76-driver/155933)
- [Banana Pi Forum: 802.11ac module gives max 6dBi transmitter power](https://forum.banana-pi.org/t/802-11ac-module-gives-max-6dbi-transmitter-power/14238/22)

## The Solution

The fix requires two components:

1. A kernel patch that allows the mt76 driver to load EEPROM data from a file
2. An appropriate EEPROM binary file with proper calibration data

### Kernel Patch

Frank Wunderlich created a patch for the BPI-Router-Linux kernel that adds filesystem-based EEPROM loading. The original patch is available at [Frank-W's BPI-Router-Linux repository](https://github.com/frank-w/BPI-Router-Linux/commit/fe31dae2ab5c2fbd39b6609845920969d10dd465).

I adapted this patch for the Armbian build system targeting kernel 6.16. The patch adds a `mt76_get_eeprom_file()` function that looks for EEPROM data at `/lib/firmware/mediatek/{driver}_rf.bin`. For MT7615E, this means `/lib/firmware/mediatek/mt7615e_rf.bin`.

The patch is available in my Armbian fork: [tmshlvck/armbian-build bpir4 branch](https://github.com/tmshlvck/armbian-build/tree/bpir4)

Specifically:

- [The patch file](https://github.com/tmshlvck/armbian-build/blob/bpir4/patch/kernel/archive/filogic-6.16/patches.armbian/0085-mt76-add-eeprom-file-loading-support.patch)
- [series.conf with the patch included](https://github.com/tmshlvck/armbian-build/blob/bpir4/patch/kernel/archive/filogic-6.16/series.conf)

### EEPROM Binary File

You need a working EEPROM image for your card. I'm providing the one that works for my MT7615E card with unlocked power limits:

**Download:** [mt7615e_rf.bin](../files/mt7615e_rf.bin) (19880 bytes)

Alternative sources for EEPROM files:

- [Eric Woud's buildR64ubuntu firmware](https://github.com/ericwoud/buildR64ubuntu/tree/master/linux-5.16-rc3/firmware/mediatek) - contains various mt76 EEPROM files
- Extract from another MT7615E card with working EEPROM via its factory MTD partition

## Installation

### 1. Build Armbian with the patch

Clone my Armbian fork and build:

```bash
git clone -b bpir4 https://github.com/tmshlvck/armbian-build.git
cd armbian-build
./compile.sh BOARD=bananapi-r4 BRANCH=edge kernel
```

Or apply the patch manually to your own Armbian build.

### 2. Install the EEPROM file

Copy the EEPROM binary to the target system:

```bash
sudo mkdir -p /lib/firmware/mediatek/
sudo cp mt7615e_rf.bin /lib/firmware/mediatek/
```

### 3. Reboot and verify

After rebooting, check dmesg for successful EEPROM loading:

```bash
dmesg | grep -i eeprom
```

You should see something like:

```
mt7615e 0000:01:00.0: Load eeprom: /lib/firmware/mediatek/mt7615e_rf.bin
mt7615e 0000:01:00.0: Load eeprom OK, count 19880 byte
```

Now verify TX power can be set properly:

```bash
sudo iw phy0 set txpower fixed 2000  # 20 dBm
sudo iw dev
```

The interface should now show the requested TX power (within regulatory limits).

## Technical Details

The patch works by modifying `drivers/net/wireless/mediatek/mt76/eeprom.c` to try loading EEPROM from filesystem before falling back to device tree or NVMEM sources. The file path is constructed as `/lib/firmware/mediatek/{driver_name}_rf.bin`, where `driver_name` comes from the kernel driver (e.g., `mt7615e`, `mt7915e`, etc.).

This approach was proposed upstream but wasn't merged as OpenWRT prefers device tree based solutions. However, for development boards and cards without factory-programmed EEPROM, the filesystem approach is practical and flexible.

## Important Notes

- Different EEPROM files may have different regulatory domain settings affecting available channels and power levels
- The EEPROM contains calibration data specific to the hardware - using wrong data could result in suboptimal performance or regulatory violations
- This patch is not upstream in mainline Linux or Armbian; you'll need to maintain it yourself or use my fork

## References

- [OpenWRT mt76 Issue #949: MT7615 eeprom binary from filesystem](https://github.com/openwrt/mt76/issues/949)
- [Frank Wunderlich's BPI-Router-Linux](https://github.com/frank-w/BPI-Router-Linux)
- [My Armbian fork with the patch](https://github.com/tmshlvck/armbian-build/tree/bpir4)
- [Banana Pi BPI-R4 Wiki](https://wiki.banana-pi.org/Banana_Pi_BPI-R4)
