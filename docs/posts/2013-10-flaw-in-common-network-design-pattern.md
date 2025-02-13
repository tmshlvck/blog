---
date: 2013-10-12T08:06:00.003+02:00
description: ''
published: true
slug: 2013-10-flaw-in-common-network-design-pattern
tags:
- Networking
- legacy-blogger
readtime: 5
title: Flaw in common network design pattern (or in understanding how to use it)?
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2013/10/flaw-in-common-network-design-pattern.html)*.

There is an old and not-so-clever network design pattern like this:<br />
<br />
<div class="separator" style="clear: both; text-align: center;">
<a href="http://2.bp.blogspot.com/-E_TM_EDbTkw/UljlhoDw1UI/AAAAAAAAAHs/H3jvFEfNa38/s1600/layered-design-pattern.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="165" src="http://2.bp.blogspot.com/-E_TM_EDbTkw/UljlhoDw1UI/AAAAAAAAAHs/H3jvFEfNa38/s320/layered-design-pattern.png" width="320" /></a></div>
<br />
<br />
I know this from CCN(A|P) but I have no idea who invented it. Cisco perhaps. Idea is to use IGP, nowadays it is naturally OSPF in most cases, for resolution of the L3 path towards a subnet in question. The subnet itself is terminated on a gateway. Two different gateways to be accurate. One acts as a default gateway, which means that it is active router in HSRP, so it holds "virtual" IP address as well as MAC address of the default gateway. It is the address that hosts in the subnet uses in their routing tables and subsequently in their ARP tables.<br />
<br />
So far so good. Everything is up and running, one gateway is active, another one passive, both routers are redistributing connected routes to OSPF and both are able to deliver traffic from L3 networks towards clients in the L2 subnet in question.<br />
<br />
But wait a moment... How do L3 switches know where to send the traffic over L2? Yes, routing table and ARP resolution. By default the <b>ARP table records timeout is 4 hrs</b>, right?<br />
<br />
And how do L2 switches know the port to forward traffic? MAC address table (or switching table, depends on terminology). <b>Timeout for MAC address table? 5 mins.</b><br />
<br />
So what does the passive switch do when no traffic from hosts hits this switch, because it does not have active default GW address and nobody wants to communicate over L2 to it's "real IP". Well, obviously the MAC address table cleans out after a while but ARP records stays much much longer (and ARP records are refreshed when some traffic arrives and the L3 switch does not have proper ARP table records).<br />
<br />
But... When there is no MAC address table record for the particular MAC on the standby L3 switch, <b>the switch just broadcasts the traffic for that MAC to all ports</b>. And it multiplies in the network further because in case you have prevailing vertical flows in your network it is likely that any other L2 switch in the chain does not know the MAC address either. Except the one which has the destination host connected. So it means you might have huge multiplication of useless traffic if your L3 switch attracts some traffic in OSPF for any subnet which it is standby for it's gateway.<br />
<br />
It might several megs or even tens or hundred megs of constant scattered traffic in real life scenario in 1 Gig network.<br />
<br />
And what to do to avoid this? Think of the design pattern... <b>Set higher OSPF costs for redistributed routes on the standby router.</b> Simple but effective, huh?