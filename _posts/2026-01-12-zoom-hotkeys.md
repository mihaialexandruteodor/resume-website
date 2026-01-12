---
title: ZoomIt Installation on Windows
date: 2026-01-12
categories: [WINDOWS, TIPS]
tags: [windows, quality-of-life, zoom]     # TAG names should always be lowercase
---

I liked the zoom functionality in KDE plasma, so I wanted something similar in Windows. Fortunately, I found ZoomIt, a very powerful zoom tool. Here are the steps to install it:

### ZoomIt Installation and Setup Guide

ZoomIt is a standalone tool from Microsoft Sysinternals. It does not require a traditional installation process, making it very lightweight and easy to remove.

1. **Download ZoomIt:**
   * Go to the official [Microsoft Sysinternals ZoomIt page](https://learn.microsoft.com/en-us/sysinternals/downloads/zoomit).
   * Click **Download ZoomIt** to save the `ZoomIt.zip` file to your computer.

2. **Extract and Run:**
   * Right-click the downloaded `.zip` file and select **Extract All**.
   * Open the extracted folder and double-click `ZoomIt64.exe` (for 64-bit Windows).
   * Click **Agree** on the license terms window.

3. **Configure Settings and Shortcuts:**
   * **General Tab:** Check **Run ZoomIt when Windows starts** so your shortcuts work every time you reboot.
   * **Standard Zoom (`Ctrl` + `1`):** This freezes the screen and zooms in. You can use the mouse wheel to change magnification and left-click to start drawing.
   * **Draw Mode (`Ctrl` + `2`):** This allows you to draw on the screen at the current zoom level without magnification.
   * **Live Zoom (`Ctrl` + `4`):** This is the "Linux-style" mode. It keeps windows active so you can still click and type while zoomed. You can use the **Mouse Wheel** to zoom in and out smoothly in this mode.
   * Click **OK** to save and minimize to the system tray.

4. **Usage Instructions:**
   * **To Start/Stop Zoom:** Press your chosen hotkey (e.g., `Ctrl` + `4` for Live Zoom).
   * **To Zoom In/Out:** Use the **Mouse Wheel** while any zoom mode is active.
   * **To Pan:** Simply move your mouse; the screen will follow your cursor smoothly.
   * **To Exit:** Press `Esc` or the hotkey again.

5. **How to Uninstall:**
   * To stop ZoomIt: Right-click the ZoomIt icon in the System Tray (near the clock) and select **Exit**.
   * To delete: Simply delete the `ZoomIt64.exe` file and the extracted folder.