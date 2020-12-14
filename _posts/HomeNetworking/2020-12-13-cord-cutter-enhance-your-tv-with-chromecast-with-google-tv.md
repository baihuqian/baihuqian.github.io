---
layout: post
title: 'Cord Cutter: Enhance Your TV with Chromecast with Google TV'
date: '2020-12-13 11:07'
tags:
  - CordCutter
---

In [Network Design Considerations for Cord Cutters]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-11-29-secure-home-network-network-design-considerations-for-cord-cutters.md %}) post, I discussed how to design your network for Cord Cutters. Specifically, I called out the desired features for your smart TV devices. However, not everyone has the latest and greatest smart TVs. This post, I will discuss how to enhance your non-smart or older smart TVs with a $50 gadget: Chromecast with Google TV.

Chromecast, made by Google, has been around for a while. It was largely a receiver for casting content from your smartphones to your TV and thus lacked functionality on its own. However, it has changed with recently-announced [Chromecast with Google TV](https://store.google.com/product/chromecast_google_tv). It is a $50 TV dongle running Google's version of Android TV, called Google TV. Yes the naming is confusing. Android TV is the Android OS for smart TV devices. Google TV, not the deprecated smart TV platform, is Google's own implementation of Android TV. It is not the same as Youtube TV, though, as Google TV does not provide any content on its own and require to have your own subscriptions to Netflix, HBO, etc., to view content.

Then, why is this new Chromecast better than other TV dongles?

First, its hardware is newer. It features 802.11ac WiFi and Bluetooth inside, and a fully-functional USB Type-C for power. It supports output for 4K 60 fps and HDR (both Dolby Vision and HDR10). The fully-functional Type-C connector can take in a Type-C hub, allowing you to connect to USB devices like keyboard/mouse and camera, SD cards to extend its storage beyond 4 GB, Gigabit Ethernet if the WiFi receiving isn't good behind the TV, etc. You need a hub that supports power delivery, such as [Anker PowerExpand+ 7-in-1 USB-C PD Ethernet Hub](https://www.anker.com/products/variant/powerexpand--7in1-usbc-pd-ethernet-hub/A83520A1) for $35. Don't buy Chromecast's Ethernet power adaptor as it costs $20 and only supports 100Mbps Ethernet. Given Chromecast sits behind TV, you may find the WiFi receiving is poor even if you have good coverage throughout your house. Thus a wired connection may be necessary to provide a good viewing experience.

Second, it has a proper remote, so you don't need to use your phone to control it. The remote comes with voice control, which is super easy to use. It also supports [HDMI-CEC](https://en.wikipedia.org/wiki/Consumer_Electronics_Control), allowing you to control your TV's power and audio level using Chromecast's remote, if your TV supports it. It removes a lot of pain points of using TV dongles around having to use multiple remotes.

Third, being Google's official Android TV implementation, it supports Google Play store, providing access to so much more apps. A lot of third-party clone of Android TV, including those in the smart TV itself, only supports vendor's own app store.

Lastly, it allows sideloading apps (installing apps from APK), enabling you to install anything you want to it. Combined with Google Play store, this provides unlimited freedom of content viewing to your TV.

## Network Design Considerations for Chromecast
Same as any connected devices, and being as notorious as Google (at collecting data in exchange for providing service), we should be careful at how it could interact with other devices on the private network and what information it could phone home. There are two areas: block undesired access to private network and block undesired tracking.

If you use [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}) DNS server, make sure to have your Chromecast use it. Google devices notoriously bypass DNS settings from DHCP servers and default to `8.8.8.8` (Google's public DNS) for "important" tasks (respective to its business). So make sure to [add a `drop` rule to block any non-Pi-Hole DNS traffic]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}/#firewall-rules).

Chromecast should not be allowed to access other devices on your LAN network unless there is a use case. There are discovery protocols, such as mDNS and UPnP, that allow devices to discover each other on a network without any special setup. By placing it in the IoT network with [firewall rules allowing casting functionality]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-12-13-secure-home-network-using-chromecast-across-vlans.md %}), we can block unnecessary interactions to your other devices.

## Sideloading Apps to your Chromecast
To sideload apps, you need a file browser. I recommend [File Commander](https://play.google.com/store/apps/details?id=com.mobisystems.fileman&hl=en_US&gl=US) which is available in Google Play store.

First, enable developer mode. Scroll over to your profile photo, go into your account settings. Navigate to "System" and then "About", scroll down to and select "Android TV OS build" and then click OK on your remote several times until the toast message says "You are now a developer!".

Second, enable apps from "unknown sources." In setting, navigate to "Apps" and then "Security & Restrictions". Select "Unknown sources". Here, you'll find the File Commander app as a potential source. Click it and, after heeding the warning on the page, flip the toggle to blue in order to enable it to install unknown apps.

Third, transfer your APK to TV. I use the PC File Transfer protocol to transfer an APK from your smartphone or desktop computer to the Chromecast. File Commander will provide a web address for you to access on your smartphone or computer, and you can upload the file directly through your browser.

Then it's time for sideloading! Now, open File Commander and navigate to the location of the APK file. Select the file and choose "Install." When the buttons "Open" and "Done" appear, the app is installed. After installation, you can click "Open" to immediately launch the app.

## Launch Sideload Apps
Sideload Apps may be listed alongside your other apps under the *For You* or *Apps* pages. But sometimes they don't show up. You can find them in *Settings –> Apps*, then select the app from the list and choose "Open."

To make it easier, you can install [Sideload Launcher](https://play.google.com/store/apps/details?id=eu.chainfire.tv.sideloadlauncher&hl=en_US&gl=US), which provides access to all your apps, from Google Play store.

## 下载中文区视频 app
中文区的视频 app，如腾讯视频，芒果TV，并不在 Google Play store 中。所以 sideloading 可以让你在电视上播放中文区的内容。为了避免多次 sideload 应用，我推荐安装[当贝市场](http://www.dangbei.com)然后通过它来安装其它 app。需要注意的是，在安装当贝市场后，你需要允许它安装 apps from unknown sources，只需要重复之前的步骤即可。有的 app 可能支持更新，所以也需要允许它安装 apps from unknown sources。

有的电视 app 需要购买大屏会员。大多数 app 可以播放综艺和电视剧。电影可能会受到版权限制。
