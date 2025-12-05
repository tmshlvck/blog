---
date: 2014-05-08T01:51:00.001+02:00
description: ''
published: true
slug: 2014-05-smokeping-slavery-madness
tags:
- Linux
- Networking
- legacy-blogger
readtime: 5
title: Smokeping s\*\*\*\*\*y madness
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2014/05/smokeping-slavery-madness.html)*.

So once again I have been hit by an insane portion of **weird, flawed and inherently unreliable design** in **Smokeping**.

What the \*\*\*\* is **Smokeping**? Well, it is an utility for measuring and graphing latency in network. It should be pretty straight-forward to use. You configure IP addresses and names of boxes to monitor and that't it? Well, not so fast... It is a bit **poxy** because it is Perl after all. You need some modules, you need to have multiple configuration files, you have to configure probes, bind them to targets, modify arguments for probes in target configs etc. There is quite a lot of how-to's for this and in fact it works almost out of box on most distributions.

But... The Smokeping **beast** has also a slave mode. Which means: You have one master that holds configurations and it performs measurements and it generate graphs, everything is the same as in standalone mode. But it also commands slaves to perform the same measurements and report results so the master saves the results to .rrd files and it create graphs.

This communication with slaves is performed by CGI in the Smokeping webroot. And here comes the problem: Privileges.

When Smokeping runs as a daemon it is executed under smokeping user, at least on Debian. So all files that it stores (on Debian usually these files end up in `/var/lib/smokeping`) have `smokeping:smokeping` user and group and privileges `644`. That works for daemon to store data and for CGIs to read them.

But if you need CGIs to store data from slaves, it is not enough. The Smokeping (daemon perhaps?) creates proper .rrd files for the remote measurements from slaves, but these files are updated, so no data in are being displayed in graphs.

Solution:

```
chmod -R g+w /var/lib/smokeping
```
