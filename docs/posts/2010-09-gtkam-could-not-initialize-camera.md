---
date: 2010-09-04T06:32:00.001+02:00
description: ''
published: true
slug: 2010-09-gtkam-could-not-initialize-camera
tags:
- Linux
- gtkam
- Gnome
- legacy-blogger
readtime: 5
title: 'gtkam: Could not initialize camera.'
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2010/09/gtkam-could-not-initialize-camera.html)*.

I have Canon Ixus 80IS. Canon cameras have **"a feature"** that they are not a USB mass storage. Maybe they have some **\*\*\*\*ing** reason for this, noone knows...

Under Linux, one needs `gphoto2` in order to download photos from the camera on USB. And one also needs `usbfs` mounted to `/proc/bus/usb` and of course one needs proper permissions. For example on current Debian unstable I had to add this line to `/etc/fstab`:

```
none  /proc/bus/usb  usbfs  defaults  0  0
```

And one also needs a user accessing the camera to be in group **plugdev**. Permissions on devices in `/proc/bus/usb/...` are set by some of beasts like udev/hal/wtf. No matter, plugdev did the trick for me.
