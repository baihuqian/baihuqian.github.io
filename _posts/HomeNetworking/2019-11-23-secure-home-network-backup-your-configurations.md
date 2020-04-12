---
layout: "post"
title: "Secure Home Network - Backup Your Configurations"
date: "2019-11-23 15:49"
tags:
 - Networking
---

Now, you have already set up your device and your management systems (UniFi Controller and UNMS). Nothing lasts forever so it is important to configure backups so that in an unlikely event of disaster, you can easily restore your device configurations and management systems.

UniFi Controller and UNMS both provide automatic backups:
* [UniFi - How to Configure Auto Backup](https://help.ubnt.com/hc/en-us/articles/226218448-UniFi-How-to-Configure-Auto-Backup)
* [UNMS v1 - How to Back Up UNMS](https://help.ubnt.com/hc/en-us/articles/115015690027-UNMS-How-to-Back-Up-UNMS)

UniFi's backup files are placed at `/var/lib/unifi/backup/autobackup/` and belongs to `unifi:unifi` user and group. UNMS's backup files are placed at `/home/unms/data/config-backups/` for device configurations and `/home/unms/data/unms-backups/backups` for controller backups, respectively, and belongs to `unms:root` user and group. Thus, you should use `sudo` to access these files or configure users and groups.

I opt for `s3 sync` functionality to sync these folders to an S3 bucket.
1. Attach an instance role with S3 write permissions to the instance running your UniFi Controller and UNMS.
2. Write a short script as your backup routine.
3. Create a cron job to run them automatically.

The backup script is `s3 sync` commands:

```bash
#!/bin/bash
sudo aws s3 sync /var/lib/unifi/backup/autobackup/ s3://unifi-backups-baihqian/unifi-controller
sudo aws s3 sync /home/unms/data/config-backups/ s3://unifi-backups-baihqian/unms/config-backups
sudo aws s3 sync /home/unms/data/unms-backups/backups s3://unifi-backups-baihqian/unms/unms-backups
```

The cron job runs a couple of hours after the weekly backup, which is configured to run at midnight UTC every Monday:
```bash
0 5 * * 1 /home/ubuntu/unifi-controller-backup.sh
```

Then, if you lose your instance, you can easily launch another one, attach the Elastic IP to the new instance, install controllers and restore them from the S3 bucket.


# Further Reads
This is the post series. Other posts on the home network topics are:
1. [Device and Management Setup]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-07-secure-home-network-device-and-management-setup.md %})
1. [Isolating Connected Devices with VLAN]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-02-secure-home-network-isolating-connected-devices-with-vlan.md %})
1. [Using HomeKit Devices Across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-08-27-secure-home-network-using-homekit-devices-across-vlans.md %})
1. [Using AirPlay Across VLANs]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-08-31-secure-home-network-using-airplay-across-vlans.md %})
1. [Troubleshoot DHCP Problems]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-troubleshoot-dhcp-problems.md %})
1. [Extend WiFi Coverage with Multiple APs]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-01-11-secure-home-network-extend-wifi-coverage-with-multiple-aps.md %})
1. [Block Ad and Tracking with Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})
