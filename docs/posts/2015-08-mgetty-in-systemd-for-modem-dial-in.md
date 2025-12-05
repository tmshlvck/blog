---
date: 2015-08-12T02:26:00.002+02:00
description: ''
published: true
slug: 2015-08-mgetty-in-systemd-for-modem-dial-in
tags:
- Debian
- Linux
- legacy-blogger
readtime: 5
title: mgetty in systemd for modem dial-in server
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2015/08/mgetty-in-systemd-for-modem-dial-in.html)*.

I used to have a modem dial-in server as an out-of-band management for key network elements. The idea was to call the server over the GSM from another place with minicom, connect to the terminal of the server and then use another serial consoles that was connected from the server to routers, switches etc...

So far so good, I used to have a simple **/etc/inittab** line like this:

```
T3:23:respawn:/sbin/mgetty -D -s 115200 ttyS0
```

But... The \*\*\*\*ing almighty **systemd** came to create hell on earth for Linux users... No **/etc/inittab** anymore, no nothing. Well, UTFG, so I get completely wrong advice to try:

```
# systemctl start getty@ttyS0.service
```

And that's it... (?) No, it isn't! It simply does not accept the call, because it uses agetty and agetty can't do that or it is not configured or whatever.

After one particularly hot, unpleasant and exhausting afternoon spent on experimenting with this I came to following config for systemd that works for me (place it to **/etc/systemd/system/mgetty.service**):

```
[Unit]
Description=Smart Modem Getty(mgetty)
Documentation=man:mgetty(8)
Requires=systemd-udev-settle.service
After=systemd-udev-settle.service

[Service]
Type=simple
ExecStart=/sbin/mgetty -D -s 115200 /dev/ttyS0
Restart=always
PIDFile=/var/run/mgetty.pid.ttyS0

[Install]
WantedBy=multi-user.target
```

And of course:

```
# systemctl start mgetty.service
# systemctl enable mgetty.service
```

I hate systemd! I really do. But I'll learn about it. It has spread to all reasonable distros like a nasty infection so we have to learn living with it, I guess.

---

## 5 comments captured from [original post](https://snarkybrill.blogspot.com/2015/08/mgetty-in-systemd-for-modem-dial-in.html) on Blogger

**Unknown said on 2015-08-21**

Thanks.  Finally surrendered to systemd since it seemed inevitable, and needed my trusty faxmodem still receiving for legal documents.

**Unknown said on 2018-07-20**

I didn't fully understand this ...
(still reeling from the systemd crap)

I created a directory entry:

serial-getty@ttyUSB0.service.d

much like the one I created for ttyS0 where my terminal is, but the modem just keeps running. It's treating the modem like a terminal.

How did you specify that it use mgetty instead of agetty?

Sorry ... still trying to grasp this.

/etc/inittab was soooooo much easier!

-rAllcorn-

**Rich Allcorn said on 2018-08-10**

After reviewing this, I have come to the conclusion that "YOU SIR" are the ONLY ONE who ever explained this on how to do with a "systemd infected system".

THANK YOU SO VERY MUCH!!!!


I am copying the file I created, to make things simple, and to honor YOUR labors ... again, I thank you!!

(see following post)

**Rich Allcorn said on 2018-08-10**

```
# /etc/systemd/system/mgetty.service
#
# credit for creating this goes to:
# "Snarky Brill", who posted a guide that helped me to create this one
#
#  see URL:
#  https://snarkybrill.blogspot.com/2015/08/mgetty-in-systemd-for-modem-dial-in.html?showComment=1533868591623#c3913812844269156303
#
#
# This file enables model dial-in services using mgetty for ttyUSB0
# where the modem resides on my system.
#
# You can tailor this for whatever serial port your modem may reside
#
#
# Copy this file to:  /etc/systemd/system

[Unit]
Description=Smart Modem Getty(mgetty)
Documentation=man:mgetty(8)
Requires=systemd-udev-settle.service
After=systemd-udev-settle.service

[Service]
Type=simple
ExecStart=/sbin/mgetty -D -s 38400 /dev/ttyUSB0
Restart=always
PIDFile=/var/run/mgetty.pid.ttyUSB0

[Install]
WantedBy=multi-user.target


# Don't forget to run the following commands:
#
#
# $ sudo systemctl start mgetty.service
# $ sudo systemctl enable mgetty.service
#
#
# ... after copying the file to /etc/systemd/system/
#
```

**Unknown said on 2018-12-13**

Thank you very much.

I am the author of mgetty, but I stopped using Linux a while ago due to systemd plague - and today I needed to install mgetty on a customer's Ubuntu system.  Which brought a nice precompiled package, but NO UNIT FILE...

Now I have one, thanks to your blog :-)
