---
layout: "post"
title: "Secure Home Network: Disable EdgeOS SSH and GUI Access from WAN Interface"
date: "2021-06-07 20:46"
tags:
 - HomeNetwork
---

Today, I accidently accessed the standard HTTP and HTTPS endpoint at my home's DNS name after [setting up DDNS]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-06-06-secure-home-network-access-your-home-assistant-from-internet.md %}/#ddns-using-edgerouter), and the EdgeOS GUI shows up. In addition, I can SSH to my DNS name and access EdgeOS from terminal. I have [set up firewall rules to block any access from WAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}/#firewall), but how is it possible?

It turns out that EdgeOS listens on port 22, 80, and 443 for SSH, HTTP, and HTTPS on **all interfaces**, including the WAN interface. This is a significant security risk if the EdgeOS username and password are compromised, or the default username and password are used.

To disable WAN access, we should configure EdgeOS to listen on only LAN interface IP addresses for `gui` and `ssh` services. This should be configured for every VLAN that can access EdgeOS. For example, the untagged LAN network is 192.168.1.1/24 and VLAN 10 (192.168.10.1/24) is the "secure" VLAN, then the following setup is required:

```
set service gui listen-address 192.168.1.1
set service gui listen-address 192.168.10.1
set service ssh listen-address 192.168.1.1
set service ssh listen-address 192.168.10.1
```

After committing the change, I can no longer access GUI or SSH via the DNS name or WAN IP address.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
