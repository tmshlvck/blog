---
date: 2022-03-17T23:13:00.006+01:00
description: ''
published: true
slug: 2022-03-ssh-is-going-ape-due-to-ed25519-host
tags:
- Debian
- Linux
- Ubuntu
- legacy-blogger
readtime: 5
title: SSH is going ape due to ed25519 host keys
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2022/03/ssh-is-going-ape-due-to-ed25519-host.html)*.

I started seeing the complaints from SSH clients about the unknown host keys from various servers that are perfectly stable and secure few months ago. I didn't have time to fully analyze it but I suspected that it is partially caused by me running cutting-edge Debain Unstable and/or Ubuntu 22.04 (before the release).

The low-level cause is that "something" has changed (and as I said, I am not exactly sure what is going on here) that caused that the servers or clients prefer new **_ssh-ed25519_** instead of the old **_ecdsa-sha2-nistp256_**. Which is a good thing, only that the client keeps saying something like this:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
.
Please contact your system administrator.
Add correct host key in /.ssh/known_hosts to get rid of this message.
Offending RSA key in /.ssh/known_hosts:
  remove with:
  ssh-keygen -f "/.ssh/known_hosts" -R ""
RSA host key for  has changed and you have requested strict checking.
Host key verification failed.
```

Which is a serious warning that should not be ignored in any case, unless one positively knows that the server had good reason for changing the key.

If you read it and started worrying, that's good. Bad thing is that after an half an hour googling and browsing _openssh_ changelogs I haven't got to any a detailed explanation and/or diffs that causes that. I have two suspects:

- server side change (see [http://metadata.ftp-master.debian.org/changelogs/main/o/openssh/unstable_changelog](http://metadata.ftp-master.debian.org/changelogs/main/o/openssh/unstable_changelog))

```
New upstream release
(https://www.openssh.com/releasenotes.html#8.5p1):
    - ssh(1), sshd(8): change the first-preference signature algorithm from
      ECDSA to ED25519.
```

- client side change [https://launchpad.net/ubuntu/+source/openssh/1:8.2p1-4ubuntu0.4](https://launchpad.net/ubuntu/+source/openssh/1:8.2p1-4ubuntu0.4), that fixes this bug [https://bugs.launchpad.net/ubuntu/+source/openssh/+bug/1952421](https://bugs.launchpad.net/ubuntu/+source/openssh/+bug/1952421)

Or maybe I am wrong. Please provide more details in discussions if you know what changes causes this change in behavior. In any case it is wrong because flashing tons of these unknown key warnings is going to make everyone used to the fact that this happening on its own. Ultimately people are going to be less vigilant and prone to MitM attacks in the end. :-(