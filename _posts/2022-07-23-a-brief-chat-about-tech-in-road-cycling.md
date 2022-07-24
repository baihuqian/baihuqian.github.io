---
layout: post
title: A Brief Chat about Tech in Road Cycling
date: '2022-07-23 19:49'
tags:
  - Cycling
---

I love riding a bike since I was a child. But it was not until late 2021 did I pick up my road bike and become serious about road cycling as my main form of exercise. As a competitive sport, road cyclists crave to ride faster and longer. And sport training is based on vast amount of data that we can collect from our riding. Tech gadgets help cyclists to gather more information about the rides and get more information during the ride. I finally have a complete setup, so this post is the summary of my thought process and my research so far.

## Bike Computer, or Smartphone

Dedicated bike computers exist for a long time. They serve a good purpose as a dedicated computing device for a specific environment. But as smartphones become the center of our lives, more powerful, more rigid, and last longer, do we still need another gadget? This was the question I asked myself and set out to answer.

First, let's analyze the functionalities we get from a bike computer:
1. Connect to various sensors and meters and display real-time stats. For road cycling, we generally use speed sensor, cadence sensor, power meter, and heart rate monitor.
2. Record ride data;
3. Navigate in the real world.

In the sport world, ANT+ has been the leading wireless technology to connect monitoring devices to each other. ANT is an acronym for “Advanced and Adaptive Network Technology,” which is a low-energy wireless protocol meant to collect and transfer sensor data. The first generation of ANT sensors would only work with ANT-enabled sensors and not competitors' products; but the second generation, ANT+, works with many different devices regardless of the manufacturer. ANT+ was developed as a lower power version of ANT, allowing coin-battery-powered devices to last hundreds of hours. ANT+ uses the same frequency range as Wi-Fi and Bluetooth (2.4GHz) but has a slower transmission speed than either one.

Smartphones support WiFi or Bluetooth instead of ANT+ for higher transmission speed, so a dedicated device, such as bike computer, is required to connect to ANT+. Such dedicated device provides significant less computing power but demands less power consumption for longer battery life, which is opposite to the early days of smartphones.

However, with the emergence of IoT and low-power wireless connectivity technology, the gap became narrower. Bluetooth Low Energy (BLE) completely changed the game. BLE consumes significantly less power, allowing coin-battery-powered devices to last longer. But BLE and the "classic" Bluetooth can be supported by the same device. This was codified by the Bluetooth 4.0 specification.

Quickly, sensor manufacturers add BLE support to their devices along with ANT+ to make them "work with smartphones". The first huddle, device connectivity, was overcome.

However, the battery life of smartphones was not great, especially connecting to multiple peripherals and GPS. The main reason of high power consumption was the use of CPU for BLE, cellular, and GPS connectivity, and CPU cores were designed to be powerful. Smartphone SoC since uses two technologies to be more power-efficient in this kind of use cases:
1. Introduction of co-processors for peripheral connectivity;
2. Introduction of energy-efficient cores.

Now, it is possible to record a full day of ride with multiple sensors and GPS data on a single charge. So we can really use smartphones and apps as the substitute for dedicated bike computers.

## Sensors
I'm a big fan of data. Data can show me the progress from multiple dimensions:
* Heart rate: how hard my body works.
* Power: how much output my body can produce. By comparing power at certain heart rate, I can observe fitness improvements over time.
* Speed: road cyclist crave speed. By comparing speed and power, I can identify inefficiencies from ride position, wind and aero, and rolling resistance.
* Cadence: this is for improving pedaling skills.

Speed and cadence sensors are relatively cheap. I have a Trek bike and I use [Bontrager DuoTrap S](https://www.trekbikes.com/us/en_US/equipment/bike-accessories/bike-computers-gps/bike-computer-sensors-accessories/bike-computer-sensors/bontrager-duotrap-s-digital-sensor/p/12319/) because I like its frame integration.

Power meters are expensive, though the price has come down a lot. I use the crank arm-based, left-only power meter because it is most affordable. Crank arm-based power meter has built-in cadence meter, so I could have gotten a cheaper speed meter, like [this Wahoo speed sensor](https://www.wahoofitness.com/devices/bike-sensors/bluetooth-speed-sensor).

Heart rate is more complicated. Chest straps are most accurate but they're not comfortable. Then we have Apple Watch. It has a built-in heart-rate monitor with acceptable accuracy for enthusiast use. But, Apple Watch does not broadcast the heart-rate feed to other devices over Bluetooth, despite having the hardware to do so. So to use Apple Watch to record heart rate, we need an app that supports Apple Watch.

## App
Thus, I need an app with Apple Watch heart rate support and can connect to multiple BLE devices. While I would love to support the developer, I don't want to pay a high subscription fee. I use [Cyclemeter](https://cyclemeter.com/). I find it focused, easy to use, very customizable, and costs only $10 a year.

Cyclemeter does not support navigation, so either I use normal navigation apps like Goole Maps, or a cycling-specific one like Komoot or Ride with GPS. Both apps allow us to configure routes on the big screen of a computer and then load the route to the app. I like Komoot's location-based pricing more than Ride with GPS's monthly subscription pricing, since I only ride in one region and occasionally use navigation. Cyclemeter is able to continue recording in the background while navigation is in progress.

So now, my phone can do everything a bike computer can do. To use it as my bike computer, it must be easily accessible. Bike computers are mounted in the front of the handlebar, but smartphones are heavier and more expensive to repair if it drops during a ride. I'm a big fan of Peak Design's products for photography, and I use their [out front bike mount](https://www.peakdesign.com/products/out-front-bike-mount) and corresponding cases. It's easy to mount and unmount, and more importantly, it's secure. I had a crash and my phone did not fly off the mount, so I completely trust it.

Now with the vast amount of data I collect on every ride, I need a platform to analyze them. I really like Strava. It has all the functionalities for an enthusiast, though not as geeky at the pro level. It automatically compares similar rides and segments, showing me progresses I made without any effort. Its social functionality and challenges turns our real world in a giant competition to encourage me to ride more and ride faster. I also made a few friends on the platform. After all, riding with others is more fun than riding solo.

Beyond 30 km/h or 20 mph, aero drag is the dominant factor in resistance. Because I live in the windy city, wind speed and dimension plays an important part in variations of ride performance from day to day. [My Wind Sock](https://mywindsock.com) adds weather analysis to Strava activities, allowing me to visualize wind impact on along my ride. Though the data may not be 100% accurate due to the coarse granularity of wind data, it is useful to understand how wind can impact my performance.

## Summary
So to summarize:
* Use sensors with BLE, Apple Watch for heart rate;
* Cyclemeter as my main recording app;
* Komoot as my navigation app;
* Strava to collect and share ride data;
* My Wind Sock for weather data to rides;
* Peak design out front bike mount to hold my phone to the handlebar.

I really like this setup. I don't think I'll get a dedicated bike computer any time soon.

Ride on.
