---
date: 2011-07-06T01:35:00.000+02:00
description: ''
published: true
slug: 2011-07-telling-openindiana-firewall-to-accept
tags:
- filrewall
- OpenIndiana
- legacy-blogger
time_to_read: 5
title: Telling OpenIndiana firewall to accept my rules
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/07/telling-openindiana-firewall-to-accept.html)*.

Let's say that I have my own rules:-) For firewall of course. And I want them to be set to kernel instead of automatically generated rules by services, FEA, or whatever.<br />
<br />
Then I have to prepare rules into files:&nbsp;<b>/etc/ipf/ipf.conf</b> (rules for IPv4) and&nbsp;<b>/etc/ipf/ipf6.conf</b> (for IPv6).<br />
<br />
For example it can be something like this:<br />
<br />
/etc/ipf/ipf.conf<br />
<pre>#################### top section #####################
block in all
pass in quick on lo0 all
#################### end of top section #####################

# special rules here

########################## default policy ################################

pass out all keep state
pass out proto icmp all

pass in proto tcp from any to any port = ssh keep state

# Munin &amp; Nagios
pass in proto tcp from any to any port = 4949 keep state
pass in proto tcp from any to any port = 5666 keep state

pass in proto icmp all

# Traceroute
pass in proto udp from any to any port 33433 &gt;&lt; 33626 keep state
</pre><br />
/etc/ipf/ipf6.conf<br />
<pre>#################### top section #####################
block in log all
pass in quick on lo0 all
pass out all keep state
pass out proto ipv6-icmp all
#################### end of top section #####################

pass in proto tcp from any to any port = ssh keep state
pass in proto ipv6-icmp all
pass in proto udp from any to any port 33433 &gt;&lt; 33626 keep state
</pre><br />
Then you have to change service parametes to accept the files:<br />
<pre>svccfg -s network/ipfilter:default setprop firewall_config_default/policy = astring: custom
svccfg -s network/ipfilter:default setprop firewall_config_default/custom_policy_file = astring: "/etc/ipf/ipf.conf" 
svcadm refresh network/ipfilter
</pre>or<br />
<pre>svcadm enable network/ipfilter
</pre><br />
And finaly verify that rules are present:<br />
<pre>ipfstat -nio
ipfstat -nio6
</pre>