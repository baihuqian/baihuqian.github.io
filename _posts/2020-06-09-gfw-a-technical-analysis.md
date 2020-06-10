---
layout: post
title: 'GFW, a Technical Analysis'
date: '2020-06-09 21:44'
tags:
 - Networking
---

The Great Firewall (GFW) is the infrastructure that filters and blocks internet traffic to and from China. It is widely known to censor and block access to certain foreign websites from inside of China and thus a frequent accusation point by China critics. But it is less known to defend Chinese network infrastructure from foreign cyber attacks. This post will not cover any political and ideological topics, but to discuss GFW and technologies to circumvent it from a network security perspective.

# Mechanisms
GFW mainly have three ways to filter and block network traffic: active filtering, active probing, and proxy distribution, among which active filtering is the most widely-known.

## IP Range Ban
The oldest method is to ban and block hole certain IP ranges known to blocked domain names. It is quite unreliable as IPv4 addresses get very fragmented these days and the prevalence of content-delivery network. It is largely automated to block frequent IP addresses identified by other methods.

## DNS Spoofing, Filtering and Redirection
Because DNS queries are plaintext, it can be spoofed and filtered. DNS requests are analyzed, and the GFW returns an invalid result if a blocked domain name is part of the request. It is not unique to GFW to spoof DNS traffic, as ISPs around the world collect user access information by spoofing DNS as well. I have written [a post on DNS spoofing]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}) in the context of ad-blocking. Typical circumvention methods include modifying the Hosts file, typing the IP address instead of the domain name in a Web browser or using DNS over TLS/HTTPS.

Note, prevention of DNS spoofing does not stop network traffic analysis. Many websites use well-known IP addresses and the destination IP address field in HTTP/HTTPS requests are plaintext, thus open to traffic pattern analysis. In addition, the public IP of a network is often dynamically obtained via DHCP or statically allocated when the connection is purchased, thus ISPs can easily map a public IP to a user and relate HTTP/HTTPS requests to a user.

## URL filtering using transparent proxies
GFW runs transparent proxies that scan the requested URI, the "Host" Header and the content of the web page (for HTTP requests) or the Server Name Indication (for HTTPS requests) for target keywords.

## Traffic Pattern Analysis
Through Deep Packet Inspection (DPI), GFW is able to detect certain patterns of network traffic that aim to circumvent the GFW. A few simple example is SSH, VPN, or Tor tunneling.

## Other
Other less well-understood mechanisms include packet forging and TCP reset attacks (effectively arbitrarily terminating TCP connections), and man-in-the-middle attacks with valid certificates.

# Circumvention in Theory
One can circumvent the GFW by hiding the true intention of the traffic and turning the "invalid" network traffic, such as HTTPS requests to a blocked website, into "valid" traffic. It includes mainly two parts: encryption and obfuscation. Encryption aims to hide the content of your network traffic so that third-parties cannot analyze and hijack it. It is widely used in modern internet traffic with HTTPS/TLS and VPN. Obfuscation aims to hide the intent of your network traffic. For example, DPI can easily identify proxied HTTP traffic and IPSec VPN traffic by analyzing packet headers and packet sequences.

# Split Proxy
## Shadowsocks
[Shadowsocks](https://shadowsocks.org/assets/whitepaper.pdf) is a secure split proxy based on [SOCKS5](https://www.ietf.org/rfc/rfc1928.txt). SOCKS protocol operates at the OSI Layer 5 (session layer) and proxy TCP connections to arbitrary destinations via the use of a proxy server. SOCKS5 add strong authentications to proxy servers and capability to proxy UDP traffic.

Split proxy means the proxy is broken into two parts, `local` and `remote`, and the traffic between `local` and `remote` is encrypted. For TCP traffic, `local` initiates and maintains a TCP connection to `remote`, which establishes and maintains anther TCP connection to the true destination. For UDP traffic, `remote` performs NAT for `local` besides encryption/decryption.  Traffic between `local` and `remote` is encrypted with AEAD Ciphers, which stands for Authenticated Encryption with Associated Data. AEAD ciphers simultaneously provide confidentiality, integrity, and authenticity.

`local` is run on the local machine originating the traffic or the edge gateway of a LAN that acts as a transparent proxy to your network. `remote` is run outside China and thus able to access blocked websites. Traffic between `local` and `remote`, which traverses through the GFW, looks like regular TCP and UDP traffic with an unidentifiable payload, thus circumventing the filtering and spoofing by GFW.

There are other protocols based on the same idea of split proxy, such as ShadowsocksR (SSR), [VMess](https://www.v2ray.com/en/configuration/protocols/vmess.html) (used in V2Ray), or [Trojan](https://trojan-gfw.github.io/trojan/protocol) (proxy through TLS).

## WebSocket + TLS
WebSocket + TLS obfuscates network traffic into something almost identical to HTTPS by operating over TCP port 443 and is encrypted with TLS. However, the network traffic pattern may not be similar to normal HTTPS and the traffic target is not a website or web server. With active probing of the trojan server by GFW, it is possible to detect suspicious trojan server and ban its IP by the GFW.

It is possible to hide the `remote` server in an actual website or web server that responds to normal HTTP/HTTPS requests, thus turning WebSocket + TLS traffic into website access. However, it is still possible to detect abnormal traffic pattern between a given IP inside China to a certain, often less accessed website.

V2Ray and Trojan both use WebSocket + TLS. V2Ray adds another encryption layer, VMess, on top of TLS, and thus requires more computing power.

## Local Server
Local proxy server can be run on the local device originating the traffic, or the edge gateway. Most protocols have applications that implement the protocol and the `local` functionality (often referred as `client`) that can run on a variety of OS, including smartphones. Depends on the permission and the ability of intercepting network traffic, these applications can proxy various level of traffic. Applications that do not honor network settings in OS cannot be proxied using such applications.

Most small LAN network uses a single router as the gateway. As the single exit point of all traffic to the internet, it can act as `local` server to proxy traffic. However, most home or enterprise-grade routers have limited CPU power as most network functionalities are implemented in silicon, but proxy protocols all use encryption algorithms that requires significant computing power. This results in unstable device performance and unable to reach the WAN speed.

Most recommended setup is to have a desktop machine running as the gateway. Software such as [Clash for Windows](https://docs.cfw.lbyczf.com/) and [Surge for Mac](https://nssurge.com/) can act as the transparent proxy in the local network.

## International Internet Access and Remote Server
The success of circumventing the GFW and the network quality of cross-border traffic depends on the accessibility of the remote server and the quality-of-service (QoS) of the international internet access.

There are many internet access lines and virtual private server (VPS) for hosting the `remote` server. It is not recommended to set up your own `remote` server, because
1. Complexity. You are responsible to choose internet access lines, hosting, IP, DNS, software, and testing.
2. Access to only a few locations, and multi-location deployment requires setting up and maintaining multiple VPS hosting.
3. Ongoing maintenance. Software upgrades and configuration testing are required.

Thus, it is recommended to find a reliable service provider as your `remote` server. [This website](https://www.duyaoss.com/) provides testing results for major service providers.

#### CN2
CN2 stands for Chinatelecom Next Carrier Network provides QoS to international internet access. There are two types, CN2 Global Transit and CN2 Global Internet Access. CN2 Global Internet Access is the best but expensive.

#### BGP
BGP network can dynamically switch between ISPs based on congestion, latency, and status. Hosting remote server in a data center with BGP can reduce latency and jitter variance due to ISP and internet weather.

#### Cloud Providers
AWS, Azure, and GCP provide hosting, but they mostly focus on international enterprise customers and the cost for bandwidth is quite high.

#### PCCW, HKT, HKBN
Hong Kong SAR is not within GFW but is close to mainland China. It is a popular location for data center providers. PCCW is hosted in Hong Kong SAR with dedicated connection to mainland China.

#### IPLC
IPLC stands for International Private Lease Circuit. It is dedicated lease line to access the international internet. It is very expensive on monthly and bandwidth charge. IPLC does not go through GFW.

# VPN
Shadowsocks runs on OSI Layer 5 and WebSocket + TLS runs on Layer 4 and 7. Due to this limitation, not all network traffic can be proxied. Exceptions include Layer 3 protocols such as ICMP (ping and trace route) and certain Layer 4 protocols.

Most of popular Virtual Private Networking (VPN) protocols, such as PPTP, L2TP, OpenVPN, and SSTP, runs on Layer 2 and Layer 3, allowing to proxy almost all network traffic. Though VPN can encrypt traffic, but its traffic pattern is well-defined and easily distinguishable, making it easy for GFW to detect and block.

WireGuard is a new and simpler VPN protocol. It is UDP-based, making it immune to TCP-reset attack and not as characteristic as connection-based protocols. However, UDP traffic is heavily shaped by ISP based on QoS, making it less reliable to traverse through lower-grade internet.
