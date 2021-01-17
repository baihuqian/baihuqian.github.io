---
layout: post
title: 'Secure Home Network: Configure IPv6 for Your Home Network'
date: '2021-01-16 15:06'
tags:
 - HomeNetwork
---

IPv6 has been around for a while, but surprisingly, not every ISP supports it. If your ISP supports it, you can enable it on your router. Given not all devices or everyone in the internet supports IPv6 yet (or never will), we will run dual-stack network that supports both IPv4 and IPv6 simultaneously.

IPv6 differs from IPv4 when creating a home network in the following ways:
1. There is no Network Address Translation (NAT) required. Instead of having a RFC1918 private space for your home network and your internet gateway performs NAT to translate to your assigned public IPv4 address, the ISP can assign a subnet, which is 64 bit or larger, for your home use. Thus, your home network is uniquely identified and routable on the Internet. The assignment process is called Prefix Delegation.
2. Given the subnet space is so large, it is uncommon to use stateful address allocation, such as DHCP. Instead, stateless address autoconfiguration (SLAAC) is widely used to allow IPv6 hosts to configure themselves automatically.
3. Routers provide network prefixes to devices via router advertisements.

Because the IPv6 home network is Internet-addressable, it is important to set up firewall rules before obtaining addresses. I use Ubiquiti EdgeRouter and its GUI does not support configuring IPv6, so CLI (or Config Tree) is used. I first create the `WAN_IN` firewall rules on my WAN interface (`eth0`):

```
# Configure Firewall
set firewall ipv6-name IPV6WAN_IN description 'IPV6WAN to internal'
set firewall ipv6-name IPV6WAN_IN default-action drop

set firewall ipv6-name IPV6WAN_IN rule 10 action accept
set firewall ipv6-name IPV6WAN_IN rule 10 state established enable
set firewall ipv6-name IPV6WAN_IN rule 10 state related enable
set firewall ipv6-name IPV6WAN_IN rule 10 log disable
set firewall ipv6-name IPV6WAN_IN rule 10 description 'Allow established/related'

set firewall ipv6-name IPV6WAN_IN rule 20 action drop
set firewall ipv6-name IPV6WAN_IN rule 20 state invalid enable
set firewall ipv6-name IPV6WAN_IN rule 20 description 'Drop invalid state'

set firewall ipv6-name IPV6WAN_IN rule 30 action accept
set firewall ipv6-name IPV6WAN_IN rule 30 description 'Allow ICMPv6'
set firewall ipv6-name IPV6WAN_IN rule 30 log disable
set firewall ipv6-name IPV6WAN_IN rule 30 protocol icmpv6


set interfaces ethernet eth0 firewall in ipv6-name IPV6WAN_IN

commit
save
```

Then it is time to configure DHCPv6 Prefix Delegation and allocate it to our home networks. Since I have multiple VLANs at home (and [I discussed why you should do so in this post]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %})), I can do so for every VLAN interface by assigning an unique `prefix-id` for them:

```
# Enable DHCPv6 Prefix Delegation on WAN interface
# Comcast provides subnet with prefix length of 60
set interfaces ethernet eth0 dhcpv6-pd pd 0
set interfaces ethernet eth0 dhcpv6-pd pd 0 prefix-length 60
set interfaces ethernet eth0 dhcpv6-pd rapid-commit enable

# Allocate the delegated space to VLAN 100 with a 4-bit prefix of value 4
set interfaces ethernet eth0 dhcpv6-pd pd 0 interface switch0.100
set interfaces ethernet eth0 dhcpv6-pd pd 0 interface switch0.100 host-address ::1
set interfaces ethernet eth0 dhcpv6-pd pd 0 interface switch0.100 prefix-id :4
set interfaces ethernet eth0 dhcpv6-pd pd 0 interface switch0.100 service slaac

# Advertise the 64-bit subnet into VLAN 100
set interfaces switch switch0 vif 100 ipv6 router-advert prefix ::/64

commit
save
```

Then in `show interfaces`, you should see a `/128` address is allocated to your WAN interface, and your LAN interface (in this case, `switch0.100`) should have a `/64` address, and `show ipv6 route` should show `/64` prefixes to each of the LAN interfaces, and `::/0` to your WAN interface. All your IPv6 enabled devices should pick up their IPv6 addresses, if IPv6 configuration is set to automatic, and full IPv6 connectivity to the internet is established. You can test it with [test-ipv6.com](https://test-ipv6.com).

However, the main question is: do you need IPv6 in your home network? The answer is largely, no.

Only a small fraction of your devices fully support IPv6 or is able to operate in an IPv6-only network. Many devices use network discovery protocols such as UPnP or mDNS that rely on multicast or broadcast in a local network. Given multicast is different in IPv6 and broadcast simply does not exist, and there is no more local or private network with IPv6, I fully expect many devices simply do not work in IPv6-only networks. Vendors are unlikely to provide firmware updates to legacy hardware to make it work with IPv6 and will ask you to buy new ones instead, making them trapped in the IPv4 era.

In addition, without proper firewall in place, having internet-addressable home network can potentially expose security vulnerability that are currently local within your router to the Internet, and your home network would be affected by Internet events if your ISP fails to stop it from reaching your router.

If you use [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}) DNS server, the default configuration does not block tracking on IPv6 network. Additional configuration is required to block ads and tracking if you have IPv6 enabled.

IPv4 works fine for most home networks. Home routers can NAT at higher throughput than most people's internet bandwidth so you will not see a big performance improvement in IPv6. So if you don't host things on the Internet (like web servers and such), then you don't need IPv6 in your home.

So the main benefit to configure IPv6 in my network (and remove it later on) for me is the opportunity to try something new and learn what the industry has learned from IPv6 and thus what design improvements are put into the IPv6 protocol suite.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).

# References
* [1](https://techsmix.net/ubiquti-edgemax-lite/)
* [2](https://gist.github.com/mskutta/b203b73134364a78d2e3)
* [3](https://community.ui.com/questions/IPv6-dhcpv6-pd-and-VLANs/56e663fb-1ee1-431e-90da-69c86e72e388)
