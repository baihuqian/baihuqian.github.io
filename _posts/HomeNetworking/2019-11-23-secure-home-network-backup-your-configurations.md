---
layout: "post"
title: "Secure Home Network - Backup Your Configurations"
date: "2019-11-23 15:49"
tags:
 - HomeNetwork
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
sudo aws s3 sync /var/lib/unifi/backup/autobackup/ s3://<backup-bucket-name>/unifi-controller
sudo aws s3 sync /home/unms/data/config-backups/ s3://<backup-bucket-name>/unms/config-backups
sudo aws s3 sync /home/unms/data/unms-backups/backups s3://<backup-bucket-name>/unms/unms-backups
```

The cron job runs a couple of hours after the weekly backup, which is configured to run at midnight UTC every Monday:
```bash
0 5 * * 1 /path/to/script/unifi-controller-backup.sh
```

Then, if you lose your instance, you can easily launch another one, attach the Elastic IP to the new instance, install controllers and restore them from the S3 bucket.


# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
