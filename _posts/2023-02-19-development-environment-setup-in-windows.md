---
layout: "post"
title: "Development Environment Set Up in Windows"
date: "2023-02-19 09:00:00"
---

I have been a long-time Mac user. Mac has been by development environment for pretty much all my professional life. I like its aesetics, its Unix-like environment, its native support for many things that can greatly improve the quality of life (like preview for PDF and raw files, iMessage, and FaceTime).

My 2015 Macbook Pro has been a great machine for me, but now its 2-core i5 CPU is showing its limitation. The M1/M2 ARM Macbooks, both Air and Pro, are great, but I really don't take my laptop out of my home these days. And I have a 10-year old Windows PC (i7-4770) for gaming and other things that are still going strong. Maybe its time to give Windows another try?

In the old days, a developer would create a dual boot with a dedicated Linux system. It feels too much trouble these days as configuring Linux takes a lot of time. Windows subsystem for linux (WSL) are around for a while. I tried it when it initially came out, and the experience is lackluster. Maybe things are better now, given Microsoft has released a slew of developer-friendly tools and owns GitHub?

The short answer is yes. WSL now brings the best of both Windows and Linux worlds: the ease of use and support for Windows and the better developer experience of Linux.

## Set Up

First of all, everyone should install [Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-us&gl=us&rtc=1). It's a better, integrated terminal than the old school command prompt (can Microsoft update its font?).

Installing WSL is easier than ever: it's available in Windows store. Installing it from Windows store also automates away the upgrade of it. Then, a simple command to install your favorate Linux distro: `wsl --install Ubuntu`.

Windows terminal automatically detects it and creates an option for it, without going to the Ubuntu app. So effectively, you have a native Linux terminal right in your Windows. Make sure you change the default profile to your Linux distro!

Writing code requires a few things: 1) a great editor/IDE, 2) Git, 3) and development and runtime environment.

Microsoft owns VS Code, and it has built a native integration with WSL. By using a server-client architecture where Linux is the server and Windows is the client, it enables opening up a project in Linux file system in Windows' VS Code, which supports all the usual features like source control, run and debug, and extensions. What's better, by installing VS Code on your Linux's path, you can run `code .` to open the current directory in VS Code! [Setting up VS Code with WSL is well-documented](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode), too.

Git can be set up in multiple ways, but the major decision is whether you would need to use Git in Windows. If not, then Git comes with most Linux distro and you can set it up the usual way (for example, with [GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)). However, if you do use Git in Windows and want to share credentials with Linux, then the WSL's recommended [credential manager](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git#git-credential-manager-setup) can help you. Personally I don't.

My developer workflow would be to store projects in Linux's file system, use Git in Linux, and use `code .` from the project root to open it in VS code. Honestly, this workflow feels very natural. As a bonus, Linux and Windows' file systems are connected, so it is possible to run scripts or apps in Linux against files on Windows or use Windows File Explorer to browse Linux. Windows file system is mounted under `/mnt/c`, and file explorer has `Linux` tab (as if it's a remote server, more client-server architecture) to browse Linux file system directly. Network locations mounted in Linux could be mounted in Linux either use the DrvFs file system, or directly mount them in Linux (like mounting SMB share). To mount DrvFs:

```bash
$ sudo mkdir /mnt/y
$ sudo mount -t drvfs Y: /mnt/y
```

Setting up the runtime environment is identical to your Linux distro. It's a bit of learning for me since I have not configured a major Linux distro for a long time.

Install ruby is done via `sudo apt install ruby-full`. Install [`rbenv`](https://phoenixnap.com/kb/install-ruby-ubuntu#ftoc-heading-2) to better manage ruby and gem installations.

Python is installed by default. Install pip via `sudo apt install python3-pip` and then install virtualenv.

Ubuntu comes with Bash as the default shell. Change to zsh, install `oh-my-zsh` are simple. Autojump can be installed via `apt install autojump`. Source `. /usr/share/autojump/autojump.sh` to activate it.

## Conclusion

WSL has the right tooling and support to create an easy-to-use developer environment. WSL2 comes with a dedicated Linux kernel vs. a translation in WSL1, so the support for better performance, more Linux features required to run Docker and support CUDA, bring a more complete experience. I'm satisfied with WSL as a light development environment. In fact, I'm writing this blog post using my Windows machine, Ubuntu subsystem, and VS code, and it feels as smooth as my old Mac environment.

## Reference

* [Installation of WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
* [File System Improvements to the Windows Subsystem for Linux](https://learn.microsoft.com/en-us/archive/blogs/wsl/file-system-improvements-to-the-windows-subsystem-for-linux)
