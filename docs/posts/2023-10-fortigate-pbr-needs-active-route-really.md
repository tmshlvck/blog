---
date: 2023-10-04T21:21:00.003+02:00
description: ''
published: true
slug: 2023-10-fortigate-pbr-needs-active-route-really
tags:
- firewall
- Networking
- FortiGate
- legacy-blogger
readtime: 5
title: FortiGate PBR needs active route - really?
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2023/10/fortigate-pbr-needs-active-route-really.html)*.

<p>Imagine you have a classic IPsec tunnel between HQ and branch with a slight twist: Say you want to distribute some portion of the address space that is assigned to HQ over the tunnel to the branch and use the IPs in branch DMZ.</p><p>The picture shows the routes - HQ has 5.6.7.8 address on wan1 and there are two static routes from the ISP:</p><p></p><ul style="text-align: left;"><li>5.6.9.0/24 via 5.6.7.8 (use in HQ LAN)</li><li>5.6.10.0/24 via 5.6.7.8 (use in branch DMZ)</li></ul><p></p><div class="separator" style="clear: both; text-align: center;"><a href="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgazzgIT0l48RHPr0gjigS05ijoIUvtrNkVb1XPzLd8do_F1WYW-xsAfPT6n3PBjEQ2ywiRoy3nR_YPujgekMXmZBSKtlLNEUTosGhA2sH_tzZzTUBQQUFKFwobeUo__mH6-UIvzqFtM-uTANjPVAnLnE_K-lwT4SwChcpR1hPKws6LGy6WbOFpyGEvZg/s582/fortigate-pbr.drawio.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgazzgIT0l48RHPr0gjigS05ijoIUvtrNkVb1XPzLd8do_F1WYW-xsAfPT6n3PBjEQ2ywiRoy3nR_YPujgekMXmZBSKtlLNEUTosGhA2sH_tzZzTUBQQUFKFwobeUo__mH6-UIvzqFtM-uTANjPVAnLnE_K-lwT4SwChcpR1hPKws6LGy6WbOFpyGEvZg/s16000/fortigate-pbr.drawio.png" /></a></div><br /><p>So we set up a VPN tunnel, set static routes on HQ side, set IPs and then we need PBR to make sure that despite the default route going over wan1 on branch side to the ISP, the traffic from 5.6.10.0/24 to anywhere is routed over the tunnel back to HQ. Why? Well, the branch ISP applies RPF. Everybody is supposed to apply RPF, because it is the best practice... or something (*). [<a href="https://www.rfc-editor.org/info/bcp38">https://www.rfc-editor.org/info/bcp38</a>]</p><p>Anyway, the problem is that PBR on its own is apparently not enough. If you just set it up, it does not work. Since it has not been immediately apparent why to be I tried to google questions like "FortiGate PBR not working" and I eventually found one "solution" but no real answer what is going on here. To me specific, last comment here [<a href="https://community.fortinet.com/t5/Support-Forum/Policy-Based-Routing-PBR-not-being-applied/td-p/35673?m=166935">https://community.fortinet.com/t5/Support-Forum/Policy-Based-Routing-PBR-not-being-applied/td-p/35673?m=166935</a>] gives instructions that seems to work:</p><p>Add a new default route to routing table. The route needs to have same Administrative Distance as the active default route (towards ISP), next-hop is not needed, but the interface is vpn0 and the Priority has to be lower (= higher number) than the real active default route towards the ISP.&nbsp;</p><p>OK, it works, but why? The official manual [<a href="https://docs.fortinet.com/document/fortigate/6.4.2/administration-guide/144044/policy-routes">https://docs.fortinet.com/document/fortigate/6.4.2/administration-guide/144044/policy-routes</a>] nor this [<a href="https://community.fortinet.com/t5/FortiGate/Technical-Tip-Configure-policy-routes-for-route-based-interface/ta-p/193376">https://community.fortinet.com/t5/FortiGate/Technical-Tip-Configure-policy-routes-for-route-based-interface/ta-p/193376</a>] is not really helpful and I found few other speculations on Reddit, Stack Overflow and random blogs, but nothing on the sport. So, the answer is...&nbsp;</p><p>RPF.</p><p>There is reverse path filtering on the tunnel interface and FortiGates use "feasible path" RPF rule by default, which means that with an active default route through the tunnel is enough to provide feasible path, even though the route is de-prioritized (to avoid causing random load-balancing that would very likely lead to random asymmetric routing and therefore random packetloss in this setting). Btw. the AD has to be same as the real static route, otherwise it will not be considered as a feasible path.</p><p>And another possibility is disabling the RPF with "src-check disable", like this:</p><pre><code>branch # config system interface 

branch (interface) # edit vpn0

branch (vpn0) # show
config system interface
    edit "vpn0"
        set vdom "root"
        set allowaccess ping
        set type tunnel
        set src-check disable
        set snmp-index 19
        config ipv6
            set ip6-allowaccess ping
        end
        set interface "wan"
    next
end</code></pre>

<p>
(*) On more serious note - mocking BCP 38 is not fair. It is undoubtedly the best community-based and therefore Internet-native solution to great portion of the known cyber-attacks.</p>