---
layout: post
title: "Why (Most) Software-Defined Networking Sucks in Performance"
date: '2019-03-28 22:26'
tags:
 - Networking
---

Software-defined networking (SDN) is a trending buzzword these days in the tech world. However, most SDN solutions fall short in terms of network performance. Why so?

Traditionally, networking is made possible by hardware devices, such as routers, switches, and firewalls. They are designed and manufactured by vendors such as Cisco, Juniper, Palo Alto Networks, and Huawei. These gears are purpose-built to perform a set of network functionalities very well, such as routing, switching, or packet inspection. On the other hand, almost all network protocols and functionalities have been implemented as software, either as a part of the Operating System or kernel module, or an application. However, the performance of software-based implementation lags far behind from their hardware counterparts with the same amount of computing power.

Network functionalities can be roughly divided into two categories: packet handling and configuration. Packet handling performs operations on electronic or optical signals arriving at your network ports and sends them away. Such operations include forwarding (for routing and switching), encryption/decryption, encapsulation/decapsulation, packet inspection, and handling of control packets such as ICMP or BGP. Configuration allows you to tailor the network functionalities to your needs, through the provided interface, such as a Command-Line Interface or web Console. These interfaces allow you to monitor and troubleshoot the network as well. They are often referred to as the data plane and the management plane, respectively.

Packet handling operates under a fixed and finite set of rules, requires high throughput, and often involves low-level operations on memory or network buffer. Packet handling rules are well-defined in RFCs, and changes are rare and prolonged (one example is the migration to IPv6). Configuration, on the contrary, is much slower in change, but the capability changes constantly. It involves high-level operations suitable for human interactions, but functionalities evolve through time with firmware upgrades.

The secret of high-performance networking provided by hardware devices is to tackle packet handling and configuration in vastly different ways. Packet handling is done in purposely-built chips and circuit boards called line cards, while the configuration is provided by a custom OS running on a CPU, often much cheaper than a general-purpose computer. The device configuration is written to the onboard memory by the device OS in a specialized format.

Compared to software-based networking solutions, the CPU of the networking devices is freed from doing any of the mundane packet handling routines, thus requires a lot less computing capacity. However, whether the configuration and the interface of the device can be easily done and upgraded, the specification of the line card dictates the network functionality and capacity of the device that cannot be easily upgraded unless a new one is installed.

On the other hand, because configuration changes are thought to be rare and the total amount of configurations is not significant in most cases, device vendors often design the weakest configuration capacity possible to reduce cost. Thus, the hardware devices are often described as "strong data plane, weak management plane." In most cases, upon a commit, the entire configuration for the device is generated and written to the line cards.

Software networking, on the other hand, does not rely on specialized hardware. In most cases, Layer-3 and above are not handled by hardware, but OS and applications. CPU is involved in performing packet handling. There are several reasons why it is slow:
1. Unless specialized drivers and application is used, packet handlers are triggered by interrupts and usually require user-kernel switches.
2. Most open-source network projects focuses on the comprehensive implementation of network protocols, rather than performance. This can attract more use cases, but the performance lags behind in any serious usage.
3. The networking stacks in the OS cannot perform as good as hardware.

However, SDN's configuration is often easy to perform, easy to upgrade, and easy to extend. The same CPU that performs the packet handling can now be used to support more and better configurations, quicker to mutate, and a large number of software are available for any needs. Thus unlike hardware devices, the management plane is strong, but the data plane is not.
