---
layout: "post"
title: "Secure Home Network - Block Ad and Tracking with Pi-Hole"
date: "2019-09-14 23:08"
tags:
 - Networking
---

* toc
{:toc}

Target and personalized ads are everywhere and have become the way companies generate revenue and profit from users. Ad-embedded websites use the same ad services to ensure users see the same set of ads across multiple sites and devices. With more and more connected devices and longer and longer use time, privacy becomes a significant concern as ad services are knowing end users better and better. In addition, ads become more intrusive in the user experience of Internet life, because more and more "free" services now rely on ads to generate revenue.

Displaying appropriate ads is done via "unwanted" (from user's perspective) requests from your devices to ads services. While blocking such requests is difficult, we can block the prerequisite of such requests, the DNS lookup.

> What is DNS?
> The [Domain Name Systems](https://www.cloudflare.com/learning/dns/what-is-dns/) (DNS) is the phonebook of the Internet. Humans access information online through domain names, like `nytimes.com` or `espn.com`. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources.

Because ad services' domain names are publicly known and rarely changed (compared to the IP addresses), we can block requests to ad services by responding with an invalid answer to those DNS requests. This technology is called DNS sinkhole.

> A [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_Sinkhole), also known as a sinkhole server, Internet sinkhole, or Blackhole DNS is a DNS server that gives out false information, to prevent the use of a domain name.

In addition to ad services, your Internet Service Provider (ISP) knows your entire browsing history and it is selling your data to third-parties like ad services to make a profit on top of fees to provide the service. This is by logging your DNS requests.

When you set up your home network, you are likely using your ISP's DNS service. Even if you configure your router to use some third-party's DNS server, because DNS requests and responses are plaintext, you cannot stop the ISP to hijack your DNS requests through Deep Packet Inspection (DPI). This hijacking technique is called [man-in-the-middle attacks](https://en.wikipedia.org/wiki/Man-in-the-middle_attack).

> [Deep packet inspection](https://en.wikipedia.org/wiki/Deep_packet_inspection) (DPI) is a type of data processing that inspects in detail the data being sent over a computer network, and usually takes action by blocking, re-routing, or logging it accordingly. Deep packet inspection is often used to ensure that data is in the correct format, to check for malicious code, eavesdropping and internet censorship among other purposes.

Your ISP performs DPI to detect malicious or illegal usage of Internet and its service. Therefore, you cannot detect or do anything against snooping or hijacking your DNS requests to the configured DNS server by your ISP. To avoid this, we can use DNS-over-HTTPS.

> [DNS-over-HTTPS](https://en.wikipedia.org/wiki/DNS_over_HTTPS) is a protocol for performing DNS lookups via HTTPS. With standard DNS, requests are sent in plain-text, with no method to detect tampering or misbehavior. This means that not only can a malicious actor look at all the DNS requests you are making (and therefore what websites you are visiting), they can also tamper with the response and redirect your device to resources in their control (such as a fake login page for internet banking). DNS-Over-HTTPS prevents this by using standard HTTPS requests to retrieve DNS information. This means that the connection from the device to the DNS server is secure and can not easily be snooped, monitored, tampered with or blocked. It is worth noting however, that the upstream DNS-Over-HTTPS provider will still have this ability.

However, DNS-Over-HTTPS is not the standard protocol and is not supported by all devices. Therefore, we want to set it up centrally in our network.

This post will go through the setup of [Pi-Hole](https://github.com/pi-hole/pi-hole), a DNS sinkhole that protects your devices from unwanted content, and [configuring DNS-Over-HTTPS on Pi-hole](https://docs.pi-hole.net/guides/dns-over-https/) to avoid DNS snooping and hijacking.

# Install and Configure Pi-Hole
You should install Pi-Hole on a long-running device in your network. It is not recommended to install it on your router as it is not supposed to have custom software installed on it. You can install it on a [Raspberry Pi](https://www.raspberrypi.org/), or a computer if you have spares around. In my case I have a spare NUC and I install Ubuntu Server on it and run it in the server closet with my routers.

Installing Pi-Hole is a simple one-liner:

```bash
curl -sSL https://install.pi-hole.net | bash
```

The installer will walk you through the process. A few things to note:
1. A static IP is needed for Pi-Hole to function properly. PiHole has DHCP capabilities, which means it can act as your DHCP server and assign IP addresses to your clients. However, most routers can do this well as well. Therefore, we should assign a static/fixed IP to the MAC address of the interface of the device running with Pi-Hole. In my case, I assigned `192.168.10.2/24` to it, followed the convention of AWS VPC where the `.2` address in a VPC is the DNS server.
2. If your device has more than one interface (true in most cases), select the interface connected to the router. You should connect to the router via ethernet cable for stable performance.
3. Pi-Hole puts itself between your DNS server and clients. It asks you for the upstream DNS server so it can forward requests to it. You should not use any DNS server that can potentially tracks you. Cloudflare introduced their own privacy focussed DNS server. Unlike other DNS services that usually sell your DNS lookups data to ad services, Cloudflare maintains no logs beyond 24h and does not sell your data. Therefore, I recommend to use it.
4. The final installer screen should show your password to use for the web interface. Note it down and keep it in a safe place, like password manager.
5. Leave the rest of the configuration as default.

After installation, you should configure your devices and network to use Pi-Hole. Enter the IP address of Pi-Hole (in my case, `192.168.10.2`) in your router's DHCP settings as the primary DNS, and remove secondary DNS if exists. If you have any devices configured to use manually-selected DNS servers, update them to use Pi-Hole as well.

Now most of ads are not displayed. It is a great start. However, Pi-Hole Youtube Ad blocking is a hit or a miss as the ad servers can change constantly. But this has not stopped some users from creating and maintaining Youtube ad blocklist for Pi-Hole.

# Configure DNS-over-HTTPS to Upstream Server
In the previous section, we configured DNS requests by Pi-Hole to Cloudflare. However, these requests are made in plaintext and can be inspected by anyone in between your Pi-Hole and Cloudflare's servers. We will configure Pi-Hole to use DNS-over-HTTPS to stop this. This instruction is modified from [Pi-Hole's website](https://docs.pi-hole.net/guides/dns-over-https/).

Along with releasing their DNS service `1.1.1.1`, Cloudflare implemented DNS-Over-HTTPS proxy functionality in to one of their tools: `cloudflared`.

## Install `cloudflared`
Download the installer package, then use `apt-get` to install the package along with any dependencies. Proceed to run the binary with the `-v` flag to check it is all working.

```bash
wget https://bin.equinox.io/c/VdrWdbjqyF/cloudflared-stable-linux-amd64.deb
sudo apt-get install ./cloudflared-stable-linux-amd64.deb
cloudflared -v
```

## Configuring `cloudflared` to run on startup

Create a `cloudflared` user to run the daemon.

```bash
sudo useradd -s /usr/sbin/nologin -r -M cloudflared
```

Proceed to create a configuration file for `cloudflared` by copying the following in to `/etc/default/cloudflared`. This file contains the command-line options that get passed to `cloudflared` on startup. The command-line options basically sets up a DNS proxy that exposes port `5053` locally and connects to Cloudflare's primary DNS server `1.1.1.1` and secondary `1.0.0.1` via HTTPS:

```bash
# Commandline args for cloudflared
CLOUDFLARED_OPTS=--port 5053 --upstream https://1.1.1.1/dns-query --upstream https://1.0.0.1/dns-query
```

Update the permissions for the configuration file and `cloudflared` binary to allow access for the `cloudflared` user

```bash
sudo chown cloudflared:cloudflared /etc/default/cloudflared
sudo chown cloudflared:cloudflared /usr/local/bin/cloudflared
```

Then create the `systemd` script by copying the following in to `/lib/systemd/system/cloudflared.service`. This will control the running of the service and allow it to run on startup.

```
[Unit]
Description=cloudflared DNS over HTTPS proxy
After=syslog.target network-online.target

[Service]
Type=simple
User=cloudflared
EnvironmentFile=/etc/default/cloudflared
ExecStart=/usr/local/bin/cloudflared proxy-dns $CLOUDFLARED_OPTS
Restart=on-failure
RestartSec=10
KillMode=process

[Install]
WantedBy=multi-user.target
```

Enable the `systemd` service to run on startup, then start the service and check its status.

```bash
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
sudo systemctl status cloudflared
```

Now test that it is working! Run the following `dig` command, a response should be returned similar to the one below

```bash
dig @127.0.0.1 -p 5053 google.com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> @127.0.0.1 -p 5053 google.com
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65181
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1536
;; QUESTION SECTION:
;google.com.            IN  A

;; ANSWER SECTION:
google.com.     299 IN  A   243.65.127.221

;; Query time: 3 msec
;; SERVER: 127.0.0.1#5053(127.0.0.1)
;; MSG SIZE  rcvd: 65
```

## Configuring Pi-hole
Finally, configure Pi-hole to use the local `cloudflared` service as the upstream DNS server instead of the standard Cloudflare server.

Go to "Settings" in the Pi-Hole console, choose "DNS" tab, uncheck the checkboxes before "Cloudflare", and type in `127.0.0.1#5053` as the primary and `1.1.1.1#53` as the secondary for IPv4. Check the checkboxes before them. Don't forget to hit Return or click on Save.


# Further Reads
This is the post series. Other posts on the home network topics are:
1. [Device and Management Setup]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-07-secure-home-network-device-and-management-setup.md %})
1. [Isolating Connected Devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-02-secure-home-network-isolating-connected-devices-with-vlan.md %})
1. [Using HomeKit Devices Across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-08-27-secure-home-network-using-homekit-devices-across-vlans.md %})
1. [Using AirPlay Across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-08-31-secure-home-network-using-airplay-across-vlans.md %})
1. [Troubleshoot DHCP Problems]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-troubleshoot-dhcp-problems.md %})
1. [Extend WiFi Coverage with Multiple APs]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-extend-wifi-coverage-with-multiple-aps.md %})
1. [Backup Your Configurations]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-11-23-secure-home-network-backup-your-configurations.md %})
1. [IoT Automation with Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %})
