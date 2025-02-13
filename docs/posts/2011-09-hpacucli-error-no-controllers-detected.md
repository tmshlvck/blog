---
date: 2011-09-27T12:07:00.001+02:00
description: ''
published: true
slug: 2011-09-hpacucli-error-no-controllers-detected
tags:
- Debian
- HW
- legacy-blogger
readtime: 5
title: 'hpacucli: Error: No controllers detected. with hpsa and SmartArray P410i'
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/09/hpacucli-error-no-controllers-detected.html)*.

I have encountered a <b>****ing</b> weird problem with&nbsp;hpacucli (HP utility for their RAID controllers) on ProLiant DL360 G7 or whatever. The controller was HP SmartArray P410i and the problem was simple:<br />
<br />
<pre>root@XYZ:~# hpacucli 
HP Array Configuration Utility CLI 8.70-8.0
Detecting Controllers...Done.
Type "help" for a list of supported commands.
Type "exit" to close the console.

=&gt; ctrl all show

Error: No controllers detected.
</pre><br />
The solution is: <b>Load the sg driver</b> and that's it:<br />
<br />
<pre>root@XYZ:~# modprobe sg
root@XYZ:~# hpacucli 
HP Array Configuration Utility CLI 8.70-8.0
Detecting Controllers...Done.
Type "help" for a list of supported commands.
Type "exit" to close the console.

=> ctrl all show

Smart Array P410i in Slot 0 (Embedded)    (sn: 500143801630C980)
</pre>

---

## 3 comments captured from [original post](https://snarkybrill.blogspot.com/2011/09/hpacucli-error-no-controllers-detected.html) on Blogger

**Unknown said on 2011-11-17**

Thanks for the post...this got me going. Really appreciate you sharing this.

**Sebastian Muñiz said on 2012-07-20**

Adding more information to the same stupid error:<br />If you are using kernel 3.x series, hpacucli will still say<br /># /usr/sbin/hpacucli ctrl all show config<br />Error: No controllers detected.<br />even with sg loaded.<br /><br />Some caritative sould has written a wrapper called &quot;uname-26&quot;. Download compile and voilá<br /><br />root@agrohost:/usr/src/uname-26# ./uname26 /usr/sbin/hpacucli ctrl all show config<br /><br />Smart Array P410i in Slot 0 (Embedded)    (sn: 50014380086260C0)<br /><br />   array A (SATA, Unused Space: 0 MB)<br /><br /><br />      logicaldrive 1 (465.7 GB, RAID 1, OK)<br /><br />      physicaldrive 1I:1:1 (port 1I:box 1:bay 1, SATA, 500 GB, OK)<br />      physicaldrive 1I:1:2 (port 1I:box 1:bay 2, SATA, 500 GB, OK)<br /><br />   SEP (Vendor ID PMCSIERA, Model  SRC 8x6G) 250 (WWID: 50014380086260CF)

**Tomas Hlavacek said on 2013-01-13**

Thank you very much Sebastian! I have also hit this bug. It is insane that HP utilities rely on the string &quot;2.6&quot; and it does not work with 3.x kernels. Really I do not follow but I think they are amateurs and their HW and eventual Linux support is crap.<br /><br />It is worse than ever with buggy Broadcom NICs. I had problems with multicast@NeXtreme 2 (BCM5709) and I was unable to solve it. Finally I decided to use different server from a different a better vendor with Intel 82576 and it works out of box.

