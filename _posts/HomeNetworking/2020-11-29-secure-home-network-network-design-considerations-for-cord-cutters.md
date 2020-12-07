---
layout: post
title: Secure Home Network - Network Design Considerations for Cord Cutters
date: '2020-11-29 15:21'
tags:
  - HomeNetwork
  - CordCutter
---

Cord-cutters refers to people who cancel their subscriptions to multichannel television services available over cable or satellite or drop pay television channels by replacing them with rival media available over the Internet. I personally don't have any cable TV subscriptions; but I have Amazon Prime Video and Netflix subscriptions, and I play lots of contents via AirPlay and Plex. This post discusses considerations of network design to achieve a good experience for internet-based content viewing.

# Smart TV
Most new TVs today are "smart", meaning they run an operating system that can have "apps". Even if your TV is not, there are cheap HDMI dongles (around 30 to 100 dollars) that can turn your TV into a smart one. These devices run a vendor-modified *Android TV*, a version of the Android operating system developed by Google for television sets, digital media players, set-top boxes, and soundbars. The OS supports apps for streaming services and provides support for mirroring content from other devices, such as smartphones and tablets.

Because the "smart" part of the TV works identical as dongles in the sense that both take content over the network and output video either directly to its screen or to the HDMI port, I will use Smart TV as the term in the rest of the post.

Because now the TV runs apps that can be computationally-intensive, it now contains a ARM-based CPU. The power of the CPU determines the usability of apps, room for future software upgrades that bring in new functionalities, and video codec performance for outputting high-resolution, high-frame-rate content.

# Network Requirements for Smart TV
As the "smartness" requires network access by the TV, and video streaming demands high bandwidth, it is important to understand the network requirement for TV to achieve a good viewing experience.

The two main usage pattern for TV is streaming and mirroring. In streaming, TV retrieves content from the source (such as Netflix and Amazon), decode it, and play it on the screen. Mirroring, which lets the TV to play the content on other devices, uses different technologies. DLNA works by sending the URL of the content to the TV so TV obtains the content directly from the source, effectively identical to streaming. Apple's AirPlay sends the screen of the device directly to the TV, after the device obtains and decodes the content from the source. The TV basically is a wirelessly-connected display for the device.

Streaming and DLNA require enough bandwidth to get 1080p or 4k content by the TV, but they buffer content on the TV to create a smooth viewing experience. AirPlay still requires enough bandwidth to obtain the content at the first place, but it adds an additional hop between the device and the TV, effectively requiring more throughput in the network. Moreover, the TV does not buffer content, so the viewing experience depends greatly on the latency, packet loss, and jitter of the path between the device and the TV. Latency and jitter results in variation and drop in frame rates (late frames are discarded), and packet loss results in glitches in audio and frozen screen in video.

Thus, the network requirements for a good viewing experience on smart TV are:
* Sufficient broadband internet connectivity to stream high-resolution content from service providers. Internet package nowadays can support this without any problem.
* Enough bandwidth between the TV and AP.
* To support AirPlay, enough bandwidth between the device and AP, and the AP should be able to simultaneously communicate with TV and device with minimum latency, jitter, and packet loss.

# Network Connectivity for Smart TV
Smart TV has network connectivity built-in. However, as this is an often-overlooked feature compared to size, panel, and resolution, manufacturers tend to cut the cost of network functionality as much as possible to lower the price. For example, some 4k TVs support only 2.4GHz WiFi in recent-year's model, or the WiFi antenna is housed near the main motherboard in the back side of TV, or only 10/100 Mbps ethernet port. Thus, it is important to pay attention to TV's network functionality when choosing a model.

Second, even with support for 802.11ac/ax WiFi, the reception of WiFi signal may not be great, as TVs are placed close to a wall or even mounted with metal TV mount. This is more true for TV dongles as they are hidden behind the TV panel by design, resulting in signal shielding by their surroundings. The result is lower RSSI on high-frequency radios.

Third, AP communicates with both TV and device when using AirPlay over WiFi. Before multi-user MIMO (MU-MIMO) was adopted by Next-Gen AC or AC Wave 2 and the upcoming 802.11ax, AP can only communicate with one client at a time. This results in congestion and packet loss, higher latency and jitter.

Lastly, given wireless network uses a shared medium of radio, it always has higher latency, jitter, and packet loss compared to Ethernet. You can achieve theoretical bandwidth very consistently, steady latency, and very low to no packet loss, variation is larger over wireless network even clients are closer to the AP.

# Recommendation

Therefore, I strongly recommend connecting TV to your network via Ethernet. A wired connection provides consistent performance for both streaming/DLNA and AirPlay use cases and significantly reduce the data transfer in your wireless network.

If there is an Ethernet port nearby but you have multiple devices, such as TV, soundbar, console, etc., consider using a small PoE-powered switch behind your TV to fan-out wired connections to them. I recommend UniFi Switch Flex Mini (USW-Flex-Mini), a tiny 802.3af/USB-C powered 5-port switch. It is slightly larger than a credit card and you can easily attach it to the back of the TV. It is managed by UniFi and can have VLAN profiles for every port, important as entertainment devices notoriously gather information about your network and send home (see my post on [isolating connected devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}) on how to put them on a separate VLAN and protect your other devices on network,  [using AirPlay across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-09-secure-home-network-using-airplay-across-vlans.md %}), and [blocking tracking by TV with Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})).

If wired connection is not possible, then make sure your TV supports 5GHz radio and 802.11ac (as of now, 802.11ax is still too new). If you have older version of APs, consider getting a new one that supports AC Wave 2 (like UniFi's "HD" access points). If possible, move or reconfigure your access points to be closer to your TV by improving the RSSI of the TV, which you can find it in "clients" page of your UniFi controller.

The bottom line is, a wired Ethernet connection, even at 100Mbps, is more cost-effective and reliable than wireless solution.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
