---
layout: "post"
title: "Secure Home Network - Using AirPlay Across VLANs"
date: "2019-08-31 21:29"
tags:
 - Networking
---

AirPlay uses Multicast DNS (mDNS), implemented in Apple Bonjour, to discover compatible devices on a local area network (LAN). Thus by default, devices on different VLANs cannot discover each other, thus breaking the AirPlay functionality.

To allow Apple devices in the main VLAN to use AirPlay on TV and speakers in the IoT VLAN, make sure the following:
1. your TV and speakers can connect to the Apple devices in TCP and UDP on a random port in the 49152-65535 range;
2. enable mDNS reflection in the router (see instructions in [this post]({{ site.baseurl }}{% post_url HomeNetworking/2019-08-27-secure-home-network-using-homekit-devices-across-vlans %})).
3. allow established/related back from the IoT VLAN to the main VLAN.

1 and 3 can be set up using firewall rules. Make sure
1. Accept rules must come before the drop rule for RFC1918 traffic.
2. Use TV and speakers' MAC address as source to skip setting up static IPs for them.

The firewall rules look like this:
![Firewall rule for AirPlay](/assets/posts/HomeNetworking/airplay.png)

# Further Reads
This is the post series. Other posts on the home network topics are:
1. [Device and Management Setup]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-07-secure-home-network-device-and-management-setup.md %})
1. [Isolating Connected Devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-02-secure-home-network-isolating-connected-devices-with-vlan.md %})
1. [Using HomeKit Devices Across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-08-27-secure-home-network-using-homekit-devices-across-vlans.md %})
1. [Troubleshoot DHCP Problems]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-troubleshoot-dhcp-problems.md %})
1. [Extend WiFi Coverage with Multiple APs]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-extend-wifi-coverage-with-multiple-aps.md %})
1. [Backup Your Configurations]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-11-23-secure-home-network-backup-your-configurations.md %})
1. [Block Ad and Tracking with Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})
1. [IoT Automation with Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %})
1. [Set Up a Plex Server]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-10-18-secure-home-network-set-up-a-plex-server.md %})
1. [Place APs for Optimal Coverage]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-11-29-secure-home-network-place-aps-for-optimal-coverage.md %})
1. [Network Design Considerations for Cord Cutters]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-11-29-secure-home-network-network-design-considerations-for-cord-cutters.md %})
