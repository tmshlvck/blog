---
date: 2012-04-12T00:04:00.002+02:00
description: ''
published: true
slug: 2012-04-desktop-hell
tags:
- Debian
- Linux
- Xfce
- desktop
- legacy-blogger
time_to_read: 5
title: Desktop hell
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2012/04/desktop-hell.html)*.

Well after some time I am back with my reckless criticism of insane (but OpenSource) desktops. Basically what can you choose when you want a Linux desktop? Well, of course plenty of things but when you need getting things done and you do not want to play with own <i>xrandr, xkb, xinput&nbsp;</i>scripting or with printing, keybindings for multimedia keys etc. you would probably need some desktop environment. And there are quite a lot of such projects (look at Wikipedia). KDE. Gnome3. Xfce, LXDE, Unity to name some.<br />
<br />
Well what you can find is that there is a huge hare-core of Gnome3 and Unity. People just hate them because these desktops are everything but useful and intuitive. The both are resembling some of iPad madness and with Unity there are rumors on the web that it is actually designed for tablet computers and its usage on desktop is some sort of side-effect of development in the meantime when Canonical (author of Unity &amp; Ubuntu) is negotiating deals with tablet manufacturers. Pity. In fact these desktops forces you to do things by their ways (different from what we all are used to from previous generations of desktops starting from Win 3.11 up to Gnome 2.32) and they are putting obstacles between you and your productivity apps. And I concur with all these objections against particularly these two desktops.<br />
<br />
There is of course KDE. KDE is way long from what is my idea of a decent desktop. I like some aspects of it but it lacks what I need - really fast desktop (with or rather without eye-candy but really fast!), easy-to used virtual desktops, app panel and good application switcher and support for docking/undocking (which means changing screen resolution, switching on/off the LVDS etc. It takes me to the remaining Xfce.<br />
<br />
But current stable Xfce (on Debian Wheezy) to be specific is fare from being easy to use. I had to tweak it deeply to become usable. I had 4 most severe problems I had to cope with. <i>NetworkManager</i> (+ <i>NM Applet</i>) was unable to connect to set network. (Solved). There are nasty sounds in different applications set. The most annoying are terminal bell in <i>gnome-terminal</i> and <i>gdm3</i> greeter sound (turned off easily). Two problems are more complicated and I am still working on them. First is Xfce&nbsp;<i>xkb</i> applet which looses it's configuration (I mean set layouts, layout-switcher keyboard shortcut etc.) each and every time you connect or disconnect USB keyboard. Which effectively means it looses the configuration each time you dock or undock your laptop. And the last problem is automatic switching of desktop resolution and putting LVDS to on/off state according to presence of another screen connected via Display Port. I have done some scripting workarounds to switch this semi-automatically but I am not proud of it at all.<br />
<br />
Next time I am going to put here mentioned Debian tweaks for the two already-solved problems on my Debian Wheezy + Xfce desktop.