---
title: Setting up a laptop as a server
description: Notes from setting up an old laptop as a home server
date: 2024-09-21
tags:
  - homelab
  - ubuntu
extraWideMedia: false
opengraph:
  image: /assets/images/article.png
---

I've set up an old laptop as a home server and wanted to write up some of the configuration and setup steps for future reference.

It's an old Lenovo ThinkPad X121e laptop from 2011 on which I've installed Ubuntu server 24.04. There are just a couple of configuration tweaks I wanted to make.

I'll also try to remember to add anything else I needed to adjust from the defaults to get things working how I need them to.

## Lid closure behaviour

### Don't sleep when the lid is closed


The nice thing about using a laptop as a server is that problems connecting via SSH can be easily overcome by using the in-built screen and keyboard!
But most of the time the machine is sitting with the lid closed on my desk. I didn't want it to go to sleep when I shut the lid. ([Source](https://askubuntu.com/questions/15520/how-can-i-tell-ubuntu-to-do-nothing-when-i-close-my-laptop-lid))

```bash
# Add or edit to /etc/systemd/logind.conf (needs sudo)
HandleLidSwitch=ignore
```

Then `sudo service systemd_logind restart`

### Switching the screen off when not in use


Similarly, I don't want the screen to stay on all the time when the lid is usually closed. ([Source](https://askubuntu.com/a/1076734))

```bash
# Add to /etc/default/grub (needs sudo)
GRUB_CMDLINE_LINUX_DEFAULT="consoleblank=60"
```
