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
time_to_read: 5
title: SSH is going ape due to ed25519 host keys
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2022/03/ssh-is-going-ape-due-to-ed25519-host.html)*.

<p>I started seeing the complaints from SSH clients about the unknown host keys from various servers that are perfectly stable and secure few months ago. I didn't have time to fully analyze it but I suspected that it is partially caused by me running cutting-edge Debain Unstable and/or Ubuntu 22.04 (before the release).</p><p>The low-level cause is that "something" has changed (and as I said, I am not exactly sure what is going on here) that caused that the servers or clients prefer new&nbsp;<b><i>ssh-ed25519</i></b>&nbsp;instead of the old&nbsp;<i style="font-weight: bold;">ecdsa-sha2-nistp256.</i>&nbsp;Which is a good thing, only that the client keeps saying something like this:</p>
<pre>@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@&nbsp; &nbsp; WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!&nbsp; &nbsp; &nbsp;@
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
.
Please contact your system administrator.
Add correct host key in /.ssh/known_hosts to get rid of this message.
Offending RSA key in /.ssh/known_hosts:
&nbsp; remove with:
&nbsp; ssh-keygen -f "/.ssh/known_hosts" -R ""
RSA host key for  has changed and you have requested strict checking.
Host key verification failed.
</pre>
<p>
Which is a serious warning that should not be ignored in any case, unless one positively knows that the server had good reason for changing the key.<br /><br />If you read it and started worrying, that's good. Bad thing is that after an half an hour googling and browsing <i>openssh</i> changelogs I haven't got to any a detailed explanation and/or diffs that causes that. I have two suspects:</p><p></p><ul style="text-align: left;"><li>server side change (see&nbsp;<a href="http://metadata.ftp-master.debian.org/changelogs/main/o/openssh/unstable_changelog">http://metadata.ftp-master.debian.org/changelogs/main/o/openssh/unstable_changelog</a>&nbsp;)</li></ul>
<pre style="text-align: left;">New upstream release
(https://www.openssh.com/releasenotes.html#8.5p1):
    - ssh(1), sshd(8): change the first-preference signature algorithm from
      ECDSA to ED25519.
</pre><ul style="text-align: left;"><li>client side change <a href="https://launchpad.net/ubuntu/+source/openssh/1:8.2p1-4ubuntu0.4">https://launchpad.net/ubuntu/+source/openssh/1:8.2p1-4ubuntu0.4</a> , that fixes this bug <a href="https://bugs.launchpad.net/ubuntu/+source/openssh/+bug/1952421">https://bugs.launchpad.net/ubuntu/+source/openssh/+bug/1952421</a></li></ul><div>Or maybe I am wrong. Please provide more details in discussions if you know what changes causes this change in behavior. In any case it is wrong because flashing tons of these unknown key warnings is going to make everyone used to the fact that this happening on its own. Ultimately people are going to be less vigilant and prone to MitM attacks in the end. :-(</div>