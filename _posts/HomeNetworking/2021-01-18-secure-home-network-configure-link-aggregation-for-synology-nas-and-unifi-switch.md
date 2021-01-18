---
layout: post
title: >-
  Secure Home Network: Configure Link Aggregation for Synology NAS and UniFi
  Switch
date: '2021-01-18 14:49'
tags:
  - HomeNetwork
  - NAS
---

My Synology NAS (DS920+) has two Gigabit ethernet ports, and I have enough ports on my switch (UniFi Switch 8), I have configured link aggregation to fully utilize the bandwidth of the NAS device and get automatic failover in case either port fails.

Synology NAS supports many different link aggregation methods, but the switch only supports dynamic link aggregation via Link Aggregation Control Protocol (LACP), a dynamically negotiated protocol that ensures that the LAG configuration is compatible and viable on each endpoint. The order of operation is important. To allow provisioning via network, it is important to configure downstream devices first before configuring the upstream devices.

Thus, I first create a "bond" in DSM via Control Panel -> Network -> Network Interface. Select IEEE 802.3ad Dynamic Link Aggregation as the mode, and configure a static IP for the bond interface. DHCP should work too but I always configure a static IP for my NAS.

Then, go to the switch device page in the UniFi controller, and select the lowest port (in terms of port number). Change its operation to "Aggregate" in the "Profile Overrides" section, and specify the next port as the aggregate ports. Note UniFi only allows aggregation among consecutive ports. In my case, I create a LAG for Port 1 and 2, so I change Port 1 to be "Aggregate" and aggregate ports is 2. Once it is provisioned, you will not be able to edit port profile for Port 2.

Once the link aggregation is established, the bond interface will have the network status as "2000 Mbps, Full duplex, MTU 1500".

Note, link aggregation does not increase the bandwidth for each user/client beyond 110 MB/s you would normally get over Gigabit ethernet. It will allow multiple concurrent users to achieve an aggregate higher bandwidth by utilizing both ethernet links simultaneously.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
