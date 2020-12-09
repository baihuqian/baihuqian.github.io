---
layout: post
title: "Secure Home Network - Using HomeKit Devices Across VLANs"
date: '2019-09-10 22:18'
tags:
 - HomeNetwork
---

_TL;DR Version: Your iOS devices should be able to connect to the HomeKit Devices on port 80 and 443, and  mDNS should work between VLANs._

In previous posts, I discussed [why and how to set up multiple VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}) and now all those Internet-connected devices are away from the LAN where your laptops and NAS sit. Then you realize, my iOS devices on the secure VLAN can no longer connect to my HomeKit-enabled devices on the Device VLAN. This post discusses how to add selective Firewall rules to allow HomeKit functionality.

HomeKit uses the [HAP Protocol](https://developer.apple.com/support/homekit-accessory-protocol/), which actually uses peer-to-peer connectivity for really fast action when you try to perform actions. If your device is unable to reach the HomeKit device, it will, through iCloud, try to perform the HomeKit action through your hub (such as Apple TV). If your hub is unable to reach the HomeKit device, the request will fail. If your hub is in the right VLAN and does reach it, it will work, however, it will be slower than if it had worked directly from your device. And believe me, 500ms of lag when trying to turn on the lights can be a significant pain.

The following settings are added to make it work. *Note:* This is not the most restrictive configuration possible. I am sure you can lock it down more, especially if everything you have has known hostnames or assigned IPs that do not change.

1. Allow your secure VLAN to connect to port 80 and 443 of Device VLAN. You can do this via IP to IP + port rules, or, if you do not mind your secure VLAN reaching the Device VLAN, simply add allow rule to *in* direction of your secure VLAN for port 80 and 443 with destination Device VLAN. _You can make this more restrictive by only allowing the static or reserved IPs of devices you use with HomeKit. Be sure to include your hub so your schedules work when you're away._
2. Allow established/related packets from Device VLAN (direction: *in*).
3. ensure mDNS can traverse through VLANs. An mDNS message is a multicast UDP packet to/from IPv4 address of 224.0.0.251 and UDP port 5353. This means the combination of:
   1. mDNS reflector or multicast repeater is enabled. I prefer enabling mDNS repeater between the device VLAN and secure VLAN's interfaces, i.e., `switch0.10` and `switch0.20`. Note that mDNS reflector enables mDNS on **ALL** interfaces, including the WAN interface, thus it is bad.
   2. If the default action for `DEVICE_LOCAL` (traffic from the device VLAN to the router) is `drop`, create a rule that allows mDNS traffic.

To add devices to HomeKit, you need to:
2. Disable Guest Isolation on your Device wireless netowrk while you pair the devices (the pairing process seems to require p2p connectivity for some devices, and in any case, some apps will force you to send WiFi settings from your phone, preventing you from being on the main SSID while you do this).
3. Join your IoT SSID with your phone.
4. Add the device to HomeKit.
8. Turn the lights on and off and on and off and on and off. And on and off and on and off.
9. Re-enable guest isolation on your IoT SSID.

Credit: This post is a combination of [Guillaume Ross](https://medium.com/@gepeto42/using-homekit-devices-across-vlans-and-subnets-aa5ae1024939)'s solution of installing Avahi on the router and [John Reed](http://leerspace.com/2015/12/20/bonjour-mdns-reflection-on-ubiquiti-edgeos/)'s solution to allow Bonjour/mDNS on EdgeOS. The solution is improved by using [mDNS repeater instead of mDNS reflector](https://www.reddit.com/r/Ubiquiti/comments/jerhab/mdns_reflector_vs_repeater/).

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
