---
layout: post
title: 'Secure Home Network: Using Chromecast across VLANs'
date: '2020-12-13 19:23'
tags:
 - HomeNetwork
 - CordCutter
---

In previous posts, I discussed why you should [isolate connected devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}) and how to add pinhole rules to [allow AirPlay to work across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-09-secure-home-network-using-airplay-across-vlans.md %}). This post discusses another use case: Chromecast.

Not only Chromecast devices implement this functionality, but also TV apps such as Youtube or Netflix use this protocol to allow smartphones to cast content to smart TVs. Chromecast works similar to DLNA such that content is streamed from server to TV directly, instead of server to smartphones and then to TV in AirPlay. It also allows smartphones to control the playback on TV, similar to DLNA.

There are three parties in the Chromecast use case: client (TV) receiving the content, server which is the internet-based content provider, and control (your smartphones). We assume the client is already able to reach server via internet. Firewall rules should be added to allow client to interact with control. Client communicates with control using
* TCP traffic from port 8008-8009 and 8443;
* UDP traffic from port 32768-61000;
* UDP traffic from any port to control on port 32768-6100.

Assuming client is in the Device VLAN and control is in the secure VLAN, and secure VLAN is able to communicate with device VLAN by default, the following firewall rules should be added to the Device VLAN for direction *in* (assuming an address group `chromecast_address` is created with all Chromecast-supported devices):

```
**Port Group for Chromecast**
Group Name: chromecast
Type: port-group
Port: 32768-61000

**DEVICE_IN Rule**
Description: allow TCP traffic from chromecast
Action: Accept
Protocol: TCP
Source: Port 8008-8009,8443, Address Group chromecast_address
Destination > Network Group : LAN_NETWORKS

Description: allow UDP traffic from chromecast (src)
Action: Accept
Protocol: UDP
Source > Port Group: chromecast, Address Group chromecast_address
Destination > Network Group : LAN_NETWORKS

Description: allow UDP traffic from chromecast (dst)
Action: Accept
Protocol: UDP
Source > Address Group chromecast_address
Destination > Network Group : LAN_NETWORKS, Port Group: chromecast
```

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).

# References
1. [IoT VLAN settings specific to Chromecast](https://www.reddit.com/r/Ubiquiti/comments/gq0999/iot_vlan_settings_specific_to_chromecast/)
