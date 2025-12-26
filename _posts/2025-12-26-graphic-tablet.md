---
title: XP-PEN Deco 01v2 Setup Guide for Linux Nobara 
date: 2025-12-26
categories: [LINUX, TIPS]
tags: [linux, quality-of-life, drawing, graphic-tablet]     # TAG names should always be lowercase
---



This guide covers the installation of OpenTabletDriver (OTD) and the specific fixes for the Deco 01v2's scaling and button issues on Wayland.

---

## 1. Install .NET Dependencies

Nobara is Fedora-based, so use dnf to install the required .NET runtime:

```zsh
sudo dnf install dotnet-runtime-8.0
```

---

## 2. Download and Extract OpenTabletDriver

1. Go to: <https://github.com/OpenTabletDriver/OpenTabletDriver/releases>
2. Download the 'OpenTabletDriver.Linux.x64.tar.gz' file.
3. Extract the folder to your Home directory: ~/OpenTabletDriver

```zsh
tar -xzf ~/Downloads/opentabletdriver-*.tar.gz -C ~/
```

---

## 3. Set Hardware Permissions (Udev Rules)

Run these commands to allow the driver to talk to your tablet hardware:

```zsh
cd ~/OpenTabletDriver
sudo cp 99-opentabletdriver.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules && sudo udevadm trigger
```

---

## 4. Running the Driver

You must run the Daemon first (the engine), then the UX (the settings interface).

To start the Daemon:

```zsh
~/OpenTabletDriver/OpenTabletDriver.Daemon
```

To start the Settings GUI (run this in a separate terminal window):

```zsh
~/OpenTabletDriver/OpenTabletDriver.UX.Gtk
```

---

## 5. Specific Fixes for XP-PEN Deco 01v2

**A. Fixing the "Double Size" Scaling Issue:**
The Deco 01v2 often reports dimensions twice as large as the physical surface.

1. In the OTD GUI, go to the 'Display / Area' tab.
2. Manually divide the 'Width' and 'Height' values by 2.
3. Click 'Apply' and 'Save'.

**B. Fixing the Side Buttons (Backport Plugin):**

1. In the OTD GUI, open the 'Plugin Manager'.
2. Search for and install the 'OTD Backport' plugin.
3. If buttons still don't work, copy the configurations manually:

```zsh
cp -r ~/.config/OpenTabletDriver/Plugins/Configurations/* ~/OpenTabletDriver/Configurations/
```

---

## 6. Enable Autostart on Boot

1. Open the 'Startup Applications' utility in Nobara.
2. Add a new startup program:
   - Name: OpenTabletDriver Daemon
   - Command: /home/YOUR_USERNAME/OpenTabletDriver/OpenTabletDriver.Daemon
   (Replace YOUR_USERNAME with your actual Linux user name)
3. Save and close.

---

## 7. Wayland Support

If you are using Nobara's default Wayland session, ensure the 'Output Mode' in the OTD settings is set to 'Libevdev' or 'Wayland' to ensure proper pen mapping and pressure sensitivity.

## 8. Add a Desktop Shortcut
To launch the settings GUI from your app menu without using the terminal:

1. Create a new desktop file:

```zsh
nano ~/.local/share/applications/opentabletdriver.desktop
```

2. Paste the following content into the file (Replace YOUR_USERNAME with your actual username):

```zsh
[Desktop Entry]
Type=Application
Name=OpenTabletDriver
Comment=Graphic Tablet Configuration
Exec=/home/YOUR_USERNAME/OpenTabletDriver/OpenTabletDriver.UX.Gtk
Path=/home/YOUR_USERNAME/OpenTabletDriver/
Icon=input-tablet
Terminal=false
Categories=Graphics;Settings;
```

3. Press CTRL + O, then Enter to save, and CTRL + X to exit. The "OpenTabletDriver" icon will now appear in your application launcher.