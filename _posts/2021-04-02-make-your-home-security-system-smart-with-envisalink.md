---
layout: post
title: Make Your Home Security System Smart with EnvisaLink
date: '2021-04-02 17:30'
tags:
 - HomeNetwork
 - HomeSecurity
 - IoT
---

You probably have a security system in your home, mostly from Honeywell or ADT (they're pretty much the same thing). The system has dumb-looking keypads or less likely, a touch screen system. But you still need to learn how to use them (they're not entirely easy to use) and be physically near the keypads to , operate them. However, the good thing is they are super cheap, reliable, and pervasive.

I inherited an existing Honeywell system from the previous owner that I never use. It seems to operate fine; but I would never remember to arm in the night and disarm in the morning, and I absolutely hate the numeric keypad that asks you key in your code plus some digit to program it. The faults are represented as codes, and I have to run to the keypads to see what happens when it chimes. As someone with Electrical Engineering background, I absolutely understands the mechanism behind this; but as a user I really hate to even try to work with it.

Usability is the natural enemy of security. Secure solutions and products are sometimes so hard to use that it creates so much trouble or annoyance that people rather takes the risk and not to use them. As a smart home DIYer, I set out to seek a solution that can fix this.

# My Requirements
First, I'm a firm believer of no-subscription and no-vendor-lock-in. I don't want to buy a product that requires continuous payment of which I have absolutely no control. Otherwise the products became useless if I decide to stop paying because I don't want to or the vendor raises price.

Second, it supports local control over network. Internet-based control is a nice-to-have but should not be a must. In this way I can disable its access to the Internet if necessary. I hope it can work with HomeKit but it is also a nice-to-have.

Third, it does not break the bank. This leads to the last one: it works with my existing Honeywell system.

# EnvisaLink
I found [EnvisaLink](https://www.amazon.com/Envisalink-EVL-4EZR-Interface-Honeywell-Compatible/dp/B016WQTJ4S), a small board that interfaces with my existing Honeywell system like a keypad and have an Ethernet port to my home network. It's designed and manufactured by a small Canadian company called EyezOn, who also offers an Internet-based monitoring for a low price.

It has a TCP-based event-driven notification system, and [Home Assistant has native support for it](https://www.home-assistant.io/integrations/envisalink/). The event-driven local communication makes it super responsive to state changes, and zones in the security system can be mapped to sensors in Home Assistant, allowing me to build automation for it. In addition, Home Assistant has native support for HomeKit, and the alarm system and sensors can be exposed to HomeKit. Then I can view the zone status and arm/disarm the system right from my iOS devices.

# Programming, Installation, and Configuration
Before installing the board, I need program my existing security system to make it workable.

Because the previous owner did not disclose the installer code or the master code, I have to reset both codes. I reset the installer code using [the backdoor method](https://www.alarmgrid.com/faq/how-do-i-backdoor-into-programming-on-a-honeywell-vista-system-i), and then [reprogram the master code and user codes](https://www.alarmgrid.com/faq/how-do-i-get-into-programming-mode-on-my-vista-20p). Note, the backdoor method works while the keypad says "busy" during initialization.

Installation of the board is straightforward after figuring out the access to the locked enclosure of the alarm system. The board can be mounted or taped to the enclosure, and there is a 4-wire cable that connects the screwdown on the board to terminal 4 to 7 on the security system. The security system has wiring diagrams for each terminal. Then connecting the board to switch via an Ethernet cable and power up the system. The board should self register, upgrade, and program within 10 minutes.

Because I own a Honeywell system, it has extra steps to program on the keypad. The guide is not very clear so I have to consult to the installation manual of the security system; but after going through it, the EnvisaLink works through its online portal, though the website looks outdated.

Setting up Home Assistant integration is simple, too, by following the documentation. The longest time it took me is to figure out which zones are in use and for what areas (by running around in the house and opening up windows and doors). After integrating to EnvisaLink, you can expose the alarm system and binary sensors for each zone in Home Assistant's HomeKit integration to make them visible in your Home App.

# Network Considerations
Being an IoT device, it should not be connected to the main VLAN. Instead, it should be in the [IoT VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}) with no access to other devices on the same network.

However, it does not work during initial setup because the EnvisaLink board does not accept incoming connections from a different subnet. You will need to be on the same VLAN of the board (i.e., the IoT VLAN) to change its password before accessing it from other VLANs. I have confirmed this with EyezOn's technical support.

The board only requires outbound UDP connections and TCP on port 4025, so it is possible to restrict it in the firewall.
