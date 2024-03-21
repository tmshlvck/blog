---
date: 2015-08-12T02:26:00.002+02:00
description: ''
published: true
slug: 2015-08-mgetty-in-systemd-for-modem-dial-in
tags:
- Debian
- Linux
- legacy-blogger
time_to_read: 5
title: mgetty in systemd for modem dial-in server
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2015/08/mgetty-in-systemd-for-modem-dial-in.html)*.

I used to have a modem dial-in server as an out-of-band management for key network elements. The idea was to call the server over the GSM from another place with minicom, connect to the terminal of the server and then use another serial consoles that was connected from the server to routers, switches etc...<br />
<br />
So far so good, I used to have a simple <b>/etc/inittab</b> line like this:<br />
<br />
<pre>T3:23:respawn:/sbin/mgetty -D -s 115200 ttyS0
</pre>
<br />
But... The ****ing almighty&nbsp;<b>systemd</b> came to create hell on earth for Linux users... No <b>/etc/inittab</b> anymore, no nothing. Well, UTFG, so I get completely wrong advice to try:<br />
<br />
<pre># systemctl start getty@ttyS0.service
</pre>
<br />
And that's it... (?) No, it isn't! It simply does not accept the call, because it uses agetty and agetty can't do that or it is not configured or whatever.<br />
<br />
After one particularly hot, unpleasant and exhausting afternoon spent on experimenting with this I came to following config for systemd that works for me (place it to <b>/etc/systemd/system/mgetty.service</b>):<br />
<br />
<pre>[Unit]
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
</pre>
<br />
And of course:<br />
<br />
<pre># systemctl start mgetty.service
# systemctl enable mgetty.service
</pre>
<br />
I hate systemd! I really do. But I'll learn about it. It has spread to all reasonable distros like a nasty infection so we have to learn living with it, I guess.

---

## 5 comments captured from [original post](https://snarkybrill.blogspot.com/2015/08/mgetty-in-systemd-for-modem-dial-in.html) on Blogger

**Unknown said on 2015-08-21**

Thanks.  Finally surrendered to systemd since it seemed inevitable, and needed my trusty faxmodem still receiving for legal documents.

**Unknown said on 2018-07-20**

I didn't fully understand this ...<br />(still reeling from the systemd crap)<br /><br />I created a directory entry:<br /><br />serial-getty@ttyUSB0.service.d<br /><br />much like the one I created for ttyS0 where my terminal is, but the modem just keeps running. It's treating the modem like a terminal.<br /><br />How did you specify that it use mgetty instead of agetty?<br /><br />Sorry ... still trying to grasp this.<br /><br />/etc/inittab was soooooo much easier!<br /><br />-rAllcorn-

**Rich Allcorn said on 2018-08-10**

After reviewing this, I have come to the conclusion that &quot;YOU SIR&quot; are the ONLY ONE who ever explained this on how to do with a &quot;systemd infected system&quot;.  <br /><br />THANK YOU SO VERY MUCH!!!!<br /><br /><br />I am copying the file I created, to make things simple, and to honor YOUR labors ... again, I thank you!!<br /><br />(see following post)

**Rich Allcorn said on 2018-08-10**

# /etc/systemd/system/mgetty.service<br />#<br /># credit for creating this goes to:  <br /># &quot;Snarky Brill&quot;, who posted a guide that helped me to create this one<br />#<br />#  see URL:  <br />#  https://snarkybrill.blogspot.com/2015/08/mgetty-in-systemd-for-modem-dial-in.html?showComment=1533868591623#c3913812844269156303<br />#<br />#<br /># This file enables model dial-in services using mgetty for ttyUSB0<br /># where the modem resides on my system.  <br />#<br /># You can tailor this for whatever serial port your modem may reside <br />#<br />#<br /># Copy this file to:  /etc/systemd/system<br /><br />[Unit]<br />Description=Smart Modem Getty(mgetty)<br />Documentation=man:mgetty(8)<br />Requires=systemd-udev-settle.service<br />After=systemd-udev-settle.service<br /><br />[Service]<br />Type=simple<br />ExecStart=/sbin/mgetty -D -s 38400 /dev/ttyUSB0<br />Restart=always<br />PIDFile=/var/run/mgetty.pid.ttyUSB0<br /><br />[Install]<br />WantedBy=multi-user.target<br /><br /><br /># Don't forget to run the following commands:<br />#<br /># <br /># $ sudo systemctl start mgetty.service<br /># $ sudo systemctl enable mgetty.service<br />#<br /># <br /># ... after copying the file to /etc/systemd/system/<br />#<br />

**Unknown said on 2018-12-13**

Thank you very much.<br /><br />I am the author of mgetty, but I stopped using Linux a while ago due to systemd plague - and today I needed to install mgetty on a customer's Ubuntu system.  Which brought a nice precompiled package, but NO UNIT FILE...<br /><br />Now I have one, thanks to your blog :-)

