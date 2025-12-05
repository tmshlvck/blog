---
date: 2019-01-30T09:06:00.001+01:00
description: ''
published: true
slug: 2019-01-java-disabled-certain-tlsssl-algorithms
tags:
- Ubuntu
- desktop
- legacy-blogger
readtime: 5
title: Java disabled certain TLS/SSL algorithms - enable them back
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2019/01/java-disabled-certain-tlsssl-algorithms.html)*.

Now hear this.. You know it yourself: You have a server. Not a really old one, just a bit old. It has IPMI/ILO or whatever they call the Java remote console. And it stopped working (you can run the Java client app, but it fails to connect) with the modern Ubuntu installs as a client. What the ****, you say... Well, the problem is:

```
01/30/2019 08:38:17:859:  Connection failed with exception:
No appropriate protocol (protocol is disabled or cipher
suites are inappropriate)
```

The solution?: Comment out the algorithm disabling line in /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/java.security

```
#jdk.tls.disabledAlgorithms=SSLv3, RC4, DES, MD5withRSA, \
#DH keySize < 1024, EC keySize < 224, 3DES_EDE_CBC
```

---

## 1 comments captured from [original post](https://snarkybrill.blogspot.com/2019/01/java-disabled-certain-tlsssl-algorithms.html) on Blogger

**themylogin said on 2020-06-18**

It helped me, thank you very much!
