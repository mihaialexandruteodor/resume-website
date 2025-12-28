---
title: Zoomed in GUI for Flowblade on 4k Monitors 
date: 2025-12-28
categories: [LINUX, TIPS]
tags: [linux, quality-of-life, flowblade]     # TAG names should always be lowercase
---

If you use KDE Plasma desktop environment and a 4k monitor like me, you'll notice that you can't zoom in on your GUI. Here's a single command that will fix this at shortcut level.

```zsh
sudo sed -i 's|^Exec=.*|Exec=env SDL_VIDEODRIVER=dummy GDK_BACKEND=x11 GDK_SCALE=2 flowblade --disable-gpu-test %F|' /usr/share/applications/io.github.jliljebl.Flowblade.desktop
```

> This might not be the proper fix for you, depending on your desktop environment (I use KDE Plasma 6.5.4), this is more of a backup of my fix. Use with caution.
{: .prompt-warning }