---
layout: post
title: 'Troubleshoot and Improve the Throughput of your NAS'
date: '2023-06-21 21:00:00'
tags:
  - NAS
---

Sometimes, your file transfer speed to your NAS is quite slow that your important workflow takes more time than usual. How can you figure out whether it is normal, or something is not right? Unfortunately, file transfer speed to NAS, which is populated with spinning hard drives, over your home network, can be impacted by many factors:

1. The network connection from your PC to your NAS, including the connection itself and utilization of the channel. For example, ethernet runs at different speeds, such as 100 Mbps, 1 Gbps, 2.5 Gbps, or even 10 Gbps, and in rare scenarios a higher grade connection would stuck in a lower speed. If you connect to your NAS over wifi, then more factors can be at play.
2. The read/write speed of your hard drives. Generally speaking, the higher the RPM is and the larger the cache is, the faster the read/write speed is for your HDDs.
3. The type of file transfers. HDDs are faster for large, sequential read/writes, and notoriously slow for small, random accesses.
4. The concurrent access to your NAS.
5. The overall load, CPU and memory, of your NAS.

## Network Connection Speed

First, we can easily test and confirm the network connection speed between your PC and your NAS. The easiest way is to set up a speed test server on your NAS using Docker. I use two: [iperf3](https://hub.docker.com/r/networkstatic/iperf3) and [openspeedtest](https://hub.docker.com/r/openspeedtest/latest). `iperf3` is a common command-line tool to test network performance, and OpenSpeedTest is an HTML5-based open source network performance tool.

For Gigabit ethernet, both tools should be able to hit close to 950 Mbps in both directions throughout the test. If you cannot achieve the rated speed or the speed drops during the test, it suggests something is not right for your network.

## Maximum Seqential Read/Write Speed

Once you verify your network is not the culprit, next verify that your disk arrays and the transfer protocol do not become a bottleneck before saturating the network connection. Use an application like [Blackmagic Disk Speed Test](https://apps.apple.com/us/app/blackmagic-disk-speed-test/id425264550?mt=12) to test the sequential read/write speed to your volume. You can create a folder on your NAS, set the test target to it, and start the test. The test should be able to repeatedly sustain at about 110 MB/s in both read and write tests over Gigabit ethernet.

If you cannot reach this speed, the next step is to test the performance of the hard drives. Synology DSM provides drive benchmark tools in `Storage Manager -> HDD/SSD -> right click on a drive -> Benchmark` and select `Run Test Now`. Make sure the drive is not under load when running the test. Normally, a single drive can achieve about 110 MB/s throughput at 5400 RPM and 160 MB/s throughput at 7200 RPM. If your drive(s)'s throughput is significantly lower, then something might be wrong with it or the SATA port.

If the drive benchmark is normal, then it is likely something related to the file sharing protocol. The SMB protocol (Server Message Block protocol) is the most commonly-used protocol for home use after AFP being deprecated by Apple. The SMB protocol version used might be too low, for example.

## Fill All Memory Slots

NAS may come with empty memory slots. For example, my DS920+ comes with 4GB of irreplacable memory and an empty SO-DIMM memory slot. You can still benefit from expanding the available memory even you don't use memory-intensive workloads like VMs or docker containers. DSM uses memory for caching and less swapping means more disk IOPS are used to serve the file system than running the DSM. Memory is so cheap these days that filling all empty slots is a very cost effective upgrade. You don't have to buy Synology-branded overpriced memory modules; compatible memory modules at the maximum supported clock speed are widely available. List of compatible modules are available on the internet, such as this [Reddit-curated Google sheet](https://docs.google.com/spreadsheets/d/13pJDfDot_7CmSWeo1jjbegM82QwQNIW0gFQ9o_4xhXA/edit#gid=1974572930).

## Set up NVME Cache or Storage Pool

Of course, SSD is way faster than HDD. Many newer NAS models come with NVME M.2 slots that support read or read/write cache and storage pool. It can be tempting to set it up. However, it might not yield much benefit.

For caching, you must achieve a high cache hit rate to see much benefit. If the data you access are not already in cache, then you will not see performance benefit. And cache writes frequently to the drive, and it can burn through consumer-grade models quickly, like in a year. So it can be an expensive investment with little return.

For storage pool, unless you have 10GbE connection, you're still limited by your network throughput. So to achieve the full benefit, you need 2 M.2 drive in RAID1 (mirror), a 10GbE network card in NAS, 10G switch, and a 10GbE network card/port in your PC. It is quite a lot of investment.
