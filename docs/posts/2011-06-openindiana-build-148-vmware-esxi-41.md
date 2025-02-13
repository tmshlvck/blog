---
date: 2011-06-25T11:55:00.001+02:00
description: ''
published: true
slug: 2011-06-openindiana-build-148-vmware-esxi-41
tags:
- OpenIndiana
- legacy-blogger
readtime: 5
title: OpenIndiana Build 148 @ VMWare ESXi 4.1
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/06/openindiana-build-148-vmware-esxi-41.html)*.

I have been trying to run an OpenIndiana server in a virtualized envrionment (sort of a development and test machine). And I ran into problems, actually into <b>one huge ****ing problem</b> at the very beginning: The OI installer CD image does not boot inside VMWare virtual server when you have more than 4 virtual CPUs set in the virtual machine profile.<br />
<br />
Easy solution: Set only 2 CPUs and everything works fine. Well, not everything... The Open Indiana <b>****ing installer</b> freezes in the end instead of restarting the system, but it worked for me after all.<br />
<br />
My next article: Why do we virtualize? What the **** virtualization brings to our lives and what it takes? And why do we suffer so miserably with commodity servers...?:-)