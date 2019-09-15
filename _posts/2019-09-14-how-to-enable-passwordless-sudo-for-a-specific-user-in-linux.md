---
layout: "post"
title: "How To Enable Passwordless Sudo For A Specific User in Linux"
date: "2019-09-14 21:17"
tags:
 - Linux
---

This can be configured with the `sudoers` file in `/etc/sudoers`. However, it is not recommended to edit this file directly. Instead, as of Debian version 1.7.2p1-1, the default `/etc/sudoers` file now includes the directive:

```bash
#includedir /etc/sudoers.d
```

This will cause sudo to read and parse any files in the `/etc/sudoers.d` directory that do not end in '`~`' or contain a '`.`' character.

Note that there must be at least one file in the `sudoers.d` directory (there should be a `README` file by default), and all files in this directory should be mode 0440.

Thus, configuring a user with passwordless sudo can be done in 3 steps:

1. Create a file file in the `/etc/sudoers.d` directory:
```bash
sudo vim /etc/sudoers.d/<user>
```
3. Add: `<user> ALL=(ALL) NOPASSWD: ALL` , where `<user>` is your passwordless sudo username;
3. Change the permission of the file to be mode 0440:
```bash
sudo chmod 0440 /etc/sudoers.d/<user>
```

From now on, your `<user>` will be able to execute `sudo any_command` without providing a password.
