---
layout: post
title: 'Secure Home Network: Create Your Own Syslog Server on Synology NAS'
date: '2021-05-27 20:47'
tags:
  - HomeNetwork
  - NAS
---

Syslog stands for System Logging Protocol and is a standard protocol used to send system log or event messages to a specific server, called a syslog server. It is primarily used to collect various device logs from several different machines in a central location for monitoring and review. It was widely adopted by Linux and can be commonly found in network devices.

My Ubiquiti devices all have syslog capability that are very easy to configure in the GUI. However, why do you care about their logs? Mostly to record unauthorized and illegal access to my wireless environment (APs) and violation of firewall rules. EdgeOS can record packets that hit certain firewall rules but it was not easy to examine them to find out what certain IoT devices are doing. For example, most IoT devices periodically send telemetry data home so the manufacturers can understand how customers use them more. However, some fearmongers with little network knowledge spreads the false impression on the internet that these devices are sending surveillance data to foreign government, especially many devices are manufactured by Chinese companies.

By logging them to a central log server that has more powerful analytics tools, it is easy to understand the traffic pattern and decide whether to block them. For example, I realized [my Amcrest cameras]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-03-19-secure-home-network-add-security-camera.md %}) periodically send data to an IP in AWS us-east-1 region, so it is unlikely that the data go to foreign governments. In addition, it is useful to analyze traffic patterns to reduce the pinhole firewall rules to make IoT devices work across VLANs.

When operating over network, syslog works in a client-server architecture using UDP port 514. There are many open-source and commercial implementations of syslog server. But I will use Synology's Log Center.

# Configure Synology Log Center
[Synology Log Center](https://www.synology.com/en-global/knowledgebase/DSM/help/DSM/LogCenter/logcenter_desc) is a centralized log management application. The DSM includes the basic version for its own logs; but to get the full functionality, we should install the **Log Center** package in **Package Center**.

First, we should create a shared folder in control panel for log archive. Then, under "Archive Settings" in Log Center, select the created shared folder as the log archive destination. Then, to enable syslog server, head to "Log Receiving", create a new rule, and select UDP as Transfer Protocol and 514 as port. Click "OK".

# Configure EdgeRouter
EdgeOS can send logs to a syslog server, and the configuration is super easy. Log in EdgeOS, and open the "System" panel at the bottom. There should be a "System Log" section on the right. Input the Synology NAS's IP and select the log level. All firewall logging are done to warning so I select "warning" as the level. Hit "Save" at the bottom of the page.

# Configure Unifi
Unifi can send device logs to a remote server. It does not, however, send controller logs. I do not have controller running on the LAN as my Synology NAS anyway, though.

In the setting of your site, under "Site", check "Enable remote Syslog server" in "Remote Logging" and type in the IP address and port (514) of NAS. Hit "Apply Changes".

# Verify Log Receiving and View Logs
If we head back to DSM, the "Overview" page of Log Center should show more hosts sending logs. Under "Logs" page, you should be able to find and search logs by device.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
