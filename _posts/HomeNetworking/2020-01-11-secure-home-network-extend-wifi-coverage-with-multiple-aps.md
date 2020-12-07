---
layout: "post"
title: "Secure Home Network - Extend WiFi Coverage with Multiple APs"
date: "2020-01-11 15:06"
tags:
 - HomeNetwork
---

In many cases, you can get good WiFi coverage with only one Access Point (AP). However, when

1. The layout is elongated or too large;
2. There are too many walls, especially reinforced concrete or thick brick walls, in the location;
3. It is not possible to place the AP at or near the center of the location.

WiFi coverage is not sufficient with only one AP. To improve signal strength and coverage, you can use either WiFi extenders (or repeaters) which re-broadcasts your WiFi signal from your existing single AP, or multiple APs. This post discusses the latter with multiple UniFi APs.

There are many aspects of considerations involved in designing your WiFi environment using multiple APs, so they can work effectively instead of interfering with each other:
1. A single set of SSIDs for all APs, or distinct SSIDs?
2. Which channels to use for each AP?
3. How to set the suitable radio transmission power?
3. How to connect APs together?
4. How to effectively place WiFi clients onto the best AP?

Unfortunately, there is no one-size-fits-all answer. Because the WiFi standard, IEEE 802.11, leaves the selection of APs to the client, there is no authoritative way for network administrators to enforce WiFi policies. Rather, the best thing we can do is to design and implement our WiFi network so that the commonly-used clients work the best with it.

## SSID
This is not controversial: you should have the same SSID(s) for all your APs. To be more precise, the SSID, authentication method, and authentication key must be identical to allow clients to roam across APs. In fact, this is the default behavior in UniFi: your wireless network configurations are automatically deployed to all APs in the same site.

## Channels
WiFi spectrum is divided into channels of certain width. 2.4GHz band is divided into 11 channels spaced 5MHz apart, leaving only 3 non-overlapping channels, 1, 6, 11, at 20MHz width. 5GHz band contains channels ranging from 36 up to 165, with width of 20, 40, 80, or 160 MHz. As WiFi uses [Carrier-sense multiple access with collision avoidance (CSMA/CA)](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access_with_collision_avoidance), APs using the same channel cannot operate simultaneously. Therefore, it is vital to assign distinct channels to different APs.

UniFi devices can scan the spectrum at startup time and pick the least-used channel. However, this is not reliable with multi-AP deployment as APs can pick the same channel. If you have fewer than 4 APs, you can easily assign distinct channels to different APs. Otherwise, re-use the same 2.4GHz channel for APs that are most apart, or even disable 2.4GHz radio on some APs.

My recommendation:
* For 2.4GHz band, choose distinct channels from channel 1, 6, and 11, with channel width set to HT20 (20MHz).
* For 5GHz band, choose distinct channels with channel width set to VT40 (40MHz).

## Radio Transmission Power
The power of the radio determines how far it can go from the sender to the receiver. Most wireless clients have limited radio transmission power to reach back to APs, thus it is undesirable to set the radio transmission power of APs too high that clients can connect to it but not transmit data back.

Because 2.4GHz radio attenuates slower than 5GHz radio and thus transmits farther, it is recommended to set the 2.4GHz power lower than that of 5GHz. I recommend to set 2.4GHz to low and 5GHz to low or medium depend on the type of your UniFi APs (i.e. long-range or not) and layout (more obstacles means faster attenuation for 5GHz radio).

## Uplink
APs can connect to your router via two ways: wired uplink via Ethernet or wireless uplink via another AP. It is recommended to have wired uplinks as wireless uplinks is less reliable and prone to interference compared to Ethernet. Plug all your APs to the same switch with identical VLAN settings, if any.

If you use wired uplinks, be sure to **uncheck** "Allow meshing from other access points" under "Devices" - "Config" - "Radios".

## Help Clients Choose the Best AP
This is the artistry bit in the network design. The goal here is to place clients onto the "closest" AP possible while minimizing the delay of switching among APs, also known as "roaming".

As previously discussed, the WiFi standard leaves the choice of AP to the client. Most of the critical devices are Apple devices, and thus I heavily rely on Apple's implementation of the WiFi standard to guide the design of my network. Here are the documentation from Apple:

* [About wireless roaming for enterprise](https://support.apple.com/en-au/HT203068)
* [Wi-Fi network roaming with 802.11k, 802.11r, and 802.11v on iOS](https://support.apple.com/en-us/HT202628)

Without going into the details of how UniFi implements these protocols, here are my recommendation:
1. **Check** "Enable advanced features" under "Settings" - "Site", in "Services" section. This enables *advanced features" used later in this section.
2. **Disable** "Automatically Optimize Network and WiFi performance" under "Settings" - "Site", in "Auto-Optimize Network" section.
3. If using WPA Personal as the authentication method: **Uncheck** "Enable fast roaming" under "Settings" - "Wireless Networks" - "[Network Name]", in "Advanced Options" section. Although the tooltip explains that it implements certain 802.11r features, which are supported by Apple devices, it is said to work only with WPA Enterprise with a RADIUS server.
4. Set "Band Steering" to "Prefer 5G" under "Devices" - "Config". This is only visible after "advanced features" are enabled.
5. Set "Minimum RSSI" (Received Signal Strength Indicator) to -75 dBm, under "Devices" - "Config" - "Radios". Do not enable "Strict Mode" or "Cell Size Tuning". Minimum RSSI forces APs to drop a client if the received signal strength is below the threshold. You may want to fine-tune this value for each AP to ensure that this is set to the highest value possible without creating dead spots in your location. You can read more about Minimum RSSI in [this UniFi article](https://help.ubnt.com/hc/en-us/articles/221321728-UniFi-Understanding-and-implementing-minimum-RSSI).

With this setting, I find it is mostly fast to switch among APs with only a few seconds of delay. VoIP applications may drop the communication for a while during roaming though.

## Reference
I found this [useful FAQ post](https://community.ui.com/questions/Wireless-LAN-Roaming-FAQ/3044afc5-55ac-4c52-804d-2fbb91381e60) for WLAN Roaming.


## Further Reads
This is the post series. Other posts on the home network topics are:
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
