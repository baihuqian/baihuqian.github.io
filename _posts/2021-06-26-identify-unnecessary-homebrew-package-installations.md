---
layout: post
title: Identify Unnecessary Homebrew Package Installations
date: '2021-06-26 10:57'
---

As a Mac user, [Homebrew](https://brew.sh) is a must-have to install development packages and keep them up-to-date. And with [Homebrew Cask](https://github.com/Homebrew/homebrew-cask), it can install and update GUI applications, too. However, after a few years of usage and frequent updates, the number of packages installed has increased a lot. Some are due to added dependencies of installed packages, and some are left-over dependencies from packages that are no longer required.

To clean up the Homebrew installation, we need to identify unused packages that are not part of the dependency tree of other packages. Brew provides a few commands to list packages and dependencies:

* `brew list` returns all installed packages;
* `brew deps --installed` returns the dependencies for all installed packages, and `brew deps --installed --tree` shows the dependencies in a nicer tree structure;
* `brew leaves` returns installed packages that are not dependencies of another installed package.

Therefore, packages that are part of `brew leaves` but were not installed explicitly by us can be removed by `brew uninstall`. As we remove packages, more leaf packages can surface, so make sure to repeatedly run `brew leaves` after each uninstallation with `brew uninstall` until there is no more unwanted leaf packages.
