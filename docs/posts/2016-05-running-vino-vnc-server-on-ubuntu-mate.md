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
time_to_read: 5
title: Running vino (VNC server) on Ubuntu Mate
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2016/05/running-vino-vnc-server-on-ubuntu-mate.html)*.

Simple how-to:<br />
<br />
<ul>
<li>sudo apt-get install vino dconf-tools</li>
<li>run vino-preferences</li>
<ul>
<li>enable remote access</li>
<li>disable confirmation of the access</li>
</ul>
<li>run dconf-editor</li>
<ul>
<li>go to org -&gt; gnome -&gt; desktop -&gt; remote-access</li>
<li>change network-interface to lo</li>
<li>uncheck require-encryption</li>
<li>clear vnc-password</li>
</ul>
<li>create startup application for Vino VNC server</li>
<ul>
<li>run&nbsp;/usr/lib/vino/vino-server --sm-client-disable</li>
</ul>
<li>logout &amp; login</li>
</ul>

---

## 2 comments captured from [original post](https://snarkybrill.blogspot.com/2016/05/running-vino-vnc-server-on-ubuntu-mate.html) on Blogger

**Mr. project hacker said on 2017-10-23**

Which ver of Mate?

**Tomas Hlavacek said on 2019-01-16**

From Trusty on...<br />Though lately (I am not sure when), the vino-preferences has been removed and the dconf-editor has its own package (named dconf-editor). At least on Cosmic it is like that.

