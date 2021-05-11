---
layout: post
title: Automate Docker Container Updates on Synology NAS
date: '2021-01-30 14:13'
tags:
 - NAS
---

Using Docker on Synology NAS is quite straightforward and can be accomplished via a nice web UI. Deploying a new container comes down to a few simple steps: download the image and launch with required parameters. I have quite a few containers running, including
* [Pi-Hole and cloudflared]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}/#installing-pi-hole-on-docker)
* [Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %})
* [HomeBridge]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-17-secure-home-network-make-your-iot-devices-compatible-with-apple-homekit-with-homebridge.md %})

Updating also straightforward: download the latest image, stop your container, clear it, and restart it.But every time the same steps need to be done. There is no "update" button for your images so you need to re-download the image from registry by searching the image and select the "latest" tag. This is tedious and error-prone. Fortunately, there is a simple and elegant way of how to automate the containers update process using [containrrr/Watchtower](https://github.com/containrrr/watchtower):

>Watchtower is an application that will monitor your running Docker containers and watch for changes to the images that those containers were originally started from. If watchtower detects that an image has changed, it will automatically restart the container using the new image.

>With watchtower you can update the running version of your containerized app simply by pushing a new image to the Docker Hub or your own image registry. Watchtower will pull down your new image, gracefully shut down your existing container and restart it with the same options that were used when it was deployed initially.

Watchtower does this using the UNIX domain socket that Docker server listens for commands from Docker CLI. So all you need to do is to map `/var/run/docker.sock` on your host machine into the container. But Synology Docker package doesn't allow to configure such path. As a workaround you need to SSH into the NAS and run Watchtower via CLI:

* Enable SSH access
* Connect to the NAS via SSH
* Run the Watchtower on the NAS via `sudo docker run -d --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --restart unless-stopped`
* For security, disable SSH connections

Then you will see the container in the web UI of your Synology NAS. By default, Watchtower checks for update for the images of all running containers every day. Docker Hub has implemented [rate limiting for pull](https://www.docker.com/blog/scaling-docker-to-serve-millions-more-developers-network-egress/), and the default limit is 100 pulls per 6 hours for anonymous users. If you have many images or containers, you can avoid throttling by increasing the [poll interval](https://containrrr.dev/watchtower/arguments/#poll_interval) using the `WATCHTOWER_POLL_INTERVAL` environment variable. This can be configured and updated via web UI after the container is created. I personally set it to `604800` (7 days).

Because Watchtower is also a running container, it will update itself, too. Thus, it is truly a "fire and forget" maintenance tool.
