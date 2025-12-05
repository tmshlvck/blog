---
date: 2011-08-27T22:38:00.006+02:00
description: ''
published: true
slug: 2011-08-test-driven
tags:
- UNIX
- Debian
- test-driven operation
- monitoring
- legacy-blogger
readtime: 5
title: Test driven service operation vs. Nagios et al.
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/08/test-driven.html)*.

I am constantly thinking about network and server outages handling. This was my focus few years ago, I worked as an admin/op in a small hosting company and I was bombarded by SMSes from our home-brewed server/service monitoring system written in Perl. The system has bunch of drawbacks so we decided to replace it by Nagios (3 dot something). It has drawbacks as well, and I would say that even more serious in some cases.

I was responsible for the Nagios migration but I am not an op anymore so I have less experience with that. From my point of view, being aware of my limited insight, I can describe some drawbacks in Nagios 3 (= Nagios Core, simply the OSS version you get when apt-get install Nagios3 on top of Debian...).

The first one and probably the most severe: The **configuration is complicated** by nature. In addition Debian forces/strongly suggest some ideas about how you should write the config files. And when you try to do so, you have to read manual which does not give answers to all questions. For example: Are multiple parents in dependency tree of hosts/services in AND, OR or whatever relation? You can find lots of small questions and eventually Google some answers, dig into documentation or whatever, but it takes time. Anyway, writing Nagios configs takes time and one should ask himself: Why? Of course you can use Swiss-made NConf to convert writing config into clicking configs in web interface, but it is not a real improvement. **Why can't it be automatic?** Let's say the system can **auto-discover hosts** and **test-run all yet-know service tests**. If some of tests results to OK, it can suggest that test. It should be able to clone hosts, categorize hosts, make exceptions etc. but on the other hand **I prefer text config files** over some sophisticated database schema...

The another thing is that sending alarms should be smarter than only triggering scripts like send mail to contacts and send messages to pagers or cell phones in modern days. Well, I like the idea of **master alarm**. I would like to have a possibility to set some alarms as not crucial for business/system operation and have them listed in web interface/reports and alarmed by less aggressive way to ops. I would like to have a possibility to have some threshold for sounding master alarm and then sending this master alarm (once or N-times but not overflowing ops with hundreds of different and probably correlated errors). And I would like to have a permissive and easy to use system, not a system which does not allow to acknowledge all reported errors and when not acknowledged, it bothers by SMSes over and over. I would like the system to accept my input and respect what I want or want not to save, not like Nagios->Acknowledge->Error: You have to write a comment. **Wtf.?** I have major network problem, I want to investigate what is going on and not writing stupid comments, especially in situation I do not know what to say, I just want to stop SMSes from bothering me.

I would like to have a **monitoring cluster**, able to monitor network/servers/services from more locations and give me a overall report. I would like to have a possibility to write own triggers on errors/warnings, to report more complex situations. Let's say that I have a cluster of 10 servers with loadbalancing and I know that 5 servers would be sufficient. I would make sense not to send alarm during nighttime when one of these 10 servers went down. But it make sense to send alarm if only 6 or less servers remains operational. Event more complex situations could be described and it would be nice to set this triggers easily.

And I would like to have a **overview on my system**. I want to see what is going on, what happened in past and write afterwards how did I solved the problem to have op's log and tip for next time.

I think that technically it should be relatively easy to run few thousands of test each minute on a decent Intel server. Not speaking about parallelization. Then it comes an idea: We have a paradigm/style/philosophy of **test driven development**. Why not to have a **test driven system operation**? I think there are two "contras": ComplexNess of configuration and complications with data acquisition and interpretation - i.e. people fears that it would be more complicated to answer a question "what is broken?". But I believe that both "con's" a only drawbacks of current software. Discussion will be appreciated.