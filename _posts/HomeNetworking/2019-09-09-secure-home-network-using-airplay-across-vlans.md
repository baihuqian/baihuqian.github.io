---
layout: "post"
title: "Secure Home Network - Using AirPlay Across VLANs"
date: "2019-09-09 21:29"
tags:
 - HomeNetwork
 - CordCutter
---

In a previous post, I discussed why you should [isolate connected devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}). Smart TVs ideally should be in the Device VLAN, too, instead of your "secure" VLAN. But putting them on a separate VLAN breaks an important TV use case for cord cutters: AirPlay.

AirPlay uses [Multicast DNS (mDNS)](https://en.wikipedia.org/wiki/Multicast_DNS), implemented in Apple Bonjour, to discover compatible devices on a local area network (LAN). As the TV and your phone are in separate VLAN, by default, devices on different VLANs cannot discover each other, thus breaking the AirPlay functionality.

To allow Apple devices in the main VLAN to use AirPlay on TV and speakers in the IoT VLAN, make sure the following:
1. your TV and speakers can connect to the Apple devices:
   1. in TCP and UDP on a random port in the 49152-65535 range;
   2. in TCP with a source port of 7000;
   3. in UDP with a source port of 6002.
2. ensure mDNS can traverse through VLANs. An mDNS message is a multicast UDP packet to/from IPv4 address of 224.0.0.251 and UDP port 5353. This means the combination of:
   1. mDNS reflector or multicast repeater is enabled. I prefer enabling mDNS repeater between the device VLAN and secure VLAN's interfaces, i.e., `switch0.10` and `switch0.20`. Note that mDNS reflector enables mDNS on **ALL** interfaces, including the WAN interface, thus it is bad.
   2. If the default action for `DEVICE_LOCAL` (traffic from the device VLAN to the router) is `drop`, create a rule that allows mDNS traffic.
3. allow established/related back from the Device VLAN to the secure VLAN.

1 and 3 can be set up using firewall rules. Make sure
1. Accept rules must come before the drop rule for RFC1918 traffic.
2. Use port group to manage ports used by AirPlay. For simplicity, I created a single rule to accept TCP and UDP traffic on these ports.
3. Use address group to manage devices (with static IP assigned to them) supporting AirPlay.

```
**Port Group for AirPlay**
Group Name: airplay
Type: port-group
Port: 6002, 7000, 49152-65535

**DEVICE_IN Rule**
Description: allow established related
Action: Accept
Protocol: All protocols
Advanced > State: Established / Related
Destination > Network Group : LAN_NETWORKS

Description: allow airplay
Action: Accept
Protocol: UDP
Source > Address Group : TV_ADDRESS, Port Group: airplay
Destination > Network Group : LAN_NETWORKS

**DEVICE_LOCAL Rule**
Description: allow Bonjour/mDNS
Action: Accept
Protocol: UDP
Destination > Address: 224.0.0.251, Port: 5353
```

To enable mDNS repeater, go to the config tree in EdgeOS, navigate to `service` > `mdns` and click the `+` sign right to `repeater`, and add `switch0.10` and `switch0.20` to the interface list on the right. After doing this, be sure to click the Preview button to save the change.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).

# References
1. [mDNS reflector vs. repeater](https://www.reddit.com/r/Ubiquiti/comments/jerhab/mdns_reflector_vs_repeater/)
2. [IoT VLAN settings specific to AirPlay / Apple TV](https://www.reddit.com/r/Ubiquiti/comments/gu2yox/iot_vlan_settings_specific_to_airplay_apple_tv/)
