---
date: 2012-12-30T13:51:00.002+01:00
description: ''
published: true
slug: 2012-12-openwrt-insanity-shame-and-ignorancy
tags:
- Linux
- HW
- Networking
- legacy-blogger
readtime: 5
title: OpenWRT insanity, shame and ignorancy
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2012/12/openwrt-insanity-shame-and-ignorancy.html)*.

I have been hit by a idiotic, unreasonable, insane and stupid setting in OpenWRT:<br />
<br />
<b>/etc/sysctl.conf:</b><br />
<pre>net.ipv4.netfilter.ip_conntrack_tcp_timeout_established=3600
</pre>
<br />
Well what does it mean? Simple thing: Every long-lasting connections breaks after one hour. No matter whether I have TCP keepalive on, because TCP keepalive timeout defaults to 7200 seconds.<br />
<br />
In practice you will see this:<br />
<pre>brill@tapir ~ $ ssh milhouse.backbone.ignum.cz
Linux milhouse 2.6.32-5-amd64 #1 SMP Wed May 18 23:13:22 UTC 2011 x86_64

Keep keep your hands off this server!
Nikdo tu na nic nesahejte!

Last login: Sun Dec 30 00:46:47 2012 from 89.177.24.237</pre>
<pre>&lt;no activity for more than 1 hour&gt;
brill@milhouse:~$&nbsp;Write failed: Broken pipe

brill@tapir ~ $
</pre>
<br />
There was a flame on this topic on OpenWRT forum:<br />
<br />
<a href="https://dev.openwrt.org/ticket/5777">https://dev.openwrt.org/ticket/5777</a><br />
<br />
And the result is: invalid, wontfix, ****yourself.<br />
<br />
Mother****ers. They break everything. God... You can sacrifice filesystem, give up serial port, GPIO, LED diodes, whatever, but keep firewall working normally when you are building network appliance you idiots!