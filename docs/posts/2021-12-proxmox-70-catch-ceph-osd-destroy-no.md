---
date: 2021-12-09T02:32:00.002+01:00
description: ''
published: true
slug: 2021-12-proxmox-70-catch-ceph-osd-destroy-no
tags:
- Linux
- Ceph
- ProxMox
- legacy-blogger
readtime: 5
title: 'ProxMox 7.0 catch - Ceph OSD destroy: No Disks unused'
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2021/12/proxmox-70-catch-ceph-osd-destroy-no.html)*.

## ProxMox OSD create: No Disks unused

I just realized that when I destroy Ceph OSD in ProxMox 7.0-11 - either in Web UI or using console

```
# pveceph osd destroy X --cleanup 1
```

I can't create new OSD on the same disk. ProxMox simply says that there are "No Disks unused" in the Web UI OSD create form. And in console I get:

```
# pveceph osd create /dev/sdX
device '/dev/sdX' is already in use
```

Zaping disk with the customary

```
# ceph-volume lvm zap /dev/sdX --destroy
```

does not help, even though it is often very useful command.

## Workaround

First I realized that rebooting the machine helps. But... Rebooting hyperconverged hypervisor to see the perfectly working disk drive in self-healing unbreakable unstoppable storage? In 2021? Seriously???

I realized that a bucnch of crazy Perl libraries that do everything in ProxMox call this command:

```
# lsblk --json -o path,parttype,fstype
{
   "blockdevices": [
...
      {"path":"/dev/sdX", "parttype":null, "fstype":"LVM2_member"},
...
```

and they consider everything with _fstype_ other than _null_ to be **"used"**. The solution is to create any partition table on the /dev/sdX. For example just running

```
# fdisk /dev/sdX
```

and pressing "**w**" to save empty table is enough for the disk to re-appear.
