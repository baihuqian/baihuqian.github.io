---
layout: "post"
title: "Secure Home Network - Troubleshoot DHCP Problems"
date: "2020-01-11 20:19"
tags:
 - HomeNetwork
---

Recently, after an EdgeOS upgrade, I started to see intermittent DHCP timeouts from my UniFi controller, mostly from new clients. Then after flushing the DHCP lease, all devices are unable to obtain an IP address, indicating a problem with the DHCP server.

The EdgeOS GUI does not provide a status indicator about DHCP server, so I have to use the CLI. First, I tried to start the DHCP service by using this command.
```
sudo service dhcpd start
```

But it errors out. To continue, I looked in the following log files:

```
cat /var/log/messages
cat /var/log/vyatta/vyatta-commit.log
```

I found the following log lines:

```
Jan 10 20:15:19 Chicago dhcpd3: WARNING: Host declarations are global.  They are not limited to the scope you declared them in.
Jan 10 20:15:19 Chicago systemd[1]: vyatta-dhcpd.service: Control process exited, code=exited status=1
Jan 10 20:15:19 Chicago dhcpd3: /opt/vyatta/etc/dhcpd.conf line 58: host Samsung: already exists
Jan 10 20:15:19 Chicago systemd[1]: Failed to start EdgeOS DHCP Server.
```

So the problem is that I have multiple VLANs and separate DHCP server for each of them. My Samsung device has static mappings in multiple DHCP servers. It breaks the DHCP configuration after an EdgeOS upgrade. I removed the offending DHCP static mappings using the EdgeOS GUI and started DHCP server:

```
sudo service dhcpd start
```

And clients can now obtain IP addresses in my network.


# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
