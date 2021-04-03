---
layout: post
title: 'Secure Home Network - Isolating Connected Devices with VLAN'
date: '2019-09-08 23:24'
tags:
 - HomeNetwork
 - IoT
---

Nowadays many devices connect to the Internet besides your computers, phones, and tablets. Those things are often referred as "smart devices" or "IoT". however, the security standard of IoT devices is quite poor and the devices are prone to hacking. You don't want hackers to get into your important devices such as laptops and NAS from a compromised smart light, do you? When friends and family come over, often they are going to ask you for your WiFi password the moment they sit down at your place. You don't want a compromised device to get into your home network, do you?

I hope the answer to both questions are "no". In this case you will set up VLANs and firewall rules to isolate devices in your network.

* TOC
{:toc}

# VLAN
Virtual LANs (VLANs) allow network administrators to subdivide a physical network into separate logical broadcast domains. On a standard Layer 2 network, all hosts connected to a switch are members of the same broadcast domain; and broadcast domains can only be physically separated across different switches by routers. A VLAN represents a broadcast domain. VLANs are identified by a VLAN ID (a number between 0 – 4095), with the default VLAN on any network being VLAN 1. Each port on a switch or router can be assigned to be a member of a VLAN (i.e., to allow receiving and sending traffic on that VLAN).

In order to implement VLANs, the routers and switches must support VLANs. The most commonly used protocol for configuring VLANs is IEEE 802.1Q. In order for 802.1Q compatible hardware to identify what VLAN a data packet belongs to, an 802.1Q Header is added to the Ethernet frame which specifies the VLAN ID. This VLAN ID tag may be added or removed by a host, a router, or a switch. Within the network, physical ports are configured as untagged or tagged for a specific VLAN—determining whether to accept and forward traffic belonging to each VLAN ID.

**Untagged**: a VLAN that is untagged is also sometimes referred to as the "Native VLAN".  Any traffic that is sent from a host to a switch port that **doesn't have a VLAN ID specified, will be assigned to the untagged VLAN**. This option is typically used when connecting hosts such as workstations or devices like IP cameras that don't tag their own traffic, and only need to communicate on one specific VLAN.  A port can only have one Untagged VLAN configured at a time.

**Tagged**: Assigning a tagged VLAN to a port adds that port to the VLAN, but all ingress and egress traffic **must be tagged with the VLAN ID in order to be forwarded**. The host connected to the switch port must be capable of tagging its own traffic, and be configured to do so with the same VLAN ID.

Tagged VLANs (as opposed to Untagged) on a port are typically used when connecting to a host that needs access to several networks at once using the same interface, such as a server providing services to more than one department in an office. It can also be used when connecting two switches, in order to restrict access to a VLAN to hosts connected to a downlink switch for security purposes.

# VLAN Design For Home Network

In a home network, a minimum of 3 VLANs are required:
1. VLAN 10 for the "secure" network where your phones, tablets, computers and NAS live;
2. VLAN 20 for the "Device" network where all your connected devices, including smart TV, speakers, lights, thermostats, etc. live;
3. VLAN 30 for the "Guest" network.

While VLAN is a Layer 2 protocol, we can set up separate DHCP servers for each VLAN. By convention, the third octet in the IP range matches the VLAN ID. For example, if we use 192.168.0.0/16 as the overall network range, VLAN 10 should have the range 192.168.10.0/24.

# Configure VLAN

I use Ubiquiti Edge Router as my home router, and I don't have any switches. The specific configuration for different vendors and switch setup may vary.

### Create VLAN off `switch0`
On the "Dashboard" page, the Edge Router has 5 eth ports and a switch, `switch0`. Click **Add Interface > Add VLAN**:

```
VLAN ID: 10
Interface: switch0
Address: Manually define IP address > 192.168.10.1/24

VLAN ID: 20
Interface: switch0
Address: Manually define IP address > 192.168.20.1/24

VLAN ID: 30
Interface: switch0
Address: Manually define IP address > 192.168.30.1/24
```

Note the IP address of the VLAN should be the 1st address in the subnet, using CIDR representation. The logical switch for VLAN 10 is named as `switch0.10`.

### Create DHCP Scopes
Go to **Services > DHCP Server > Add DHCP Server**:

```
DHCP Name: vlan10
Subnet: 192.168.10.0/24
Range Start: 192.168.10.11
Range Stop: 192.168.10.150
Router: 192.168.10.1
DNS 1: 192.168.10.1

DHCP Name: vlan20
Subnet: 192.168.20.0/24
Range Start: 192.168.20.11
Range Stop: 192.168.20.150
Router: 192.168.20.1
DNS 1: 192.168.20.1

DHCP Name: vlan30
Subnet: 192.168.30.0/24
Range Start: 192.168.30.11
Range Stop: 192.168.30.150
Router: 192.168.30.1
DNS 1: 192.168.30.1
```

You can choose the dynamically-allocated range. Note that the router and DNS 1 should be the VLAN address to avoid configuring firewalls to allow DNS traffic to go to a different subnet.

### Enable DNS Forwarding
If your router is your DNS server on the LAN, then you need to enable DNS forwarding. Go to **Services > DNS**, in **DNS Forwarding** section, click **Add Listen Interface**, and add `switch0.10` and `switch0.20`.

If you have a separate DNS server, such as [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}), then you need to add firewall rules for VLANs to reach the DNS server. My post on [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}/#firewall-rules) includes how to set up proper firewall rules.

### Tag/Untag Ports
Go to **Dashboard > switch0 > Actions > Config > VLAN**:
```
VLAN Aware: Enabled
eth3 pvid: 10
eth4 vid: 10,20,30
```
`pvid` is for untagged VLAN, and each port can have only one. `vid` is for tagged VLAN, and each port can have multiple. In our setup, `eth4` connects to the UniFi AP and eth3 connects to the NAS. Because the AP supports VLAN tagging while NAS doesn't, we should configure them accordingly.

### Configure Wireless Network
I use a UniFi AP for my wireless network and it supports VLAN. It is recommended to use separate SSID for different VLAN.

In the UniFi controller, go to **Settings > Wireless Networks**, and create or edit networks for each VLAN: under "advanced options", check "VLAN" and specify the corresponding VLAN ID.

For guest network, check "Guest Policy".

### Configure Switch Ports
VLANs can be used with UniFi Switches. By default, ports are set to `All`, so it'll have an untagged VLAN 1 (which should be the default network in your controller), and then the rest will be tagged. To enable tagged VLAN for a port, VLANs needs to be defined in the UniFi Network Controller under **Settings > Networks**. To simply set up a VLAN, set a network as *VLAN only*.

To change the profile on a port, or port group, click on the Switch in the Devices tab to reveal the **Properties Panel**, then go to **Ports**, and either choose the edit button on the right or select the desired ports and click "edit selected" on the bottom. In editing mode, choose the profile for the port(s). The Networks/VLANs profile on a port can be used to define the untagged and tagged networks on the selected port(s).

# Firewall

So far, you have set up VLANs and devices in these VLANs are able to obtain dynamic IP addresses and access Internet. Now, let's configure firewall rules to lock down access.

Firewall has three directions, IN, OUT, and LOCAL. However, it is quite confusing because it refers to the direction respective to the router.
* In firewall rules are processed for packets entering from a given interface.
* Out firewall rules are processed for packets exiting from a given interface.
* Local firewall rules are processed for packets destined for the router itself from a given interface.

In terms of using IN or OUT rules, some will say that IN is better because if you're going to drop a packet it's better to do it on input rather than go through the full packet processing path only to drop it before it leaves the router. Also note that creating a firewall ruleset without applying it to an interface/direction does *nothing*. Thus, we will create IN and LOCAL for WAN as well as each LAN.

The EdgeRouter uses a stateful firewall, which means the router firewall rules can match on different connection states. The traffic states are:

* `new` The incoming packets are from a new connection.
* `established` The incoming packets are associated with an already existing connection.
* `related` The incoming packets are new, but associated with an already existing connection.
* `invalid` The incoming packets do not match any of the other states.

All the configurations are under "Firewall/NAT" tab. We will configure a ruleset in the `IN` and `LOCAL` directions for each VLAN.

### WAN_IN
`WAN_IN` matches packets from the internet, through the router, and onward to your LAN. Unless you are running a service, e.g. a web server, that accepts incoming connections from the Internet, you want to drop pretty much everything except established and related packets.

First, create a `WAN_IN` firewall policy and set the default action to `drop`. **Firewall/NAT > Firewall Policies > + Add Ruleset**
```
Name: WAN_IN
Description: WAN to internal
Default action: Drop
```

Then, add two firewall rules to `WAN_IN`. **Firewall/NAT > Firewall Policies > WAN_IN > Actions > Edit Ruleset > + Add New Rule**

```
Description: Allow established/related
Action: Accept
Protocol: All protocols
Advanced > State: Established / Related

Description: Drop invalid state
Action: Drop
Protocol: All protocols
Advanced > State: Invalid
```

Last, attach the firewall policy to the WAN interface (eth0) in the inbound direction. **Firewall/NAT > Firewall Policies > WAN_IN > Actions > Interfaces**

```
Interface: eth0
Direction: in
```

### WAN_LOCAL
`WAN_IN` matches packets from the internet and are destined to the router itself. It should drop everything except established and related packets.

First, create a `WAN_LOCAL` firewall policy and set the default action to `drop`. **Firewall/NAT > Firewall Policies > + Add Ruleset**
```
Name: WAN_LOCAL
Description: WAN to router
Default action: Drop
```

Then add two firewall rules to `WAN_LOCAL`. **Firewall/NAT > Firewall Policies > WAN_LOCAL > Actions > Edit Ruleset > + Add New Rule**

```
Description: Allow established/related
Action: Accept
Protocol: All protocols
Advanced > State: Established / Related

Description: Drop invalid state
Action: Drop
Protocol: All protocols
Advanced > State: Invalid
```

Last, attach the firewall policy to the WAN interface (eth0) in the local direction. **Firewall/NAT > Firewall Policies > WAN_LOCAL > Actions > Interfaces**

```
Interface: eth0
Direction: local
```

### GUEST_IN
`GUEST_IN` matches packets from the guest VLAN, through the router, and onward to other LAN or Internet. The guest network is allowed to access the internet but denied to other LAN. To be strict, we will add a drop rule for packets with destination within the RFC1918 range.

First, create a network group that includes all of the RFC1918 private IP ranges. **Firewall/NAT > Firewall/NAT Groups > + Add Group**

```
Name: LAN_NETWORKS
Description: RFC1918 ranges
Group Type: Network Group
```

And add the IP ranges to the newly created network group. **Firewall/NAT > Firewall/NAT Groups > LAN_NETWORKS > Actions > Config**

```
Network: 192.168.0.0/16
Network: 172.16.0.0/12
Network: 10.0.0.0/8
```

Then create a `GUEST_IN` firewall policy and set the default action to `accept`. **Firewall/NAT > Firewall Policies > + Add Ruleset**

```
Name: GUEST_IN
Description: guest to lan/wan
Default action: Accept
```

Then, add a firewall rule to the newly created firewall policy to drop traffic from guest VLAN to other LAN networks. **Firewall/NAT > Firewall Policies > GUEST_IN > Actions > Edit Ruleset > + Add New Rule**

```
Description: drop guest to lan
Action: Drop
Protocol: All protocols
Destination > Network Group : LAN_NETWORKS
```
You may want to enable logging so you can inspect packets hitting this `drop` rule.

Last, attach the firewall policy to the `switch0.30` LAN interface in the inbound direction. **Firewall/NAT > Firewall Policies > GUEST_IN > Actions > Interfaces**

```
Interface: switch0.30
Direction: in
```

### GUEST_LOCAL
`WAN_IN` matches packets from the guest VLAN and are destined to the router itself. We should only allow DHCP (UDP port 67) and DNS (TCP/UDP port 53).

First create a `GUEST_LOCAL` firewall policy and set the default action to `drop`. **Firewall/NAT > Firewall Policies > + Add Ruleset**

```
Name: GUEST_LOCAL
Description: guest to router
Default action: Drop
```

Then, add two firewall rules to the newly created firewall policy. **Firewall/NAT > Firewall Policies > GUEST_IN > Actions > Edit Ruleset > + Add New Rule**

```
Description: allow DNS
Action: Allow
Protocol: Both TCP and UDP
Destination > Port : 53

Description: allow DHCP
Action: Allow
Protocol: UDP
Destination > Port : 67
```

Last, attach the firewall policy to the `switch0.30` LAN interface in the local direction. **Firewall/NAT > Firewall Policies > GUEST_IN > Actions > Interfaces**

```
Interface: switch0.30
Direction: local
```

### DEVICE_IN
`DEVICE_IN` is mostly similar to `GUEST_IN` with additional `Accept` rules for specific use cases. For example, AirPlay requires certain traffic from the Device LAN to the Secure LAN, as discussed in [using AirPlay across VLANs]({{ site.baseurl }}{% post_url HomeNetworking/2019-09-09-secure-home-network-using-airplay-across-vlans %}). Such exceptions must be carefully evaluated and only applied as necessary. I discuss extra rules for `DEVICE_IN` in posts for each use case.

### DEVICE_LOCAL
Same as `GUEST_LOCAL`, `DEVICE_LOCAL` should at least allow DHCP and DNS (if router is the DNS server). But if have other use cases, such as connecting smart TVs in Device LAN from Secure LAN, you should allow Bonjour/mDNS traffic, too:

```
Description: allow Bonjour/mDNS
Action: Allow
Protocol: UDP
Destination > Address 224.0.0.251, Port : 5353
```

See [using AirPlay across VLANs]({{ site.baseurl }}{% post_url HomeNetworking/2019-09-09-secure-home-network-using-airplay-across-vlans %}) on details.

# References
* [Configure EdgeRouter as a VLAN-Aware switch](https://help.ubnt.com/hc/en-us/articles/115012700967)
* [EdgeRouter - How to Create a Guest\LAN Firewall Rule](https://help.ubnt.com/hc/en-us/articles/218889067-EdgeRouter-How-to-Create-a-Guest-LAN-Firewall-Rule)
* [EdgeRouter - How to Create a WAN Firewall Rule](https://help.ubnt.com/hc/en-us/articles/204962154-EdgeRouter-How-to-Create-a-WAN-Firewall-Rule)
* [Layman's firewall explanation](https://community.ui.com/questions/Laymans-firewall-explanation/2dafa379-3269-4749-b224-0dee15374de9)
* [Introduction to Virtual LANs (VLANs) and Tagging](https://help.ubnt.com/hc/en-us/articles/222183968-Intro-to-Networking-Introduction-to-Virtual-LANs-VLANs-and-Tagging)
* [Using VLANs with UniFi Routing Hardware](https://help.ubnt.com/hc/en-us/articles/219654087-UniFi-Using-VLANs-with-UniFi-Wireless-Routing-Switching-Hardware)
* [Using VLANs with UniFi Switching](https://help.ui.com/hc/en-us/articles/360046144234-UniFi-Using-VLANs-with-UniFi-Switching)

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
