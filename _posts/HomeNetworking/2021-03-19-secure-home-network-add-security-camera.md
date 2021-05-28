---
layout: post
title: 'Secure Home Network: Add Security Cameras'
date: '2021-03-19 22:11'
tags:
  - HomeNetwork
  - NAS
  - HomeSecurity
  - IoT
---

Home security is important to everyone. Security camera, once expensive, has now become very accessible, both in terms of unit price and ease of deployment. IP cameras that runs on IP network (in other words, require no special cabling) can be an effective addition to your home.

Given that [this blog series focus on home network and digital privacy]({{ site.baseurl }}/tags/#HomeNetwork), I will discuss security cameras from three aspects:
* Home network;
* Privacy and security;
* Integration with home automation systems.

# Network Considerations for Security Cameras
There are two categories of IP security cameras in terms of network connectivity: wired and wireless. Wired cameras require an Ethernet cable to connect to network, and often powered by Power-over-Ethernet (PoE). Wireless cameras connect to your wireless network and are powered by separate power cables.

Wired cameras, especially powered by PoE switches, can be simple to deploy and highly-reliable. It requires only a single Cat5e ethernet cable, which is cheap and versatile, and can have reliable network connectivity to capture every moment. It requires no outlets in adjacency and can be deployed far away, both indoors and outdoors. However, running ethernet cables from your network closet to the desired deployment locations can be challenging.

Wireless cameras can be placed anywhere with access to an outlet. It can be a great addition to your home if you have fast and reliable WiFi. However, IP cameras consume a lot of bandwidth continuously. If your wireless environment is chatty because of the density of wireless signals, or the signal strength is not ideal, such as in the garage or outdoors, IP cameras would have a hard time working reliably. Moreover, the majority of wireless cameras work on 2.4GHz WiFi only, which is over-saturated and very noisy.

I recommend using wired PoE cameras whenever possible; but wireless cameras can be easy to deploy if you have great WiFi coverage and low interference with your neighbors. If you have to use wireless cameras, use those with 5GHz (802.11ac) support. If you use wireless cameras, you are unlikely to run it in full resolution. So anything beyond 1080p is not useful.

# Privacy and Security
As security cameras stream your home continuously 24 hours a day and 365 days a year, privacy and security is paramount. Here are my principles:
1. No cloud storage. Not only manufacturers charge monthly fees to use these cloud-based cameras, but also data are stored in the manufacturers' infrastructure. You would be surprised how vulnerable these vendors' infrastructure is! In addition, cloud storage requires cameras to upload streams continuously using your scarce upstream capacity.
2. No Internet access for cameras whenever possible. While some cameras require cloud connectivity to form P2P with your phone to use their app, I prefer to use cameras that can operate in a standalone fashion.
3. Separate VLAN for wired cameras. This is for logical isolation. While separate wireless SSID would be ideal, I find it too noisy to have the same number of SSIDs as VLANs.
4. Block intra-VLAN traffic to security cameras. While IP cameras are unlikely to try reaching other devices on the network, other connected devices are overly eager to find devices on the same network with all kinds of multicast (SSDP) and broadcast traffic.

Effectively, my cameras run on its separate network with no connectivity to the internet. I implement this with
1. Fixed IP addresses for all cameras in a continuous block. Assign this address range to a network group.
2. Block traffic from this network group to elsewhere while allow established/related packets. This allows trusted devices, such as smartphone, to access cameras.

# Surveillance Station
Synology NAS comes with a great software for IP cameras, Surveillance Station. Surveillance Station is a enterprise-grade security surveillance solution. Each NAS device comes with license for two cameras and licenses for additional cameras can be purchased at $50 per camera. It is a completely offline and self-hosted solution, but the solution is reliable and requires minimal setup.

It supports a vast number of camera models with various degree of support, so you can find a great variety of selections for each of your use case. I purchased Amcrest cameras with full camera control support by Surveillance Station, so I do not have to use Amcrest's app. It also supports generic protocols such as ONVIF and many online posts report success with cameras with no explicit support. This allows you to purchase cameras from multiple manufacturers without using multiple apps or being locked by the vendor.

Setup is easy and the [DS Cam app](https://apps.apple.com/us/app/ds-cam/id349087111) works well. I set it to record on motion detection so it does not write to my NAS continuously. Motion detection by Surveillance Station and camera both work well for my simple use case.

# HomeKit Support via Homebridge
Apple HomeKit supports cameras, but HomeKit-certified cameras are very overpriced in my opinion. Since I use HomeKit as my main home automation platform and I have [Homebridge]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-17-secure-home-network-make-your-iot-devices-compatible-with-apple-homekit-with-homebridge.md %}) set up, I can expose my IP cameras using the [Homebridge Camera FFmpeg](https://github.com/Sunoo/homebridge-camera-ffmpeg) plugin. This plugin transcodes the [RTSP feed](https://en.wikipedia.org/wiki/Real_Time_Streaming_Protocol) into AAC and makes the camera feed available via Homebridge. Most cameras support RTSP but the the network path vary by vendor. For example, here is [Amcrest's RTSP specification](https://support.amcrest.com/hc/en-us/articles/360052688931-Accessing-Amcrest-Products-Using-RTSP).

Once you configure it in Homebridge, you can add the camera to your Home app and the camera feed should be available. You may want to reduce the frame rate and resolution of the video stream if your Homebridge is run on resource-constrained compute, such as NAS or Resberry Pi.

# HomeKit Support via Home Assitant
Home Assitant has [integration with Synology DSM and Surveillance Station](https://www.home-assistant.io/integrations/synology_dsm/) to bring camera feeds into Home Assistant, and can expose them HomeKit. I find the video feed in both Home Assitant and Home app very usable, even over the public internet (require you have a home hub). This is my current setup.

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
