---
date: 2013-05-30T13:39:00.000+02:00
description: ''
published: true
slug: 2013-05-usb-device-id-0bdb1926-ericsson
tags:
- Linux kernel
- Linux Mint
- legacy-blogger
readtime: 5
title: USB device ID 0bdb:1926 Ericsson Business Mobile Networks BV stopped working
  after update Linux Mint 14 to 15
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2013/05/usb-device-id-0bdb1926-ericsson.html)*.

I tried to upgrade Linux Mint on my laptop from Linux Mint 14 (Nadia) to 15 (Olivia). And suddenly GSM/3G modem that I am using to connect to one Czech mobile ISP stopped working.<br />
<br />
The problem was that the connecting to the network timed out each time. And there was no apparent reason in <i>/var/log/syslog</i> apart from that DHCP did not received a reply from the ISP:<br />
<br />
<pre>May 26 02:31:58 tapir modem-manager[13636]:   (ttyACM2) opening serial port...
May 26 02:31:58 tapir modem-manager[13636]:   (ttyACM1): using PDU mode for SMS
May 26 02:31:58 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (enabling -&gt; enabled)
May 26 02:31:58 tapir NetworkManager[13638]:  WWAN now enabled by management service
May 26 02:31:59 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (enabled -&gt; searching)
May 26 02:32:02 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (searching -&gt; registered)
May 26 02:32:42 tapir NetworkManager[13638]:  Activation (wwan0) starting connection 'T-Mobile Default'
May 26 02:32:42 tapir NetworkManager[13638]:  (wwan0): device state change: disconnected -&gt; prepare (reason 'none') [30 40 0]
May 26 02:32:42 tapir NetworkManager[13638]:  Activation (wwan0) Stage 1 of 5 (Device Prepare) scheduled...
May 26 02:32:42 tapir NetworkManager[13638]:  Activation (wwan0) Stage 1 of 5 (Device Prepare) started...
May 26 02:32:42 tapir NetworkManager[13638]:  Activation (wwan0) Stage 1 of 5 (Device Prepare) complete.
May 26 02:32:42 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (registered -&gt; connecting)
May 26 02:32:44 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (connecting -&gt; connected)
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 2 of 5 (Device Configure) scheduled...
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 2 of 5 (Device Configure) starting...
May 26 02:32:44 tapir NetworkManager[13638]:  (wwan0): device state change: prepare -&gt; config (reason 'none') [40 50 0]
May 26 02:32:44 tapir NetworkManager[13638]:  (wwan0): bringing up device.
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 2 of 5 (Device Configure) successful.
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 3 of 5 (IP Configure Start) scheduled.
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 2 of 5 (Device Configure) complete.
May 26 02:32:44 tapir avahi-daemon[657]: Joining mDNS multicast group on interface wwan0.IPv6 with address fe80::dcc9:44ff:fe89:56fc.
May 26 02:32:44 tapir avahi-daemon[657]: New relevant interface wwan0.IPv6 for mDNS.
May 26 02:32:44 tapir avahi-daemon[657]: Registering new address record for fe80::dcc9:44ff:fe89:56fc on wwan0.*.
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 3 of 5 (IP Configure Start) started...
May 26 02:32:44 tapir NetworkManager[13638]:  (wwan0): device state change: config -&gt; ip-config (reason 'none') [50 70 0]
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Beginning DHCPv4 transaction (timeout in 45 seconds)
May 26 02:32:44 tapir NetworkManager[13638]:  dhclient started with pid 13689
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 4 of 5 (IPv6 Configure Timeout) scheduled...
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 3 of 5 (IP Configure Start) complete.
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 4 of 5 (IPv6 Configure Timeout) started...
May 26 02:32:44 tapir NetworkManager[13638]:  Activation (wwan0) Stage 4 of 5 (IPv6 Configure Timeout) complete.
May 26 02:32:44 tapir dhclient: Internet Systems Consortium DHCP Client 4.2.4
May 26 02:32:44 tapir dhclient: Copyright 2004-2012 Internet Systems Consortium.
May 26 02:32:44 tapir dhclient: All rights reserved.
May 26 02:32:44 tapir dhclient: For info, please visit https://www.isc.org/software/dhcp/
May 26 02:32:44 tapir dhclient: 
May 26 02:32:44 tapir NetworkManager[13638]:  (wwan0): DHCPv4 state changed nbi -&gt; preinit
May 26 02:32:44 tapir dhclient: Listening on LPF/wwan0/de:c9:44:89:56:fc
May 26 02:32:44 tapir dhclient: Sending on   LPF/wwan0/de:c9:44:89:56:fc
May 26 02:32:44 tapir dhclient: Sending on   Socket/fallback
May 26 02:32:44 tapir dhclient: DHCPDISCOVER on wwan0 to 255.255.255.255 port 67 interval 3 (xid=0x40900a61)
May 26 02:32:47 tapir dhclient: DHCPDISCOVER on wwan0 to 255.255.255.255 port 67 interval 8 (xid=0x40900a61)
May 26 02:32:55 tapir dhclient: DHCPDISCOVER on wwan0 to 255.255.255.255 port 67 interval 13 (xid=0x40900a61)
ay 26 02:33:08 tapir dhclient: DHCPDISCOVER on wwan0 to 255.255.255.255 port 67 interval 7 (xid=0x40900a61)
May 26 02:33:15 tapir dhclient: DHCPDISCOVER on wwan0 to 255.255.255.255 port 67 interval 13 (xid=0x40900a61)
May 26 02:33:28 tapir dhclient: DHCPDISCOVER on wwan0 to 255.255.255.255 port 67 interval 20 (xid=0x40900a61)
May 26 02:33:29 tapir NetworkManager[13638]:  (wwan0): DHCPv4 request timed out.
May 26 02:33:29 tapir NetworkManager[13638]:  (wwan0): canceled DHCP transaction, DHCP client pid 13689
May 26 02:33:29 tapir NetworkManager[13638]:  Activation (wwan0) Stage 4 of 5 (IPv4 Configure Timeout) scheduled...
May 26 02:33:29 tapir NetworkManager[13638]:  Activation (wwan0) Stage 4 of 5 (IPv4 Configure Timeout) started...
May 26 02:33:29 tapir NetworkManager[13638]:  (wwan0): device state change: ip-config -&gt; failed (reason 'ip-config-unavailable') [70 120 5]
May 26 02:33:29 tapir NetworkManager[13638]:  Activation (wwan0) failed for connection 'T-Mobile Default'
May 26 02:33:29 tapir NetworkManager[13638]:  Activation (wwan0) Stage 4 of 5 (IPv4 Configure Timeout) complete.
May 26 02:33:29 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (connected -&gt; disconnecting)
May 26 02:33:29 tapir NetworkManager[13638]:  (wwan0): device state change: failed -&gt; disconnected (reason 'none') [120 30 0]
May 26 02:33:29 tapir NetworkManager[13638]:  (wwan0): deactivating device (reason 'none') [0]
May 26 02:33:29 tapir avahi-daemon[657]: Interface wwan0.IPv6 no longer relevant for mDNS.
May 26 02:33:29 tapir avahi-daemon[657]: Leaving mDNS multicast group on interface wwan0.IPv6 with address fe80::dcc9:44ff:fe89:56fc.
May 26 02:33:29 tapir avahi-daemon[657]: Withdrawing address record for fe80::dcc9:44ff:fe89:56fc on wwan0.
May 26 02:33:29 tapir modem-manager[13636]:   Modem /org/freedesktop/ModemManager/Modems/0: state changed (disconnecting -&gt; registered)
</pre>
<br />
<b>WTF?!</b> I have asked myself?<br />
<br />
After some time spent with the change-logs, debugging and asking google I have found this:&nbsp;<a href="https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/1172178">https://bugs.launchpad.net/ubuntu/+source/network-manager/+bug/1172178</a><br />
<br />
So it seems there is another pile of code in Linux kernel that aims to standardize some drivers and set common APIs, but it breaks things in meantime. Why it is always me who have to suffer this?:-)<br />
<br />
Advice from&nbsp;Oleg Cherkasov at the end of the ticked worked for me. Thanks!