---
date: 2011-09-08T11:15:00.000+02:00
description: ''
published: true
slug: 2011-09-build-environment-for-openindiana
tags:
- UNIX
- OpenIndiana
- legacy-blogger
time_to_read: 5
title: Build environment for OpenIndiana
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/09/build-environment-for-openindiana.html)*.

Just a quick post - I wanted to build few things like Nagios NRPE, few Perl modules with C bindings etc. So I needed to install toolchain, compiler (rather Sun Studio than gcc) etc.<br />
<br />
The easy way is:<br />
<br />
<pre>pkg install SUNWcvs SUNWsvn SUNWarc SUNWj6dmo SUNWj6dev SUNWj6dmx SUNWj6dvx \
SUNWj6cfg SUNWj6rtx SUNWj6man SUNWgnu-automake-19 SUNWgnu-automake-110 SUNWaconf \
SUNWmercurial SUNWlibtool ss-dev SUNWsfwhea SUNWhea SUNWxwinc SUNWxorg-headers \
SUNWi2cs SUNWgpch SUNWgnome-common-devel SUNWgmake SUNWbison SUNWflexlex

pkg install SUNWcvs SUNWsvn SUNWarc SUNWj6dmo SUNWj6dev SUNWj6dmx SUNWj6dvx \
SUNWj6cfg SUNWj6rtx SUNWj6man SUNWgnu-automake-19 SUNWgnu-automake-110 SUNWaconf \
SUNWmercurial SUNWlibtool ss-dev SUNWsfwhea SUNWhea SUNWxorg-headers SUNWi2cs \
SUNWgpch SUNWgnome-common-devel SUNWgmake SUNWbison SUNWflexlex

pkg install developer/sunstudio12u1
</pre>