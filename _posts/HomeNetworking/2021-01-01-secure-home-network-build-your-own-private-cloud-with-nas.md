---
layout: post
title: 'Secure Home Network: Build Your Own Private Cloud with NAS'
date: '2021-01-01 18:51'
tags:
 - HomeNetwork
 - NAS
---

As everyone's digital footprint increases with more and more photos on phones, more and more digital (or "paperless") documents from life and work, and as Covid-19 changes our lives to be more online, we now have a large amount of digital data lying around on our desktops, laptops, tablets, phones, and in the cloud. Many tech companies provide online storage, either as a storage service, like Dropbox or Google Drive, or as a bundled benefit, such as iCloud storage when you have an iCloud account for your Apple devices.

Cloud storage is convenient yet problematic in the following ways:
1. Cost. Cloud storage charges a monthly subscription fee. For example, a 2TB plan costs $10 monthly from iCloud, Google, and Dropbox, and $20 extra from Adobe Creative Cloud. It translates to $120 per year. Note, this pricing is for individual plans; family plans with limited sharing is more expensive.
2. Internet bandwidth usage. Given the data is stored in the cloud, uploading and accessing requires internet connectivity. Most ISP's home internet has low upload speed compared to download speed by 10x, thus taking significant amount of time to transfer your local data to the cloud. In addition, accessing data requires downloading data from the internet. As many ISPs imposing data quota regardless of your bandwidth, such as Comcast's 1.2TB monthly quota, frequent access of large files over the internet will result in higher bill from your ISP.
3. Security. Although most cloud storage providers' security posture is excellent, the security of your data depends on the security of your account, such as password and two-factor authentication. As most people's own security lags far behind with insecure and reused passwords, compromised accounts are not uncommon.
4. Provider lock-in. If you are not happy about provider increasing pricing or changing policy (such as [Google abandoning previously-promised unlimited data in Google Photo](https://www.bbc.com/news/technology-54919165)), it is very time-consuming and error-prone to move your data to a different provider. You almost have to download all your data and upload to a different provider, resulting in a lot of internet usage (in terms of both bandwidth and data quota). You almost have no choice but to continue to pay the higher price or upgrade to a higher plan.

Storage on your computers or direct attached storage devices, such as USB drives, suffers from the following drawbacks:
1. Availability. Data is only available when the computer is running or storage devices are connected to the device you are using.
2. Device incompatibility. Your storage devices may or may not work with your Windows PC, Mac, tablets and phones at the same time. Tablets and phones sometimes cannot directly access data from storage devices.
3. Data loss due to device failure. Nothing lasts forever but your data should. It is common for a computer disk or storage device to die all of a sudden, leaving you scrambling with data recovery or permanent data loss. Having multiple backups sometimes can be hard to manage and easy to get out of sync.

Network-Attached Storage, or NAS, brings the benefit from both worlds. NAS is a specialized computer designed to run 24-7, host your data with managed redundancy, and provide access to your data from all devices by supporting a wide range of access protocols. In addition, many NAS devices can run applications, such as IoT controller (see my post on [Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %})), DNS server (see my post on [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %})), or surveillance station for your home security systems, 24-7 without a dedicated computer.

NAS has a high up-front cost compared to cloud storage plans, but it is cheaper over the long run, especially with large amount of data. For example, a top-the-line NAS hard disk from WD (Red Pro) costs $100 for 2TB (though I personally only use Red Plus at $70) and should last 5+ years, resulting in just $20 per year. Of course, with redundancy the average cost per TB would be higher, but it is still going to be cheaper over the long run.

Of course, NAS is your personalized device and you manage it. Thus, it is important to do some research and find a good one for your current and future use cases.

# NAS Device and Drives
NAS is a specialized computer running vendor-specific software. Thus, it is important to decide both the hardware specification and the software capabilities prior to purchase to avoid expensive upgrades later. I personally made such a mistake 5 years ago when purchasing my first NAS, and the user experience has been poor and I made an expensive upgrade recently, compared to the life expectancy of a proper NAS device.

Generally there are four factors when purchasing a NAS device:
* Vendor. Not only vendor makes the hardware, but also the software. Software provides the features and functionalities that manages the data storage and providing the data access. The software determines most of the user experience, just like Windows versus Mac.
* CPU and memory. The responsiveness and usability depends on the CPU and memory of the NAS, and in a lot of cases they are not upgradable.
* Drive bays. NAS is a storage device and the number of drive bays will limit the performance, redundancy, and upgradability of the device. While drive bays are mostly for 3.5'' hard disk drives (HDD) for their longevity and capacity, SSD caching can provide good performance boost.
* Connectivity. NAS connects to your home network via ethernet. Though Gigabit ethernet is still the mainstream, multi-Gigabit ethernet is on the horizon with 2.5Gbps, 5Gbps, and 10Gbps models. While I believe a single or two Gigabit ethernet ports will satisfy most of the user's need in foreseeable future, technology advancement can only amaze people in consumer electronics. In addition, USB ports, expansion slots, or even connectivity to additional drive bays (such as eSATA), can be useful for certain users.

NAS drives sometimes do not come with drives in it. In fact, I prefer purchasing drives separately to suit my own use case. You should purchase drive models designed for NAS usage, which is completely different from computer usage, such as WD Red or Seagate IronWolf. You should decide how many drives to begin with and their capacity for your use case, and decide on the number of drive bays in your NAS device. The decision gets more complicated if you are upgrading like me. So I'm going to tell my story.

# My Story with NAS
I need storage for my photos, documents, and software development needs. I also have a Macbook Pro so I use Time Machine as the backup. I had about 500GB total in 2015 and my portable hard drive was full, so I was looking for an upgrade. I bought a WD MyCloud EX2, a 2-bay NAS with 2 WD Red (equivalent to Red Plus today) 2TB hard drive in it. I set up RAID1 for full data redundancy with a total of 1.8TB of usable space. Perfect for my use case under a budget, right?

Well, the NAS's CPU and memory is too weak for even some light usage, and the software from WD is not good. The management console is slow. It does not work with Mac well as broken connection lingers resulting in failed connections after sleep for both file and Time Machine usage. Because of its weak CPU, the throughput is quite poor and the extensibility of apps is limited. But I have to give WD the credit for managing redundancy well and the longevity of their disks. After 5 years of continuous usage, the disk still performs well with no degradation in sight.

Because of Covid, I spend all my time at home tinkering technology and my collection of photos. I spend some time thinking what I need for a NAS upgrade and my budget, and here is my requirements:
1. A software-first approach with lots of possibility through upgrades and plugins, similar to my approach with network and reasons to choose UniFi.
2. A 4- or 5-bay NAS that can reuse my existing two 2TB drives.
3. Gigabit ethernet is enough for me. Most of my networking devices are Gigabit only and I have no plan to re-wire my home with more expensive cabling and upgrade my devices.
4. Powerful CPU and memory to run Docker.
5. A $1000 budget for the upgrade, including hard drives.

I end up with a Synology DS920+ ($550) and two new 8TB WD Red Plus drives ($200 each). I like Synology's DSM software and its package ecosystem. It's SHR (Synology Hybrid RAID) system allows mix-and-match of drive sizes and models, through I will stick with WD Red. This model provides 4-bay with the possibility of extending it with another 5-bay device through its eSATA interface, and support NVMe SSD cache. Its CPU and memory (upgradable to 8GB) is powerful for my foreseeable use cases.

With the upgrade, I get 12 TB of theoretical space with possibility of expansion by replacing my 2TB drives with 8TB or larger ones. The software features are way beyond my need and the speed and responsiveness exceeds my expectation. It requires quite some learning and I just start my journey with it, but I am very happy with it.

If you need help choosing a model, configuring something, or asking for help, I recommend [NAS Compares](https://nascompares.com). It provides guides for purchases, setup, and common use cases across the leading brands in the industry. The rest of the post documents what I have gone through for the setup and what features and packages I use.

# Initial Configuration and Migration
This is not a guide as there are plenty on the internet, but I will document what went wrong for me here.

You should decide on
* The name of your NAS as it will be used to configure your devices talking to it;
* The username and password as changing it requires changes to all your devices;
* The storage configuration, including disk configuration (what should be in the storage pool when it is created), file system, and RAID mode. They are impossible to change without reformatting all the disks.

I use SHR and Btrfs in a single storage pool and volume, but I only added the two new 8TB drives to it during the initial setup. SHR allows adding additional drives that are *larger than the largest* drives, **or** *equal to any of the drives* in the storage pool. So to achieve my goal of reusing my existing two 2TB drives, I need to put one of them into Synology during initial setup. I ended up redoing the entire setup after two days and all the data migration completed.

So the correct steps to migrate from my existing WD MyCloud EX2 with two 2TB drives are:
1. Ensure the two drives in WD MyCloud EX2 are in RAID1 (full redundancy to each other) and the RAID status is normal.
2. Take one drive out and put it into Synology along with two new 8TB drives. Perform the initial setup and configuration.
3. Migrate all the data from WD MyCloud EX2 to Synology.
4. Wait for the disk parity check to complete. You cannot add new drives until the storage pool's status becomes "normal". This can take a long time depending on the disk size.
5. Pull the other drive in WD MyCloud EX2 out and put it in Synology, and add it to the storage pool (i.e., "expanding" the storage pool).

The user created in the setup wizard has admin privilege. As a security best practice, I created another regular user for myself to access the data, and configure my computers to use the regular user for data access.

# Maintenance
Synology NAS is very low maintenance. The default setting handles most without user intervention. For example, it automatically set up a monthly S.M.A.R.T. (Self-Monitoring, Analysis and Reporting Technology, to detect and report various indicators of drive reliability with the intent of anticipating imminent hardware failures) test.

I set up
* email notification for drive health in case that I don't log in to DSM as admin to learn about drive problems;
* quarterly data scrubbing in Storage Manager to run parity consistency check nightly.

# Packages and Use Cases
I have used the following packages:
* Universal Search: make sure to include folders for indexing.
* Synology Drive;
* Synology Office;
* Moments.
* Docker.

## Run Docker Containers
Synology Docker package comes with a very nice UI so that we don't need to use command line to manage images and containers. To host configuration files for containers, I created a share folder called `docker`, which is only accessible by the admin account so far but is able to extend the privilege in case some users should run docker containers themselves, and sub-folders (such as `docker/pihole`) as each container's volumes.

So far I have configured [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}/#installing-pi-hole-on-synology-nas) and [Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %}/#install-on-synology-nas) to run on it. I find the experience very straightforward.

## Photography Use Cases
Synology provides packages aiming at arranging photos for users: [Moments](https://www.synology.com/en-us/dsm/feature/moments) and [Photo Station](https://www.synology.com/en-us/dsm/feature/photo_station). Since I use Lightroom 6 (the last Lightroom standalone version without the need of a subscription) to manage and edit photos, I do not use either of them to manage photo catalog. I set up Moments for my photo selection to leverage its auto-organization feature.

After mounting NAS as a network drive, I manually convert RAW files into DNG and upload them to NAS, then import them into Lightroom. My Lightroom catalog resides on my editing machine but all photos and Lightroom backups are on my NAS. Then I use [Synology Drive to back up](https://www.synology.com/en-global/knowledgebase/DSM/help/DSM/Tutorial/backup_from_computer) the Lightroom catalog in case my computer dies.

## Time Machine
Initially I tried to copy the Time Machine backup from WD MyCloud EX2 to Synology to retain its full history. But because the underlying file system is different, Macbook is unable to recognize it in the new location with a "volume in use" error message. In the end I set up a new Time Machine backup by following [Synology's guide](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/Backup/How_to_back_up_files_from_Mac_to_Synology_NAS_with_Time_Machine).

The backup speed is much faster. The initial backup of 90GB complete in under 3 hours, and subsequent backups attempts do not show "volume not found" error due to broken connections. In fact, the Resource Monitor in DSM can kill connections if it becomes a problem without restarting everything.

# Cost Analysis
Assuming NAS lasts 10 years and drives last 5 years, my NAS costs $55 per year. The total cost of ownership for my 12TB storage is $135 per year. As a comparison, personal or family plans from major cloud storage providers caps at 2TB for $120, so I get a lot more space and growth potential with similar price point. Business plans charges per user with minimum user requirements rather than per storage space so it is more expensive for prosumer usage.

I can upgrade to 18TB or 24TB in the future by replacing one or two 2TB drives with new 8TB drive at an incremental cost of $6.67 per TB per year. Adobe Creative Cloud offers higher storage at $10 per TB per month or $120 per year. Thus, the total cost of ownership is much, much lower over the long run.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
