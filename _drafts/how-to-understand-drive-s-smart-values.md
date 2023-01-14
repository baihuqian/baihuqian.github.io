---
layout: "post"
title: "How to Understand Drive's SMART Values"
date: "2023-01-05 16:55"
---

Self-Monitoring, Analysis and Reporting Technology (S.M.A.R.T., often written as SMART) is a monitoring system included in computer hard disk drives (HDDs) and solid-state drives (SSDs). Its primary function is to detect and report various indicators of drive reliability with the intent of anticipating imminent hardware failures.


Important are:

Start/Stop Count

Spinup time

Power On hours

They give you a feeling for the age and purpose of the disk. Lots of Start/Stops are typical Workstation use cases. Low Start/Stop but high Power On hours are typically for server and nas usage.

This drive has run about a year, which is used but I wouldn't consider it bad.

Another Value, if present, is max temperature. This should not exceed 50 °C. (But it depends on the type and manufacturer)

Some other important values are

Reallocated Sector count

Current Pending

And uncorrectable.

If there are some bad sectors pending, the could be reallocated, but when there are uncorrectable sectors, the disk should be avoided for important stuff, but still could be used for substitutable stuff.

UDMA CRC Errors can occur, when you have a bad cable or a bad controller/hba. If the number doesn't increase it is fine.
