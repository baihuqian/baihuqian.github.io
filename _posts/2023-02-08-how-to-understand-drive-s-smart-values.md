---
layout: "post"
title: "How to Understand Drive's SMART Values"
date: "2023-02-08 21:03"
tags:
  - NAS
---

RAID (redundant array of independent disks) is a data storage method that stores the same information on multiple hard disks to protect against drive failure. NAS drives are typically set up with redundancy, which means that they can tolerate a certain number of drive failures. However, multiple concurrent drive failures can still be problematic.

NAS drives are designed to run 24/7 for several years, with a lifespan ranging from less than 3 years (manufacturer warranty) to more than 10 years. Unfortunately, drives can fail at any point in their lifespan, often without warning. To monitor drive reliability, hard disk drives use a system called Self-Monitoring, Analysis and Reporting Technology (SMART). SMART primarily detects and reports indicators of drive failure in order to predict hardware failures.

While the goal of SMART is commendable, the indicators are not always easy to interpret. There are recommended thresholds set by manufacturers, but these can sometimes be too high or not correctly set, so understanding hard drive health from raw SMART values requires technical knowledge.

The first group of values provides insights into the age and purpose of the disk:

* Start/Stop Count (0x04)
* Spin-Up time (0x03)
* Power On hours (0x09)

For example, a high Start/Stop Count is typical of PC/workstation use, while low Start/Stop Count but high Power On hours is typical of server and NAS usage.

The second group of values indicates bad sectors and how they are handled. These values are particularly important to monitor, especially if the values become non-zero:

* Reallocated Sector Count (05, 0x05)
* Reallocated Event Count (196, 0xC4)
* Current Pending Sector Count (197, 0xC5)
* Offline Uncorrectable (198, 0xC6)

If there are pending bad sectors (197) but they can be reallocated (196), it means that something went wrong but was subsequently corrected. As long as the number of bad sectors is not continuously increasing, it's usually okay. However, if there are uncorrectable sectors (198), the disk should be replaced.

The UltraDMA CRC Error Count (199, 0xC7) can occur due to external factors, such as a bad SATA cable or controller. If the count is stable, it's usually not a cause for concern.

The temperature Celsius (194, 0xC2) shows the operating environment and heat generation/dissipation rate of the disk. Higher RPM disks and those with more platters typically operate at a higher temperature. If the temperature increases abnormally, it could indicate a problem.