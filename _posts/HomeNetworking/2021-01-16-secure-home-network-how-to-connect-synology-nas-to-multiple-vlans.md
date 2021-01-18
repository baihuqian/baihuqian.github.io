---
layout: post
title: 'Secure Home Network: How to Connect Synology NAS to Multiple VLANs'
date: '2021-01-16 20:49'
tags:
 - HomeNetwork
 - NAS
---

In a [previous post]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-01-secure-home-network-build-your-own-private-cloud-with-nas.md %}), I discussed my setup of a Synology NAS. I have [multiple VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-08-secure-home-network-isolating-connected-devices-with-vlan.md %}) at home, and I find a need to connect my Synology NAS to some of them. My NAS device has two network interfaces and can be connected to different VLANs on each interface. However, I have configured dynamic link aggregation which offers higher aggregate throughput across multiple users and automatic failover, and I do not want to be limited to only two VLANs that I can connect to.

Synology DSM supports VLAN, so I can configure my switch ports to which NAS is connected to a trunk port (i.e., support multiple VLANs if the connected device can explicitly tag packets with VLANs) instead of being a VLAN-aware port (i.e., the switch adds VLAN tags to packets received from the interface so the connected device does not have to know about VLAN). But make sure you can access your Synology web interface on the default VLAN.

Configure the "bond" interface (link aggregation) to have VLAN enabled with the first VLAN. In my case, it is VLAN 10. Click "Apply" and the NAS is now connected to my VLAN 10. Now, we need to duplicate config files via SSH as this is not supported by Synology DSM's web interface.

Go to `/etc/sysconfig/network-scripts` as root. Along with `ifcfg-bond0`, `ifcfg-eth0` and `ifcfg-eth1`, You should find an `ifcfg-bond0.<VLAN>` file there, such as `ifcfg-bond0.10` for VLAN 10. Then copy this file by replacing the `<VLAN>` section with your next VLAN, such as VLAN 20: `cp ifcfg-bond0.10 ifcfg-bond0.20`.

Now you should edit `ifcfg-bond0.20` with `vi` to correctly configure it. You should rename `DEVICE` to `bond0.<VLAN>`, set the correct `VLAN_ID`, and correct interface address and network mask. After edit, the file content should look like this:

```
DEVICE=bond0.20
VLAN_ROW_DEVICE=bond0
VLAN_ID=20
ONBOOT=yes
BOOTPROTO=static
IPADDR=<IP for VLAN 20>
NETMASK=<mask for VLAN 20>
```

Save and edit `vi`.

Then you can either reboot the NAS or restart its network stack with `/etc/rc.network restart`. Then if you refresh your DSM web interface, you should see two `Bond 1` interfaces with different IP addresses and VLAN IDs! Your NAS is now accessible by both addresses in two VLANs.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
