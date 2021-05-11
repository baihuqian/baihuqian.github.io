---
layout: post
title: >-
  Secure Home Network: Make Your IoT Devices Compatible with Apple HomeKit with
  Homebridge
date: '2021-01-17 11:31'
tags:
 - HomeNetwork
 - IoT
---

***Update May 10, 2021: Home Assistant has since greatly improved its usability for non-hardcore users with more UI-based configurations and it has native HomeKit support. I have migrated from Homebridge to Home Assistant.***

Previously, I set up [Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %}) to be my home automation platform. Though it supports a plethora of brands and devices, it is community-driven and is not very user-friendly. It's configuration is YAML-based and should be checked via a command-line tool before applying it, it's UI is hard to adjust, and sharing it with other family members is not easy at all. In addition, a recent update that breaks my Yeelight integration and I spent quite some time reading through GitHub issues to figure out what version of Home Assistant to roll back to and when it is safe to upgrade again. It is too hard for a casual user to have as a simple reliable automation platform.

I researched other leading platforms like Alexa, Google Home, Apple HomeKit, Samsung SmartThings, IFTTT, and so on. I do not like the requirement of special hubs (such as Zigbee or Z-Wave), so most of my devices work with WiFi. I focus greatly on privacy and security, so cloud-based platforms are not as preferable as those supporting local point-to-point communications. Lastly, device discovery relying on broadcast or UPnP (which is used by so many other use cases, too) makes network chatty, so I prefer mDNS or special-purpose multicast-based device discovery. So Apple HomeKit fits most of my requirements, and I am already in the Apple ecosystem.

However, the problem with HomeKit is its inferior device support compared to other major platforms, namely Google Home and Alexa. Due to the requirement of local communication and encryption (which is a good thing), it is harder for device manufacturers to implement support and sometimes require special hardware. Thus, HomeKit-supported devices in many cases are more expensive.

Home Assistant basically creates a standardized interface to all types of devices and vendor support libraries implement the translation layer between vendor-specific protocols and Home Assistant interface. Can we do the same with HomeKit, too?

The answer is [Homebridge](https://homebridge.io), which allows you to integrate with smart home devices that do not natively support HomeKit. It is a plugin-based system that can interface with many vendors, and manifests itself in iOS Home App as a bridge. Devices added to Homebridge automatically shows up in the Home App, and most vendor plugin requires very minimum amount of setup to get it going.

Installation is a breeze. It's [GitHub page](https://github.com/homebridge/homebridge) includes detailed, step-by-step guide for all major platforms. It supports installation on Docker and even has [a nice documentation for Synology NAS](https://github.com/oznu/docker-homebridge/wiki/Homebridge-on-Synology) too! It's [UI](https://github.com/oznu/homebridge-config-ui-x) comes as a separate package but I highly recommend it even for pro-users. It is a lot easier to search, install, and configure plugins via the web interface than command line (`npm`) and text editor.

My first use case is [myQ Smart Garage Door Opener](https://www.amazon.com/Smart-Garage-Opener-Chamberlain-myQ-G0401/dp/B08GD3D9YJ/) as I constantly forget to close the garage door when driving out. The Hub and sensor is very easy to use and reliable, but it does not natively support HomeKit. You would need to get a [hardware device](https://www.chamberlain.com/myq-home-bridge/p/MYQ-G0303-SP) that costs a whopping $70 and people report that it is not that reliable. [Homebridge's myQ plugin](https://www.npmjs.com/package/homebridge-myq) is simple to configure and **just works**. It polls from the myQ server like the app and translate information to HomeKit. The default configuration is good enough for me, as my Home app and myQ app sends notification about garage door open/close almost the same time. And I can use the Home app to control my garage door with no significant delay.

So far it has been reliable for me. However, certain devices are not so easy to configure, mainly due to the connectivity to the device itself requires a lot of digging. After all, we are trying to perform out-of-band control to IoT devices and we should expect local communication to be hard. Otherwise it would be a security vulnerability.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
