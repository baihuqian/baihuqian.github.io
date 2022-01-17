---
layout: "post"
title: "Secure Home Network: Enable Hardware Offloading on EdgeRouter"
date: "2022-01-17 11:09"
tags:
  - HomeNetwork
  - EdgeOS
---

Since I started tinkering my home network, even before I started writing about it, the speed of residential Internet has increased in the US, or the price-per-Mbps has gone down. You can get more than 500 Mbps for the same price of 250 or even 100 Mbps 3-5 years ago, and we probably have upgraded to higher speed. But are we able to utilize the full speed in our home network?

Wireless connection is usually limited by the WiFi performance; but Gigabit wired connection is dead cheap now and any sensible network devices, not limited to those mentioned on this blog, should be able to achieve north of 900Mbps sustained. So the wired network is not the bottleneck. But when you run a speed test using [speedtest.net](https://www.speedtest.net), or using iperf, you may not always get to that high number. Why?

It is your EdgeRouter. As capable as it is for its low price and small form factor, its CPU and software is unable to perform all operations at line rate (1Gbps). Traffic traversing the router, mainly inter-VLAN and private-Internet traffic, are processed by EdgeRouter. You can move high-traffic devices, such as your surveillance cameras and your recording and viewing devices, into the same VLAN, but it may not be a [good idea]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}). And in my own experience, I'm only getting about 300Mbps of Internet speed despite having 900 Mbps from my ISP. Do I need to upgrade?

Having EdgeRouter's tiny CPU to do all the packet processing is a bad idea. Each router has ASICs for high-performance packet processing, and such ASICs can usually do more than switching packets between interfaces at line rate. However, these functionalities are disabled by default in the EdgeRouter.

I have an EdgeRouter-X which is based on MediaTek chip. VLAN and NAT can be offloaded to the ASIC using the `hwnat` command, along with other tunneling features such as GRE and PPPoE. To enable hardware offloading using CLI command: `set system offload hwnat enable` or navigate to the Config Tree: `system -> offload` and input `enable` next to `hwnat`.

Before enabling hardware offloading, when I run speed test, the CPU usage of EdgeRouter goes up to more than 80% while throttling at slightly above 300 Mbps. After enablement, the CPU usage does not move when I saturate my Internet at 900 Mbps.

If you use a cable modem for your Internet, also make sure it supports the speed you're buying. The cheaper models usually support fewer channels and lower speed so your Internet may bottleneck there before EdgeRouter's software NAT functionality.
