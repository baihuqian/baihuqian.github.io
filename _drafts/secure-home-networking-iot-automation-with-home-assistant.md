---
layout: "post"
title: "Secure Home Networking: IoT Automation with Home Assistant"
date: "2020-04-11 15:23"
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

# Check Configuration
Home Assistant UI is useful but sometimes limited. Sometimes its configuration file, located in `/home/ubuntu/home-assitant/config`, must be manually edited. The following command can verify the integrity of the configuration files after editing.

```bash
docker exec home-assistant python -m homeassistant --script check_config --config /config
```

Then restart Home Assistant to use the updated configuration.
