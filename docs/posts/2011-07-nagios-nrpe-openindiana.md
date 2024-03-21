---
date: 2011-07-12T12:35:00.001+02:00
description: ''
published: true
slug: 2011-07-nagios-nrpe-openindiana
tags:
- Nagios
- NRPE
- OpenIndiana
- legacy-blogger
time_to_read: 5
title: Nagios NRPE @OpenIndiana
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/07/nagios-nrpe-openindiana.html)*.

I tried to install $title and I have found <b>excellent howto</b> for (Open?)Solaris 10 here:&nbsp;<a href="http://www.utahsysadmin.com/2008/03/14/configuring-nagios-plugins-nrpe-on-solaris-10/">http://www.utahsysadmin.com/2008/03/14/configuring-nagios-plugins-nrpe-on-solaris-10/</a><br />
<br />
There is as catch which demonstrates itself by <b>a ****ing compilation error</b>:<br />
<pre>root@spagetka:/usr/src/nrpe-2.12# make
cd ./src/; make ; cd ..
make[1]: Entering directory `/usr/share/src/nrpe-2.12/src'
cc -g -I/usr/include/openssl -I/usr/include -DHAVE_CONFIG_H -o nrpe nrpe.c utils.c -L/usr/lib  -lssl -lcrypto -lnsl -lsocket   
nrpe.c:
"nrpe.c", line 616: invalid source character: &lt;0xffffffe2&gt;
"nrpe.c", line 616: invalid source character: &lt;0xffffff80&gt;
"nrpe.c", line 616: invalid source character: &lt;0xffffff9d&gt;
"nrpe.c", line 616: invalid source character: &lt;0xffffffe2&gt;
"nrpe.c", line 616: invalid source character: &lt;0xffffff80&gt;
"nrpe.c", line 616: invalid source character: &lt;0xffffff9d&gt;
"nrpe.c", line 616: undefined symbol: authpriv
"nrpe.c", line 616: warning: improper pointer/integer combination: arg #2
"nrpe.c", line 618: invalid source character: &lt;0xffffffe2&gt;
"nrpe.c", line 618: invalid source character: &lt;0xffffff80&gt;
"nrpe.c", line 618: invalid source character: &lt;0xffffff9d&gt;
"nrpe.c", line 618: invalid source character: &lt;0xffffffe2&gt;
"nrpe.c", line 618: invalid source character: &lt;0xffffff80&gt;
"nrpe.c", line 618: invalid source character: &lt;0xffffff9d&gt;
"nrpe.c", line 618: undefined symbol: ftp
"nrpe.c", line 618: warning: improper pointer/integer combination: arg #2
"nrpe.c", line 1505: warning: initializer will be sign-extended: -1
"nrpe.c", line 1506: warning: initializer will be sign-extended: -1
"nrpe.c", line 1652: warning: initializer will be sign-extended: -1
"nrpe.c", line 1653: warning: initializer will be sign-extended: -1
cc: acomp failed for nrpe.c
utils.c:
make[1]: *** [nrpe] Error 1
make[1]: Leaving directory `/usr/share/src/nrpe-2.12/src'

*** Compile finished ***
</pre><br />
So you have to <b>edit the nrpe.c</b> according to linked howto.