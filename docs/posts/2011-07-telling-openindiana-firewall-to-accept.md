---
date: 2011-07-06T01:35:00.000+02:00
description: ''
published: true
slug: 2011-07-telling-openindiana-firewall-to-accept
tags:
- filrewall
- OpenIndiana
- legacy-blogger
readtime: 5
title: Telling OpenIndiana firewall to accept my rules
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/07/telling-openindiana-firewall-to-accept.html)*.

Let's say that I have my own rules:-) For firewall of course. And I want them to be set to kernel instead of automatically generated rules by services, FEA, or whatever.

Then I have to prepare rules into files: **/etc/ipf/ipf.conf** (rules for IPv4) and **/etc/ipf/ipf6.conf** (for IPv6).

For example it can be something like this:

**/etc/ipf/ipf.conf**:
```
#################### top section #####################
block in all
pass in quick on lo0 all
#################### end of top section #####################

# special rules here

########################## default policy ################################

pass out all keep state
pass out proto icmp all

pass in proto tcp from any to any port = ssh keep state

# Munin & Nagios
pass in proto tcp from any to any port = 4949 keep state
pass in proto tcp from any to any port = 5666 keep state

pass in proto icmp all

# Traceroute
pass in proto udp from any to any port 33433 >< 33626 keep state
```

**/etc/ipf/ipf6.conf**:
```
#################### top section #####################
block in log all
pass in quick on lo0 all
pass out all keep state
pass out proto ipv6-icmp all
#################### end of top section #####################

pass in proto tcp from any to any port = ssh keep state
pass in proto ipv6-icmp all
pass in proto udp from any to any port 33433 >< 33626 keep state
```

Then you have to change service parametes to accept the files:

```
svccfg -s network/ipfilter:default setprop firewall_config_default/policy = astring: custom
svccfg -s network/ipfilter:default setprop firewall_config_default/custom_policy_file = astring: "/etc/ipf/ipf.conf"
svcadm refresh network/ipfilter
```

or:

```
svcadm enable network/ipfilter
```

And finaly verify that rules are present:

```
ipfstat -nio
ipfstat -nio6
```