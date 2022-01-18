---
layout: post
title: 'Secure Home Network: Add Remote-Access VPN to EdgeRouter'
date: '2021-08-22 20:36'
tags:
  - HomeNetwork
  - EdgeOS
---

In the era of work-from-home, it is rare of a need to access your home network from outside of your home. In the rarest occasion when we need something from your digital home, from accessing files in your NAS to viewing security camera footage, while being away, it is very inconvenient and less secure to get access from a public internet hotspot.

There are plenty of cloud-based P2P solutions for specific use cases and products, ranging from Synology's QuickConnect that allows you to access your Synology NAS anytime, anywhere, from any device and browser, to security cameras' remote access feature. However, cloud-based solutions are vendor-specific, requiring different setups and apps for different use cases; and it is as secure and the vendors' security posture, which is usually not very high. The P2P communication may not be encrypted end-to-end using proper algorithm, making it possible for attackers on your public WiFi hotspot, on the internet, or at the compromised vendor cloud, to eavesdrop your access. In addition, a trip to the cloud is almost not the shortest path, especially you are not far away from home.

Remote-access VPN comes into play. We are all familiar with it; when we work from home, we use it all day long to connect to the corporate network. A remote-access VPN gives employees access to secure connection from anywhere on the internet to a remote private network and they can access resources on the private network as if they were directly plugged into it. Remote-access VPN establishes virtual tunnels between a client and a server. The laptop your employer provides already have remove-access VPN configured: it could be part of the operating system, or dedicated application like Cisco AnyConnect. They are the VPN client. A network access server is either the dedicated server or applications running on or behind your internet gateway router that VPN tunnels are established to. The client-server architecture allows a variety of protocols, either standard/open-source or proprietary, to provide the same functionality.

Given that I have an EdgeRouter and I do not want to run dedicated VPN access server or applications for some rare use cases, I went for what EdgeOS supports. EdgeOS supports [PPTP](https://help.ui.com/hc/en-us/articles/205220840-EdgeRouter-PPTP-VPN-Server), [L2TP](https://help.ui.com/hc/en-us/articles/204950294-EdgeRouter-L2TP-IPsec-VPN-Server), and [OpenVPN](https://help.ui.com/hc/en-us/articles/115015971688-EdgeRouter-OpenVPN-Server) servers. So first, we need to decide on a protocol.

# Protocol Comparisons

PPTP stands for point-to-point tunneling protocol, and it has been in common operating systems for a long time (since Windows 95 for example). PPTP has known vulnerabilities.

L2TP stands for Layer 2 Tunnel Protocol. It is a VPN protocol that does not offer any encryptions, so it is usually implemented with IPSec encryption. The combo is usually referred as I2TP/IPSec VPN. IPSec is secure, however L2TP adds overhead and complexity. First, the packet is encapsulated by adding PPP, L2TP, and UDP headers (L2TP runs on UDP port 500), then encrypted in IPSec and ESP header added, and the outer IP header is attached at last.

OpenVPN uses the OpenSSL encryption library extensively, as well as the TLS protocol, and contains many security and control features. It uses a custom security protocol that utilizes SSL/TLS for key exchange. It is capable of traversing network address translators (NATs) and firewalls. Being relatively new, OpenVPN is usually not built into operating systems. It can run in the userspace so it can be installed as an app in both desktop and mobile operating systems, increasing its versatility. It supports pre-shared keys, username/password, and certificates.

Therefore, OpenVPN is the best option.

# Server Configuration in EdgeOS
[This Ubiquiti Support Page](https://help.ui.com/hc/en-us/articles/115015971688-EdgeRouter-OpenVPN-Server) details the steps; however, I want to document a few things:

1. The Step 3, generating a Diffie-Hellman (DH) key file, can take a long time. It took more than an hour for me.
2. In Step 19, the OpenVPN server interface is created. The server interface name is `vtun0`. The server and client subnet is `172.16.1.0/24` but it can be set to anything that suits your network. `set interfaces openvpn vtun0 server push-route 192.168.1.0/24` will install `192.168.1.0/24` on the client, thus making this network available over the OpenVPN tunnel. You should specify the correct subnet range you want to access remotely. `set interfaces openvpn vtun0 server name-server 192.168.1.1` sets the IP address of the DNS server over the VPN tunnel, so you should specify your DNS server (I hope you use [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})!).
3. To support more clients, just repeat Step 11 and 12 to create for client certs.

# Client Configuration
Client requires the certificates and server information, in the form of address/domain name and port (1194). With [DDNS]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-06-06-secure-home-network-access-your-home-assistant-from-internet.md %}#ddns-using-edgerouter), we can specify a constant domain name without worrying about changing dynamic IPs assigned by the ISP.
