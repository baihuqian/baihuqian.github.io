---
layout: post
title: 'NAS: Two Years Later'
date: '2023-01-03 09:42'
tags:
  - NAS
---

At the beginning of 2021, I set up my Synology NAS and wrote about it in [this blog]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-01-secure-home-network-build-your-own-private-cloud-with-nas.md %}). Two years have gone by and I thought I write another update about my experience with NAS.

Just to recap, I bought a Synology DS920+, a 4-bay, dual Gigabit Ethernet NAS, and populated them with 2 new 8TB WD Red Plus and 2 old 2TB WD Red Plus, at the end of 2020. I created a single storage pool across the 4 drives and a single volume. I used about 40% of about 10.7 TB available space so far. I run various Synology software as well as Docker containers. I have no issue or compliants in the past two years, for both the management software (Synology DSM), the storage, and 3rd-party software. It's highly reliable and very low maintenance.

This blog post covers major improvements I have done over the past two years.

## UPS
Power supply is not the most stable at my house, and outage with very short duration can occur when weather is bad. Unexpected power outage is bad for NAS, so I purchased a UPS (Uninterruptible Power Supply) for it. I can run NAS and my home network on UPS for more than an hour, and it has covered all temporary power outages to date. UPS also comes with a USB interface that can be plugged into NAS, and DSM can recognize it as "USB UPS" under `Control Panel -> Hardware & Power -> UPS`. The UPS will notify NAS when its battery is low so NAS can dismount disks and power off safely without losing data. Just like any backups where testing the restoration is important, testing the UPS functionality after everything is set up to make sure it will do the job when power is out. I attempted flipping the switch multiple times with various power-off duarations to make sure either NAS does not shut down unexpectedly or it powers down properly when UPS battery is low.

## Acitve Backup for Business
Mac provides native backup solution with TimeMachine and Synology's implementation of it is nice to use. But I have a Windows machine at home and Windows' native backup isn't great. Synology's Acitve Backup for Business (ABB) can help here. It runs a small agent on my PC and can incrementally back it up. It will allow me to create a bootable stick to restore the PC to the backup state if anything were to happen. Of course ABB is much more powerful than backing up a single PC (it can do multiple PC, Linux machines, and VMs) for home use, but it's a nice feature that comes with Synology NAS for free.

As of 1/3/2023, ABB now supports Mac, too. I have not tried it and compare it with TimeMachine, though.

## Replacing Disk
Two of my old 2TB drives are over 7 years of power-on time. While they are running fine with no strange noise or reported problems from DSM, I started to worry about drive failures. These two drives are manufactored in the same batch and operated alongside each other in the same environment through their life. After all, drive failures are not scary; concurrent drive failures are. Additionally, I have never replaced a drive before, so I don't have experience of handling drive failures. Thus, it is not a safe operational mode with these two old drives and I decided to swap one out.

The Synology Hybrid Raid (SHR) allows gradual increase in storage pool capacity by replacing smaller drives with larger ones over time. SHR accepts replacement drives that are either of the same size of any existing drives, or larger than the largest drive. Given that most older drives would be smaller, it is a natural choice to replace them with larger ones in the storage pool, based on the per-TB cost and storage needs. Because I have 2TB and 8TB drives, my choices are 2TB, 8TB, and anything larger. 2TB is $60 per drive or $30/TB, and 8TB is $130 or $16.25/TB, and I really don't need anything bigger, so I purchased another 8TB Red Plus. Its price has come down (from $200 two years ago), though the model is different: lower disk speed and smaller cache. The small difference in lower read/write speed is probably not going to be noticeable in my workflow anyway so I welcome the lower price.

The replacement procedure is simple, yet I'm glad that I got a chance to practice without pressure, because it's a lengthy one:
1. Order the new drive. It was in stock and arrives in 2 days from B&H.
2. Meanwhile, I performed data scrubbing to ensure consistency in parity before swapping drives. You cannot do this if one of the drives completely fails.
3. When the new drive arrives, deactivate one of the 2TB drives in Storage Manager. It rendered the storage pool "degraded" and started beeping alarm. It's useful when the storage pool is "actually" degraded; but because it's manual operation, it added unnecessary pressure. It took me a while to find how to stop the beep (in `Control Panel -> Hardware & Power -> General -> Beep Control`, click `Mute`).
4. My NAS model, DS920+, supports hot swap, so I remove the drive from NAS enclosure, pop the drive out, and insert the new drive into the enclosure.
5. Click "repair" on the storage pool. It started the rebuild process. Because I replaced with a larger drive, I also selected to expand the storage pool once repair is complete.
6. It took 63 hours to fully repair the storage pool, during which all drives' read and write usage are high.
7. When the repair completes, data scrubbing is started to ensure data consistency.

While the manual action is minimal, the replacement took about 5 days from ordering the drive to the completion of repair, during which there was no data redundancy. If another drive failed due to the continuous load of repair, I would have data loss. Again, drive failure is not terrible, as long as only one fails at a time and other three survive the repair. Having drives with different lifespan (in my case two 2-years old, one 7-years old, and one brand-new) and potentially from different manufactoring batches would minimize the chance of concurrent failures. One could argue SHR2, which provides 2-drive redundancy, would provide against two simultaneous drive failures. But the cost of ownership increases by a lot while the flexibility of drive sizing reduces. I still believe SHR1 provides the best return for home users.

The "healthy" drive that got replaced is put on the shelf. If there was another drive failure in the next three years, I could put it back in service without buying new drives. A single old drive is perfectly fine in a redundant system. After three years would be another story. At that point I would have a 10-year old, two 5-year olds, and one 3-year old, So I might replace the 10-year old with a new drive in case one of the 5-year old drives fail. I generally like the idea of having drive ages being two-year apart in a four-bay system, e.g., new, 2-year, 4-year, and 6-year, using entry-level NAS drives with 3-year warranty, such as WD Red Plus and Seagate IronWolf. I think it strikes the balance between data loss risk and the cost of ownership.
