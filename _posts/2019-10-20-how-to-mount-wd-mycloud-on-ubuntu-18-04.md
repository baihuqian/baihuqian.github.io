---
layout: "post"
title: "How to Mount WD MyCloud on Ubuntu 18.04"
date: "2019-10-20 19:44"
tags:
 - Linux
 - Networking
---

Although WD MyCloud does not officially support Linux, it uses Samba for network mount. This post will explain how to mount a password-protected WD MyCloud share to Ubuntu 18.04 and enable automatic mount at start up.

**Note**: This solution works for any network attached storage (NAS) that supports Samba. For example, it works for [my Synology NAS when I migrated to it]({{ site.baseurl }}{% link _posts/HomeNetworking/2021-01-01-secure-home-network-build-your-own-private-cloud-with-nas.md %}).

## Install CIFS
You will need to install CIFS (Common Internet File System) in your Linux system, if it is not already installed:

```bash
sudo apt install cifs-utils
```

## Create Mount Point for Your Shares
You will need a separate directory for each share. You can create them under `/media` or `/mnt`.

```bash
sudo mkdir /media/<share_name>
```

`<share_name>` is the name of your share.

## Create Password File
The configuration of your network mount is in `/etc/fstab` file. Because it is readable by everyone and so is your password in it, you should use a credentials file. This is a file that contains just the username and password to your WD MyCloud.

```
vim ~/.smbcredentials
```

Enter your username and password in the file:

```
username=msusername
password=mspassword
```

Save and exit vim. By default, this file can be read by your user but not other users.

## Edit Mount File
Edit `/etc/fstab` with root privileges:

```bash
sudo vim /etc/fstab
```

And add this line for each share:

```
//<server_name>/<share_name>  /media/<share_name>  cifs  uid=<user_name>,credentials=/home/<user_name>/.smbcredentials,iocharset=utf8 0 0
```

This line refers to your previously-created password file, and enable special permissions (like `chmod` etc.) to your mount.

As an example, I have a WD MyCloud EX2 which a static IP address of 192.168.10.10, and I would like to mount a shared named "Archive". My Ubuntu username is `ubuntu` and the password file is thus located at `/home/ubuntu/.smbcredentials`. I added the following line to `/etc/fstab`:

```
//192.168.10.10/Archive /media/Archive cifs uid=ubuntu,credentials=/home/ubuntu/.smbcredentials,iocharset=utf8 0 0
```

## Mount!
Test the `fstab` entry by issuing:

```bash
sudo mount -a
```

If there are no errors, you should test how it works after a reboot. Your remote share should mount automatically.

## Reference
* [Mount Windows Shares Permanently](https://wiki.ubuntu.com/MountWindowsSharesPermanently)
