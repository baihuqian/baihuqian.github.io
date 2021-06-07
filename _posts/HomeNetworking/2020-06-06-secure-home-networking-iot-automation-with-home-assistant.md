---
layout: post
title: 'Secure Home Networking: IoT Automation with Home Assistant'
date: '2020-06-06 11:13'
tags:
  - HomeNetwork
  - IoT
---

This post introduces [Home Assistant](https://www.home-assistant.io/), an open-source home IoT automation that puts local control and privacy first. it also helps address many IoT platform by providing a cross-vendor management interface.

I by no means have a large collection of home automation or IoT toys. But between Apple HomeKit, Amazon Alexa, and Google Assistant, it is sometimes annoying to find something works on one platform but not so much on the other. In addition, automation services such as IFTTT often have limitations imposed by vendors on what can be controlled by automation. Therefore, I start to use Home Assistant as the main IoT management and automation system.

# Install with Docker and Docker Compose
Since I have an Intel NUC running Ubuntu server which is also used in my [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}) project, I install Home Assistant using [Docker and Docker Compose](https://www.home-assistant.io/docs/installation/docker/#docker-compose):

```yaml
version: '3'
services:
  homeassistant:
    container_name: home-assistant
    image: homeassistant/home-assistant:stable
    volumes:
      - /home/ubuntu/home-assitant/config:/config
    environment:
      - TZ=America/Chicago
    restart: always
    network_mode: host
```

Then start the container with:

```bash
docker-compose up -d
```

To restart Home Assistant when you have changed configuration:

```bash
docker-compose restart
```

Note, to automatically start Docker on boot:
```bash
sudo systemctl enable docker
```

The Home Assistant container is configured to restart automatically when docker engine starts up, so there is no need to start up on boot.

# Install on Synology NAS
Synology NAS supports Docker with a nice UI. Home Assistant provides [a comprehensive guide on setting it up on Synology NAS](https://www.home-assistant.io/docs/installation/docker/#synology-nas). I find it painless to set up.

Home Assistant's `stable` image is updated very frequently. To avoid manually updating them, you can [run a docker container to automate the update process]({{ site.baseurl }}{% link _posts/2021-01-30-automate-docker-container-updates-on-synology-nas.md %}).

# Check Configuration
Home Assistant UI is useful but sometimes limited. Sometimes its configuration file, located in `/home/ubuntu/home-assitant/config`, must be manually edited. The following command can verify the integrity of the configuration files after editing.

```bash
docker exec home-assistant python -m homeassistant --script check_config --config /config
```

Then restart Home Assistant to use the updated configuration.

# Upgrade Home Assistant
Use `docker-compose pull` to get the latest docker image, and restart after `pull` is complete.

```bash
docker-compose pull
docker-compose restart
```

# Configure IoT Devices
Home Assistant is developed and maintained by community. While many devices can be configured through its web UI, many still requires configurations through its YAML files. For example, I have many [Yeelight](https://www.home-assistant.io/integrations/yeelight/) bulbs, but it must be configured through YAML. It requires configuring static IPs for light bulbs:

```yaml
yeelight:
  devices:
    192.168.20.5:
      name: Bedroom Color
      model: color2
    192.168.20.6:
      name: Floor Lamp
    192.168.20.7:
      name: Hallway
    192.168.20.8:
      name: Second Room
    192.168.20.9:
      name: Lamp
      model: lamp1
```

Always validate the configuration before restarting your Home Assistant!

Home Assistant is limited by what vendors allow for third-party integration. For example, Google no longer allows new third-party integration with [Nest](https://www.home-assistant.io/integrations/nest/) devices, and you cannot configure Home Assistant to control it if you don't have a developer account with Nest before the cut-off date.

Now there are more and more integrations that can be configured via web UI, making it closer to the main stream users.

# Work with HomeKit
Home Assistant has native integration for HomeKit. In fact, it support HomeKit in two directions:
1. It's able to integrate IoT devices that support "Work with HomeKit" via the [HomeKit Controller](https://www.home-assistant.io/integrations/homekit_controller/) integration. This is useful to include HomeKit-enabled devices in Home Assistant.
2. it's able to expose devices in Home Assistant to HomeKit by acting as a bridge using the [HomeKit](https://www.home-assistant.io/integrations/homekit/) integration.

Because Home Assistant can have way more entities, I find it helpful to selectly expose devices to HomeKit with explicit "include". This way I can declutter the Home app on my iPhone while maintaining a performant integration.

I personally connects all "Work with HomeKit" devices to Home Assistant, then expose them to Home app. This allows me to have dual-mode control using both the Home app and Home Assistant. It also helps including devices that are tricky to onboard to Home Assistant, such as Ecobee (requires a developer account).

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
