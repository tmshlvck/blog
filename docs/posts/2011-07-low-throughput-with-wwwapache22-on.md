---
date: 2010-08-19T23:25:00.000+02:00
description: ''
published: true
slug: 2011-07-low-throughput-with-wwwapache22-on
tags:
- UNIX
- Apache
- FreeBSD
- legacy-blogger
readtime: 5
title: Low throughput with www/apache22 on FreeBSD 8.1
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/07/low-throughput-with-wwwapache22-on.html)*.

Few days ago I installed FreeBSD 8.1 and compiled Apache 2.2.15 from port www/apache22. I tried to transfer some files over the internet with rather good connection (100Mbps in the weakest point, but several hops resulting in RTT ~27ms). But I got only 10 – 15Mbps throughput from FreeBSD on hi-end HW. I also tried to install FTP server (ftp/proftpd) but I got the same result. It was not a network issue or a client problem, because I had full speed transfers from another machine in the same network. **What the \*\*\*\*?**

Step one… Sysctl parameters. The **most \*\*\*\*ing evil parameter is net.inet.tcp.inflight.enable**. Disable it! Actually I tuned more parameters relevant to TCP/IP stack and kernel internals:

/etc/sysctl.conf

```
kern.ipc.maxsockbuf=16777216
kern.ipc.nmbclusters=32768
kern.ipc.somaxconn=32768
kern.maxfiles=65536
kern.maxfilesperproc=32768
kern.maxvnodes=800000
net.inet.tcp.inflight.enable=0
net.inet.udp.maxdgram=57344
net.inet.udp.recvspace=65536
net.local.stream.recvspace=65536
net.inet.tcp.sendbuf_max=16777216
```

OK, transfers over FTP were then running at 100Mbps. Fine. **But HTTP file transfers were still \*\*\*\*ing slow.** Locally it was two times slower than FTP and much more slower over the internet (HTTP: 4-10 Mbps vs. FTP: 100Mbps).

Step two... Turning off ***sendfile(2)*** in Apache HTTPd.

This is also incredibly \*\*\*\*ing option and there is no documentation pointing at it... Even Google does not know what to do when the Apache \*\*\*\*s up on FreeBSD 8.1. Well, I added one single line to /usr/local/etc/apache22/httpd.conf:

```
EnableSendfile off
```

And that's it. Both FTP and HTTP are running at full speed.