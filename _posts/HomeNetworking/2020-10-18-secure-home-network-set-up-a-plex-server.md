---
layout: post
title: Secure Home Network - Set Up a Plex Server
date: '2020-10-18 15:38'
tags:
  - HomeNetwork
---

As I previously mentioned in my [Pi-Hole]({{ site.baseurl }}{% link _posts/HomeNetworking/2019-09-14-secure-home-network-block-ad-with-pi-hole.md %}) and [Home Assistant]({{ site.baseurl }}{% link _posts/HomeNetworking/2020-06-06-secure-home-networking-iot-automation-with-home-assistant.md %}) project, I have an Intel NUC running Ubuntu server in my network closet, I'd like to see what else I can do with it as even a several-year old NUC is too powerful for DNS sinkhole and IoT automation only.

While streaming is pervasive now (and I use the word "pervasive" as I hate paying for many subscriptions just to access a few, and sometimes single content), I still have some media files lying around, mostly in lossless format. It is both annoying to install players that can correctly play them across multiple devices and sync media across them, so I decide to install a [Plex Media Server](https://www.plex.tv/), an awesome and largely free media streaming server.

I like to install software in Docker containers and use Docker Compose to manage them, and Plex offers [official docker image](https://github.com/plexinc/pms-docker). I opted for the `host` mode as I can just use my host network to expose Plex server to my LAN. It is pretty easy to set up:

1. Create a folder for your Plex server, a `config` folder for your configuration, a `temp` folder for transcoding, and a symlink to the location of your media, in my case a [mounted NAS volume]({{ site.baseurl }}{% link _posts/2019-10-20-how-to-mount-wd-mycloud-on-ubuntu-18-04.md %}):
```bash
cd
mkdir plex && cd plex
mkdir config
mkdir temp
ln -s /media/Media/ media
```
2. Create the `docker-compose.yml` file. The official GitHub repo provides a good template to start with:

```yaml
version: '2'
services:
  plex:
    container_name: plex
    image: plexinc/pms-docker
    restart: unless-stopped
    environment:
      - TZ=<your_time_zone>
      - PLEX_CLAIM=<your_claim_token>
      - PLEX_UID=<uid>
      - PLEX_GID=<gid>
    network_mode: host
    volumes:
      - /home/<user_name>/plex/config:/config
      - /home/<user_name>/plex/temp:/transcode
      - /home/<user_name>/plex/media:/data
```
You need to fill in the following:
  * Your timezone, like `Europe/London`;
  * Your claim token so that your server is automatically logged in to your account. You can obtain a claim token to login your server to your plex account by visiting https://www.plex.tv/claim.
  * Your user ID and group ID that owns the `plex` folder. The server is run with the `plex` user within the container but this user is not present outside. Without setting the UID and GID of the user, it may not be able to access mounted `media` folder and thus your content.
  * Volumes for your container mapped to folders created in the previous step.

Start your server with `docker-compose up`. The Plex webpage should be accessible at `<your_server_ip>:32400`. Then, if you add content to your NAS share, it should automatically be indexed by Plex periodically and you will be able to play them anywhere in your home!

# Further Reads
This is the post series. Other posts can be found under [HomeNetwork tag]({{ site.baseurl }}/tags/#HomeNetwork).
