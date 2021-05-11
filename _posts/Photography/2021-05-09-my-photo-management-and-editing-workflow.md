---
layout: post
title: My Photo Management and Editing Workflow
date: '2021-05-09 13:20'
tags:
 - Photography
 - HomeNetwork
 - NAS
---

I'm a casual photographer. I know how to use a (proper) camera and other gears. I go out and take random photos in my leisure time and on trips. I'm not a heavy user; I don't take photos daily or edit them very frequently. Once or twice a month, I sit down, get photos off of my camera, and edit them.

I love Adobe's products. Both Lightroom and Photoshop are great products with unrivaled performance and usability for decades. However, Adobe has opted to a subscription-only model a long time ago. It makes sense for the company to generate steady stream of income, and for professional photographers who use these software for a living, and get the latest for a fraction of the cost before. It does not make sense to me to pay $10 a month to use the software once or twice a month, and my skills do not require the latest fancy features Adobe offers to get the job done. Also, I don't need its cloud storage. [I have written a great deal about my opinion about large cloud storage and Adobe's cloud-based photo service and storage is the classic one.]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-01-secure-home-network-build-your-own-private-cloud-with-nas.md %})

I bought the perpetual version of Lightroom 6 for $120 in 2017, before it started to get phased out. The price is the same as one-year of photographer subscription. My goal was to use it for as long as possible until I have to give it up. I'm happy to report that it still works today, four years later, with no signs of ending in sight. I have bought new cameras and lenses that were released after the end of Lightroom 6 (the last version 6.14 was released in December 2017) and I have found ways to make it work.

In addition, I added NAS into my workflow and have found great joy using it. I thought I would document what I have done for those out there in similar situation as me, thinking of ditching their old software and embracing the eventuality of endless subscription.

## Photo Management
I shoot RAW. If you don't and you have a decent camera that is not your phone (except iPhone 12 Pro/Pro Max), you should be shooting RAW instead of JPEG. JPEG compresses color into 8-bit channels from 10-bit in many modern cameras (256 vs. 1024), and its compression algorithm does not work nicely with high-frequency areas in the image (read: sharpness). Any post-processing would further degrade the image quality from a relatively-poor baseline. The storage card/space is so cheap now so it makes every sense to shoot RAW to get the best out of your gear.

I have a Synology NAS to store image files. Synology offers software to help manage photos with the consumer-friendly "[Moments](https://www.synology.com/en-us/dsm/feature/moments)" and professional-focused "[Photo Station](https://www.synology.com/en-us/dsm/feature/photo_station)". I do not use either of them. I organize photos by folder with dates prepended (YYYYMMDD) so it's easier to sort them chronologically. Then I import them into Lightroom.

Because I have a newer camera (Sony A7 III) and newer lenses, Lightroom 6 does not recognize the RAW files. Additionally, Sony does not offer lossless RAW for A7 III so the uncompressed RAW is quite bloated. To overcome both, I convert RAW files to DNG using Adobe DNG Converter. To be compatible with Lightroom 6, the DNG version should be **Camera Raw 7.1 and later**. The DNG file is about half of the original RAW file.

For the finished photos, I use Apple iCloud as the main storage and Synology Moments as the backup. iCloud automatically keeps all finished photos to my iPhone, iPad, and Macs, allowing me to show to friends and families immediately. Synology Moments can organize my iCloud camera row over time and back them up.

## Connectivity to NAS
The read-write speed between the computer and NAS is vital to the usability. To get the best performance out of this setup, I connect my computer (a Windows desktop with dedicated graphics card) to the wired network of my home. It is not usable to connect computer to NAS wirelessly.

I find Gigabit ethernet sufficient for my use case. I can max out at around 900 Mbps (which is 110 MB/s) sequential reads and writes to load photos into Lightroom and upload files. If you have the need, you can get 10Gbps ethernet card on your desktop, NAS, and a 10Gbps switch, or use one with Thunderbolt 3 connectivity. I personally find ethernet is a better choice than Thunderbolt: after all, everything is connected to your home network thus have access to your NAS. Unless you are a heavy user of data on your NAS from a single source that is close enough to your NAS, you are better off with ethernet. If you are a heavy user, then this post probably does not suit you. And NAS is generally louder so I find it more comfortable locking it away in a well-ventilated but sound-proof closet than sitting next to it.

## Editing Workflow
Here is my normal workflow:
1. Create a new folder on NAS;
2. As previously discussed, convert RAW files in memory card into DNG files. Move DNG files into the corresponding folder;
3. Open Lightroom and import the new folder on NAS;
4. Edit photos;
5. Export selected photos to the iCloud upload folder.
6. The iCloud client automatically uploads them to iCloud.

Photos are loaded from NAS into memory during editing and export, so the speed of NAS determines the lag of your editing.

## Lightroom Catalog
Lightroom Catalog keeps all the photo ratings and editing, so it is important to keep it backed up. Ideally, the catalog should reside on my NAS too; but it is just too slow. Instead, I place the catalog on my desktop and use [Synology Drive](https://www.synology.com/en-us/dsm/feature/drive) to continuously back it up to my NAS. In an unlikely event of data loss on the desktop, I can reimage it and sync the catalog from Synology Drive.

Lightroom catalog is full of small random files. If your NAS is good at random reads and writes, the backup speed should not worry you. I find the Drive client fast enough to back up any changes to NAS, usually within a few seconds.

## Forward Compatibility
Given its a discontinued old product, chances are it will not be compatible with new gears and new changes in the system. So far I have found two issues but have successfully resolved them:
1. [Support for new lenses and cameras]({{ site.baseurl }}{% link _posts/Photography/2021-05-10-import-new-lens-and-camera-profile-into-lightroom-6.md %})
2. [Map module stop working]({{ site.baseurl }}{% link _posts/Photography/2021-05-10-fix-the-map-module-in-lightroom-6.md %})
