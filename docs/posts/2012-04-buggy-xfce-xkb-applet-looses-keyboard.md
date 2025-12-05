---
date: 2012-04-12T14:35:00.000+02:00
description: ''
published: true
slug: 2012-04-buggy-xfce-xkb-applet-looses-keyboard
tags:
- Debian
- Linux
- desktop
- legacy-blogger
readtime: 5
title: Buggy Xfce xkb applet looses keyboard layout config
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2012/04/buggy-xfce-xkb-applet-looses-keyboard.html)*.

I have been hit by a nasty Xfce xkb applet bug, which is widely known in several bloody bugzillas (of RH/CentOS, Ubuntu, Debian,...) but it seems unresolved yet. The backround of the bug is, that the applet looses it's configuration, especially configured keyboard layouts and shortcut for switching them while for while which may seem to be random or it may seem to have some coincidence with suspending/wakeups of the computer. Well it is not random, it resolves to connections and disconnections of external (or even internal) USB keyboard, which may of course be triggered by suspend/wakeup.

So it seems I have the reason isolated. Now what is the solution? Well the applet is part of so-called Xfce-goodies, which is sort of external project. I did not find corresponding bugzilla nor some mailinglist etc. But I think that author is perhaps aware of this behavior, so there is no need to shout at him. But I needed the workaround and I think I have found one. It is simple and it is Linux-like: Just configure all your keyboards in _xorg.xonf_ file in **InputClass** section and then use the applet as an indicator and switcher.

I am using following snippet of _xorg.conf_ on my ThinkPad X301 which I am connecting and disconnecting to a USB keyboard:

```
Section "InputClass"
    Identifier "keyboard defaults"
    MatchIsKeyboard "on"
    Option "XkbLayout" "us, cz"
    Option "XkbVariant" ", qwerty_bksl"
    Option "XKbOptions" "grp:alt_shift_toggle, terminate:ctrl_alt_bksp"
EndSection
```
