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
1. Make sure that the date/time is set correctly on the EdgeRouter.
```
show date
Mon Jan 21 12:13:07 UTC 2019
```
2. Change to root user: `sudo su`.
3. Generate a Diffie-Hellman (DH) key file and place it in the `/config/auth` directory.
```
openssl dhparam -out /config/auth/dh.pem -2 2048
```
Generating a Diffie-Hellman (DH) key file can take a long time. It took more than an hour for me.
4. Go to `/usr/lib/ssl/misc`. Certificate and key generation should be run from this directory.
4. Generate a root certificate using helper script:
```
./CA.pl -newca
```
Make sure you remember the passphrase.
5. Copy the newly created certificate + key to the `/config/auth` directory.
```
cp demoCA/cacert.pem /config/auth
cp demoCA/private/cakey.pem /config/auth
```
Keep the root certificate in `/usr/lib/ssl/misc` for later use.
6. Generate, sign, and move the server certificate.
```
./CA.pl -newreq
./CA.pl -sign
mv newcert.pem /config/auth/server.pem
mv newkey.pem /config/auth/server.key
```
7. Remove password from server key file.
```
openssl rsa -in /config/auth/server.key -out /config/auth/server-no-pass.key
mv /config/auth/server-no-pass.key /config/auth/server.key
```
If this step is skipped, the clients will need to enter the password when connecting.
8. Generate certificates for clients.
```
./CA.pl -newreq
./CA.pl -sign
mv newcert.pem /config/auth/client.pem
mv newkey.pem /config/auth/client.key
openssl rsa -in /config/auth/client.key -out /config/auth/client-no-pass.key
mv /config/auth/client-no-pass.key /config/auth/client.key
chmod 644 /config/auth/client.key
```
Repeat for all your clients.
9. Once done, verify the contents of the `/config/auth` directory. It should include DH key file, root certificate, server certificate, and all client certificates and keys. Exit root user.
10. Now, add the router's open VPN configuration.
```
configure
```
11. Add a firewall rule to allow the OpenVPN traffic to the `WAN_LOCAL` firewall policy.
```
set firewall name WAN_LOCAL rule 30 action accept
set firewall name WAN_LOCAL rule 30 description openvpn
set firewall name WAN_LOCAL rule 30 destination port 1194
set firewall name WAN_LOCAL rule 30 protocol udp
```
12. Configure the OpenVPN virtual tunnel interface `vtun0`.
```
set interfaces openvpn vtun0 mode server
```
Create a server and client subnet using `172.16.1.0/24` but it can be set to anything that suits your network.
```
set interfaces openvpn vtun0 server subnet 172.16.1.0/24
```
Then install a route `192.168.1.0/24` on the client, thus making this network available over the OpenVPN tunnel. You should specify the correct subnet range you want to access remotely.
```
set interfaces openvpn vtun0 server push-route 192.168.1.0/24
```
Next, set up the DNS server over the VPN tunnel. I hope you use [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})!
```
set interfaces openvpn vtun0 server name-server 192.168.1.1
```
13. Link the server certificate/keys and DH key to the virtual tunnel interface.
```
set interfaces openvpn vtun0 tls ca-cert-file /config/auth/cacert.pem
set interfaces openvpn vtun0 tls cert-file /config/auth/server.pem
set interfaces openvpn vtun0 tls key-file /config/auth/server.key
set interfaces openvpn vtun0 tls dh-file /config/auth/dh.pem
```
14. Commit the changes and save the configuration.
```
commit ; save
```

# Client Configuration
Client requires the certificates and server information, in the form of address/domain name and port (1194). The certificates can be transferred from EdgeRouter:
```
scp username@<ip-address>:/config/auth/cacert.pem ~/Desktop/config
scp username@<ip-address>:/config/auth/client.pem ~/Desktop/config
scp username@<ip-address>:/config/auth/client1.key ~/Desktop/config
```

With [DDNS]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-06-06-secure-home-network-access-your-home-assistant-from-internet.md %}#ddns-using-edgerouter), we can specify a constant domain name without worrying about changing dynamic IPs assigned by the ISP.

Add the following information to the `er.ovpn` configuration file:
```
client
dev tun
proto udp
remote <server> 1194
float
resolv-retry infinite
nobind
persist-key
persist-tun
verb 3
ca cacert.pem
cert client.pem
key client.key
```
To send all traffic through the VPN connection, append the `er.ovpn` configuration file with the following line.
```
redirect-gateway def1
```

## iOS
The OpenVPN protocol is not one that is built into the Apple iOS operating system, thus a client app is required. [OpenVPN Connect](https://apps.apple.com/us/app/openvpn-connect/id590379981) is the preferred client app. To load the client configuration, or "profile" into the app, we need to combine certs and keys with configuration into one file and load the file into OpenVPN Connect.

First, comment out the lines in the `ovpn` file specifying the relative paths to the server, client certs and keys:

```
#ca ca.pem
#cert server.pem
#key server.key
```

Then, add the content of these files to the end of the `ovpn` file:

```
<ca>
[--- CONTENTS OF ca.crt GOES HERE ---]
</ca>
<cert>
[--- CONTENTS OF server.crt GOES HERE ---]
</cert>
<key>
[--- CONTENTS OF server.key GOES HERE ---]
</key>
```

Then, upload this file to iCloud drive and download it using the `Files` app, or AirDrop to your phone. Share the file with OpenVPN. It should prompt you to import the profile. Try not to email it to yourself, that’s less secure.

# References
1. [This Ubiquiti Support Page](https://help.ui.com/hc/en-us/articles/115015971688-EdgeRouter-OpenVPN-Server)
2. [How to import an OpenVPN profile on iOS (without iTunes)](https://www.gaelanlloyd.com/blog/how-to-import-an-openvpn-profile-on-ios-without-itunes/)
