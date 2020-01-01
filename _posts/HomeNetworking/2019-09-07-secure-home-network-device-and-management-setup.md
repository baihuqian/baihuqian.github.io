---
layout: "post"
title: "Secure Home Network - Device and Management Setup"
date: "2019-09-07 19:29"
tags:
 - Networking
---

Internet connectivity, especially through wireless network, is now a critical utility in our homes. Setting up reliable and versatile wireless network and making it secure is a crucial step at move-in time.

## Choice of Device

Being a professional working in the networking space, I prefer buying low-cost enterprise-grade network gear and do it myself. They are far more reliable, energy efficient, and provide better signal strength than commercial all-in-ones. Typically, I buy a 4- or 8-port router and one or two Access Points (AP).

The capability of remote management is important to me. Not only does it allow me to manage my parents' network without going there, but also it provides monitoring and troubleshooting tools to detect ISP failures. For this reason, I use networking devices from Ubiquiti. Specifically, I use their EdgeRouter devices and UniFi AP.

You might ask, EdgeRouter and UniFi are two different line of products. Should UniFi Security Gateway a better choice than EdgeRouter? The answer is cost and feature completeness. USG costs $129 and EdgerRouter-X (4 ports) costs $60 on B&H, and EdgeRouter comes with the full EdgeOS which is a full-blown router OS. In addition, USG comes with only 2 LAN ports so you need to buy a UniFi switch, which comes at about another $100, where EdgeRouter comes with different configurations with different LAN ports.

## Management Setup
I'd like to run management software on EC2 instances on AWS, simply because it is easier and stable than running a server behind an ever-changing public IP address at home.

To run UniFi controller and UNMS (for EdgeRouter), a m5.large instance type with 16 GB of EBS volume is recommended. You can always resize the instance and EBS volume at any time.

To install UniFi controller on AWS, refer to [this guide](https://help.ubnt.com/hc/en-us/articles/209376117-UniFi-Install-a-UniFi-Cloud-Controller-on-Amazon-Web-Services).

To install UNMS (Ubiquiti Network Management System) on the same instance, open the Security Group for port 80, 81 and 443, and follow [this guide](https://help.ubnt.com/hc/en-us/articles/115012196527). For more information about UNMS, [this GitHub wiki](https://github.com/Ubiquiti-App/UNMS/wiki) contains links to many help pages.


## Adopt UniFi Devices
Adopting UniFi APs from a remote controller (those not in the same network as APs) can be challenging. Refer to [this guide](https://help.ubnt.com/hc/en-us/articles/204909754-UniFi-Device-Adoption-Methods-for-Remote-UniFi-Controllers) on instructions on how to discover devices. I recommend using the Discovery tool plugin for Chrome (freely available in [Chrome Web Store](https://chrome.google.com/webstore/detail/ubiquiti-device-discovery/hmpigflbjeapnknladcfphgkemopofig?hl=en)) using a laptop.

1. Connect your computer to the same network as the AP. For example, connect it to the EdgeRouter via an ethernet cable.
2. Install and open the Discovery tool plugin.
3. Click "UNIFI FAMILY" if it is not enabled. When not enabled, it should show your EdgeRouter.
4. Your AP should show as "Pending" in the list. Click Action.
5. Use "Set Inform" to tell the AP the public IP address of the UniFi controller. Fill the address as `http://XYZ:8080/inform` where `XYZ` is the public IP of your controller.
6. Once the Set Inform is complete, the AP should show up in your controller as "Pending Adoption".
7. **Important: if your device has been reset, you need to Set Inform twice. Otherwise, the controller will not be able to adopt the AP.**
8. Click "Adopt" under Actions. It may be "Adopt and Update" if critical updates should be installed for your AP.
9. Your AP should blink and restart with the site's settings applied to it when its state becomes "Connected".

## Adopt EdgeRouters
Adopting EdgeRouters from remote controller can be done by adding the UNMS key to the device's configuration. [this guide](https://help.ubnt.com/hc/en-us/articles/115015772548-UNMS-The-UNMS-Key-and-the-Device-Registration-Process#3) provides useful information:

1. Open UNMS and go to the **Devices** section.
2. Click the **ADD DEVICE** button.
3. Copy the UNMS key (it is the same UNMS key for all devices).
4. Open the device's administration page.
5. Go to the **System** or **Services** section.
6. Paste the UNMS key.
7. Enable the UNMS connection.
8. Save the device configuration.
9. Authorize the device in the UNMS devices list.

Once the devices are adopted by the controller, you can configure them through the controller over the public internet. You don't have to be in the same network as the device in order to configure it.
