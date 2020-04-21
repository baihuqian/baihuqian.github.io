---
layout: "post"
title: "How to Mount WD MyCloud on Ubuntu 18.04"
date: "2019-10-20 19:44"
tags:
 - Linux
 - Networking
---

Although WD MyCloud does not officially support Linux, it uses Samba for network mount. This post will explain how to mount a password-protected Samba share to Ubuntu 18.04 and enable automatic mount at start up.

## Install CIFS
```bash
sudo apt install cifs-utils
```

## Create Mount Point
You will need a separate directory for each mount. You can create them under `/media` or `/mnt`.

```bash
sudo mkdir /media/<share_name>
```

## Create Password File
Because `/etc/fstab` is readable by everyone and so is your password in it, you should use a credentials file. This is a file that contains just the username and password to your WD MyCloud.

```
vim ~/.smbcredentials
```

Enter your username and password in the file:

```
username=msusername
password=mspassword
```

Save and exit vim.

## Edit Mount File
Edit `/etc/fstab` with root privileges:

```bash
sudo vim /etc/fstab
```

And add this line for each mount:

```
//<server_name>/<share_name>  /media/<share_name>  cifs  uid=<user_name>,credentials=/home/<user_name>/.smbcredentials,iocharset=utf8 0 0
```

This line refers to your previously-created password file, and enable special permissions (like `chmod` etc.) to your mount.

For my mount, I added the following line to `/etc/fstab`:

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
