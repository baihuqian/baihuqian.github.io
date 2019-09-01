---
layout: "post"
title: "Secure Home Network - Using AirPlay Across VLANs"
date: "2019-08-31 21:29"
tags:
 - Networking
---

To allow Apple devices in the main VLAN to use AirPlay on TV and speakers in the IoT VLAN, make sure the following:
1. your TV and speakers can connect to the Apple devices in TCP and UDP on a random port in the 49152-65535 range;
2. enable mDNS reflection in the router (see instructions in [this post]({{ site.baseurl }}{% post_url HomeNetworking/2019-08-27-secure-home-network-using-homekit-devices-across-vlans %})).
3. allow established/related back from the IoT VLAN to the main VLAN.

1 and 3 can be set up using firewall rules. Make sure
1. Accept rules must come before the drop rule for RFC1918 traffic.
2. Use TV and speakers' MAC address as source to skip setting up static IPs for them.

The firewall rules look like this:
![](/assets/posts/HomeNetworking/airplay.png)
