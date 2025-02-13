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

<h2 style="text-align: left;">ProxMox OSD create:&nbsp;No Disks unused</h2><div>I just realized that when I destroy Ceph OSD in ProxMox 7.0-11&nbsp;- either in Web UI or using console</div><pre># pveceph osd destroy X --cleanup 1
</pre>

I can't create new OSD on the same disk. ProxMox simply says that there are "No Disks unused" in the Web UI OSD create form. And in console I get:
<pre># pveceph osd create /dev/sdX
device '/dev/sdX' is already in use
</pre>

Zaping disk with the customary<pre style="text-align: left;"># ceph-volume lvm zap /dev/sdX --destroy
</pre><div style="text-align: left;">
does not help, even though it is often very useful command.</div><h2 style="text-align: left;">Workaround</h2><div style="text-align: left;"><span style="font-weight: normal;">First I realized that rebooting the machine helps. But... Rebooting hyperconverged hypervisor to see the perfectly working disk drive in self-healing unbreakable unstoppable storage? In 2021? Seriously???

I realized that a bucnch of crazy Perl libraries that do everything in ProxMox call this command:
</span></div><pre># lsblk --json -o path,parttype,fstype
{
   "blockdevices": [
...
      {"path":"/dev/sdX", "parttype":null, "fstype":"LVM2_member"},
...
</pre>
and they consider everything with <i>fstype</i> other than <i>null</i> to be <b>"used"</b>. The solution is to create any partition table on the /dev/sdX. For example just running
<pre># fdisk /dev/sdX
</pre>
and pressing "<b>w</b>" to save empty table is enough for the disk to re-appear.