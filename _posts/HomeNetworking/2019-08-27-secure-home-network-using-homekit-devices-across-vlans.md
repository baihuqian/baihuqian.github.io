---
layout: post
title: "Secure Home Network - Using HomeKit Devices Across VLANs"
date: '2019-08-27 22:18'
tags:
 - Networking
---

_TL;DR Version: Make sure your iOS devices can connect to the HomeKit Devices on port 80 and 443, and enable mDNS reflection in the router._

So, you've setup multiple VLANs at home, you're keeping all those Internet Connected devices away from the LAN where your laptops and NAS sit. Then you realize, my iOS devices on the main VLAN can no longer connect to my HomeKit-enabled devices.

HomeKit uses the [HAP Protocol](https://developer.apple.com/support/homekit-accessory-protocol/), which actually uses peer-to-peer connectivity for really fast action when you try to perform actions. If your device is unable to reach the HomeKit device, it will, through iCloud, try to perform the HomeKit action through your Apple TV.

If your Apple TV is unable to reach the HomeKit device, the request will fail. If your Apple TV is in the right VLAN and does reach it, it will work, however, it will be slower than if it had worked directly from your device. And believe me, 500ms of lag when trying to turn on the lights can be a significant pain.

Here is how to get it working almost as if it was on the same network.

*Note:* This is not the most restrictive configuration possible. I am sure you can lock it down more, especially if everything you have has known hostnames or assigned IPs that do not change.

1. Allow your main LAN to connect to port 80 and 443 on HomeKit devices. You can do this via IP to IP + port rules, or, if you do not mind your main network reaching the IoT network, simply allow 80 and 443 from Main to IoT LANs. _You can make this more restrictive by only allowing the static or reserved IPs of devices you use with HomeKit. Be sure to include your AppleTV or iPad so your schedules work when you're away._
2. Allow established/related packets from IoT LAN (direction: *in*).
2. Disable Guest Isolation on your IoT SSID while you pair the devices (the pairing process seems to require p2p connectivity for some devices, and in any case, some apps will force you to send Wi-Fi settings from your phone, preventing you from being on the main SSID while you do this).
3. Join your IoT SSID with your phone.
4. Add the device to HomeKit.
5. Enable mDNS reflection. In EdgeOS, go to the config tree, navigate to `service` > `mdns` and click the `+` sign right to reflector. After doing this, be sure to click the Preview button to save the change.
7. Reconnect your phone to your main SSID.
8. Turn the lights on and off and on and off and on and off. And on and off and on and off.
9. Re-enable guest isolation on your IoT SSID.

Credit: This post is a combination of [Guillaume Ross](https://medium.com/@gepeto42/using-homekit-devices-across-vlans-and-subnets-aa5ae1024939)'s solution of installing Avahi on the router and [John Reed](http://leerspace.com/2015/12/20/bonjour-mdns-reflection-on-ubiquiti-edgeos/)'s solution to allow Bonjour/mDNS on EdgeOS.

# Further Reads
This is the post series. Other posts on the home network topics are:
1. [Device and Management Setup]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-07-secure-home-network-device-and-management-setup.md %})
1. [Isolating Connected Devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-02-secure-home-network-isolating-connected-devices-with-vlan.md %})
1. [Using AirPlay Across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-08-31-secure-home-network-using-airplay-across-vlans.md %})
1. [Troubleshoot DHCP Problems]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-troubleshoot-dhcp-problems.md %})
1. [Extend WiFi Coverage with Multiple APs]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-extend-wifi-coverage-with-multiple-aps.md %})
1. [Backup Your Configurations]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-11-23-secure-home-network-backup-your-configurations.md %})
1. [Block Ad and Tracking with Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})
1. [IoT Automation with Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %})
1. [Set Up a Plex Server]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-10-18-secure-home-network-set-up-a-plex-server.md %})
