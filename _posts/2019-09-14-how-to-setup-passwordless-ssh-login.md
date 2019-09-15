---
layout: "post"
title: "How to Setup Passwordless SSH Login"
date: "2019-09-14 21:51"
tags:
 - Linux
---

Secure Shell (SSH) is a cryptographic network protocol used for secure connection between a client and a server and supports various authentication mechanisms. The two most popular mechanisms are passwords based authentication and public key based authentication.

To set up a passwordless SSH login in Linux all you need to do is to generate a public authentication key and append it to the remote hosts `~/.ssh/authorized_keys` file.

# Generate an SSH Keypair
Before generating a new SSH keypair, you should check if there is an existing one. You may already have an SSH key to use with Git repo such as GitHub.

```bash
ls -al ~/.ssh/*.pub
```
If you see `No such file or directory` or `no matches found` it means that you do not have an SSH key and you can proceed with the next step and generate a new one.

The following command will generate a new 4096 bits SSH key pair with your email address as a comment:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@domain.com"
```

Press Enter to accept the default file location and file name:

```bash
Enter file in which to save the key (/home/yourusername/.ssh/id_rsa):
```

Next, the `ssh-keygen` tool will ask you to type a secure passphrase. In most cases, developers and system administrators use SSH without a passphrase because they are useful for fully automated processes. If you choose to use passphrase you will get an extra layer of security. If you don’t want to use passphrase just press `Enter`

```bash
Enter passphrase (empty for no passphrase):
```

To be sure that the SSH keys are generated you can list your new private and public keys with:

```bash
ls ~/.ssh/id_*
/home/yourusername/.ssh/id_rsa /home/yourusername/.ssh/id_rsa.pub
```

# Copy the Public Key to Remote Host

The easiest way to copy your public key to your server is to use a command called `ssh-copy-id`. On your local machine terminal type:

```bash
ssh-copy-id remote_username@server_ip_address
```

You will be prompted to enter the remote_username password. Once the user is authenticated, the public key will be appended to the remote user `authorized_keys` file and connection will be closed.

If by some reason the `ssh-copy-id` utility is not available on your local computer you can use the following command to copy the public key:

```bash
cat ~/.ssh/id_rsa.pub | ssh remote_username@server_ip_address "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

# Verify
After completing the steps above you should be able log in to the remote server without being prompted for a password.

To test it just try to login to your server via SSH:
```bash
ssh remote_username@server_ip_address
```

If everything went well, you will be logged in immediately.
