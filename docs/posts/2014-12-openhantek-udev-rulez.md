---
date: 2014-12-10T03:43:00.000+01:00
description: ''
published: true
slug: 2014-12-openhantek-udev-rulez
tags:
- Debian
- Linux
- HW
- Linux Mint
- legacy-blogger
time_to_read: 5
title: OpenHantek udev rulez
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2014/12/openhantek-udev-rulez.html)*.

I have a small and cheap Hantek DSO-2090 USB oscilloscope... And, well, the HW is old, cheap Chinese box that looks extremely ugly. I have not enough bravery to look inside but I do not hope for anything good. But it was cheap enough and when I was moving I really needed to get rid of my old Russian 15 kg, 0.5 cubic meter CRT oscilloscope so I decided to buy this one. It was conscious choice and it works for me pretty well since I am doing only basic electronic measurements here.<br />
<br />
But I migrated to a newer Debian system so I had to rebuild my OpenHantek SW, which was pretty painless. Many thanks to the author of this blog post: <a href="http://verahill.blogspot.cz/2012/12/298-hantek-dso-2250-usb-with-openhantek.html">http://verahill.blogspot.cz/2012/12/298-hantek-dso-2250-usb-with-openhantek.html</a><br />
<br />
But there were still a problem with udev that failed to change group and access rights to the /dev/bus/usb/... file, so I was unable to use OpenHantek as an ordinary user. The solution is obvious, the old udev rule file used SYSFS instead of ATTR. This is my working version:<br />
<br />
<br />
<pre># Hantek DSO-2090
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", ENV{PRODUCT}=="4b4/2090/*", RUN+="/sbin/fxload -t fx2 -I /usr/local/share/hantek/dso2090-firmware.hex -s /usr/local/share/hantek/dso2090-loader.hex -D $env{DEVNAME}"
ATTR{idVendor}=="04b5", ATTR{idProduct}=="2090", MODE="0660", GROUP="plugdev"

# Hantek DSO-2100
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", ENV{PRODUCT}=="547/1006/*", RUN+="/sbin/fxload -t an21 -I /usr/local/share/hantek/dso2100-firmware.hex -s /usr/local/share/hantek/dso2100-loader.hex -D $env{DEVNAME}"
ATTR{idVendor}=="0547", ATTR{idProduct}=="1002", MODE="0660", GROUP="plugdev"

# Hantek DSO-2150
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", ENV{PRODUCT}=="4b4/2150/*", RUN+="/sbin/fxload -t fx2 -I /usr/local/share/hantek/dso2150-firmware.hex -s /usr/local/share/hantek/dso2150-loader.hex -D $env{DEVNAME}"
ATTR{idVendor}=="04b5", ATTR{idProduct}=="2150", MODE="0660", GROUP="plugdev"

# Hantek DSO-2250
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", ENV{PRODUCT}=="4b4/2250/*", RUN+="/sbin/fxload -t fx2 -I /usr/local/share/hantek/dso2250-firmware.hex -s /usr/local/share/hantek/dso2250-loader.hex -D $env{DEVNAME}"
ATTR{idVendor}=="04b5", ATTR{idProduct}=="2250", MODE="0660", GROUP="plugdev"

# Hantek DSO-5200
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", ENV{PRODUCT}=="4b4/5200/*", RUN+="/sbin/fxload -t fx2 -I /usr/local/share/hantek/dso5200-firmware.hex -s /usr/local/share/hantek/dso5200-loader.hex -D $env{DEVNAME}"
ATTR{idVendor}=="04b5", ATTR{idProduct}=="5200", MODE="0660", GROUP="plugdev"

# Hantek DSO-5200A
SUBSYSTEM=="usb", ACTION=="add", ENV{DEVTYPE}=="usb_device", ENV{PRODUCT}=="4b4/520A/*", RUN+="/sbin/fxload -t fx2 -I /usr/local/share/hantek/dso520a-firmware.hex -s /usr/local/share/hantek/dso520a-loader.hex -D $env{DEVNAME}"
ATTR{idVendor}=="04b5", ATTR{idProduct}=="520A", MODE="0660", GROUP="plugdev"
</pre>
<br />
Assuming the path to the *.hex file generated by the openhantek-extractfw is /usr/local/share/hantek .

---

## 2 comments captured from [original post](https://snarkybrill.blogspot.com/2014/12/openhantek-udev-rulez.html) on Blogger

**partinis said on 2018-02-09**

Hello,<br /><br />I have just tried it and have exactly the same problem as described by Mladen on https://sourceforge.net/p/openhantek/discussion/1153137/thread/d4f2beda/.<br /><br /><br />Please do you have any suggestions to solve it?<br /><br /><br />My device:<br /><br />ID 0547:1006 Anchor Chips, Inc. Hantek DSO-2100 UF<br /><br />ID 0547:1002 Anchor Chips, Inc. Python2 WDM Encoder (after loading firmware with fxload)<br /><br /><br />Using following sources I was able to achieve best results ever (Scope is detected and relay clicks are hearable)<br /><br />http://svn.code.sf.net/p/openhantek/code/trunk<br /><br /><br />I have also tried:<br /><br />https://github.com/OpenHantek/openhantek<br /><br />http://verahill.blogspot.de/2012/12/298-hantek-dso-2250-usb-with-openhantek.html<br /><br />However without any success…<br /><br /><br />Regards,<br />partinis<br />

**Tomas Hlavacek said on 2018-02-09**

I suggest you to ask on GitHub (create issue in OpenHantek project perhaps). It seems that developer is unexpectedly helpful, responds quickly and tries to solve your problems.<br /><br />I know, unfortunately just my version (DSO-2090) and is works out of box with current OpenHantek cloned from GitHub.
