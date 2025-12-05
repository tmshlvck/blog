---
date: 2016-05-01T01:13:00.000+02:00
description: ''
published: true
slug: 2016-05-running-vino-vnc-server-on-ubuntu-mate
tags:
- Linux
- Ubuntu
- desktop
- legacy-blogger
readtime: 5
title: Running vino (VNC server) on Ubuntu Mate
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2016/05/running-vino-vnc-server-on-ubuntu-mate.html)*.

Simple how-to:

- `sudo apt-get install vino dconf-tools`
- run vino-preferences
  - enable remote access
  - disable confirmation of the access
- run dconf-editor
  - go to org -> gnome -> desktop -> remote-access
  - change network-interface to lo
  - uncheck require-encryption
  - clear vnc-password
- create startup application for Vino VNC server
  - run `/usr/lib/vino/vino-server --sm-client-disable`
- logout & login

---

## 2 comments captured from [original post](https://snarkybrill.blogspot.com/2016/05/running-vino-vnc-server-on-ubuntu-mate.html) on Blogger

**Mr. project hacker said on 2017-10-23**

Which ver of Mate?

**Tomas Hlavacek said on 2019-01-16**

From Trusty on...
Though lately (I am not sure when), the vino-preferences has been removed and the dconf-editor has its own package (named dconf-editor). At least on Cosmic it is like that.
