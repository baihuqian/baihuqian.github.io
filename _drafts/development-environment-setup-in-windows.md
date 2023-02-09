---
layout: "post"
title: "Development Environment Set Up in Windows"
date: "2023-02-01 09:00:00"
---

Install WSL2: `wsl --install Ubuntu`: https://learn.microsoft.com/en-us/windows/wsl/install

VS Code: https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode , the remote extension is useful. `code .` to open editor is neat.

Git: dont' use credential manager. Set up GitHub SSH in the usual way: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent . Git is pre-installed from Ubuntu and we don't need it for Windows.
 
Install ruby: `sudo apt install ruby-full` and `rbenv`: https://phoenixnap.com/kb/install-ruby-ubuntu#ftoc-heading-2

Windows file system is mounted under `/mnt/c`, and file explorer has `Linux` tab to browse Linux file system directly.

For development, store the files in Linux (from `git clone`) and use `open .` from the project root to upen it in VS Code.

Autojump: install via `apt install autojump` and source `. /usr/share/autojump/autojump.sh` to activate.

Python is installed by default. Install pip:  `sudo apt install python3-pip`