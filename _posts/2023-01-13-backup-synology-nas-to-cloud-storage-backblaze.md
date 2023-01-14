---
layout: post
title: Backup Synology NAS to Cloud Storage (Backblaze)
date: '2023-01-13 20:48'
tags:
  - NAS
---

When you read the title, you might ask: "the whole point of running NAS is not to pay for Cloud storage, as [you described]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-01-secure-home-network-build-your-own-private-cloud-with-nas.md %}), what is this about?"

Your NAS has redundancy built-in so you're protected from a single drive failures. Great. But there are other rarer but possible scenarios where you might lose data, such as NAS failures resulting in storage pool corruption, fire and flooding at your place, or even thief stealing your NAS! The 3-2-1 backup rule is to further protect your data from these scenarios.
* You create three copies of your data: the original data on your primary device (NAS) and at least two copies.
* You use two different storage devices and/or media, such as your PC, external hard drive, a USB flash drive, DVD, NAS, or cloud storage devices.
* You keep one of the backup copies off-site. By keeping copies of your data in a remote location, you prevent data loss due to a local disaster or a site-specific failure scenario.

It is likely that you have the data on your PC/phone and your NAS already, but both live in your primary location. To complete the 3-2-1 back up rule, you need to have an off-site backup. Chances are, you do not have a safe and reliable place to run your secondary backup. This is where the cloud storage provider comes in: they provide a safe and highly available location.

Now your off-site backup is rarely accessed, and it does not get accessed at file or even folder level. Nor do you need every single version of file changes, probably. So the uploading can be done on a schedule outside the busy hours (e.g., in the night), the backup can be compressed and encrypted, and it can be done incrementally. The compressed and encrypted backups can then be broken up into suitable sizes to optimize for the transfer usage of the specific provider (the files are neither too big nor too small), making backup faster and more reliable. While photos and videos are not compressible, text-based files, usually important documents, can easily achieve high compression ratio (more than 50%).

[Synology Hyper Backup](https://www.synology.com/en-us/dsm/feature/hyper_backup) is the tool for the job. It can back up folders, system settings, and software packages from your Synology NAS to a wide range of destinations, including a wide range of cloud storage providers. Using compression and deduplication, it can save space even with versioning.

Hyper Backup supports a variety of destinations. For the purpose of 3-2-1 backup, it could back up to USB drives (another copy of different media), another NAS in a different location (another copy in off-site location), or a variety of Cloud storage providers, including Dropbox, Google Drive, OneDrive, and S3-compatible storage.

I choose [the B2 Cloud Storage from Backblaze](https://www.backblaze.com/b2/cloud-storage.html). It provides 10GB of free space without requiring payment method. 10GB is enough for my valuable documents and files, though I need a different backup destinations for larger, incompressible media. The setup is easy by following [this guide](https://help.backblaze.com/hc/en-us/articles/360047171594-How-to-use-Synology-Hyper-Backup-with-Backblaze-B2) from Backblaze.

B2 Cloud Storage offers Server-Side Encryption (SSE), which encrypts data before writing to the disk, so your data cannot be accessed even when attacker gains access to the physical drives in Backblaze. In addition, you can create a password-based Client-Side Encryption (CSE) in Hyper Backup to further protect your data, even if your Backblaze account is compromised or Backblaze's SSE is compromised. But you would lose your backup if you forget your password in CSE.

Like any backup solutions, setting up backups is just one part of the game. Restoration is also important to practice regularly. Hyper Backup's restoration is easy to navigate and supports restoration of a sub-directory. You can temporarily rename a relatively-unimportant folder and attempt restoration for it.
