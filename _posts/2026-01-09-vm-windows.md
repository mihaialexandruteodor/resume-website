---
title: Install Virt-Manager for Linux (Nobara, Fedora) 
date: 2026-01-09
categories: [LINUX, TIPS]
tags: [linux, quality-of-life, drawing, graphic-tablet, clip-studio]     # TAG names should always be lowercase
---
### Foreword

If you're like me and want to run some programs that aren't natively supported on Linux (sorry Krita, but you're not there just yet, and you need to fix your lag issue), you can either create a Wine bottle to run the apps through the Wine compatibility layer, or you can just host a Windows VM.

I arrived at the second option after trying to install **Clip Studio Paint** through Bottles, but the whole process was horrible. Not only did I had to install Microsoft Edge as a prerequisite (???), but all tutorials were made for Arch and targeted specific versions of Clip Studio and Wine runners.

That was too much of a headache, so I looked into VM hosts, and first arrived at **WinBoat**. Problem was, Winboat had this issue where Ctrl was stuck on my KDE Plasma Desktop after I shut down the Windows 10 instance no matter what I did, so my final approach was to switch to Virt-Manager.

Below are the install instructions. Hope this helps someone why my niche issue. If not, it will definitely help me in the future.


---

### STEP 1: INSTALL THE NATIVE VIRTUALIZATION STACK
Open your terminal in Nobara and run the following commands to install the industry-standard KVM/QEMU suite:

```zsh
sudo dnf install virt-manager qemu-kvm libvirt virt-install virt-viewer
sudo systemctl enable --now libvirtd
sudo usermod -aG libvirt $USER
```

**NOTE:** You must restart your computer after running these commands for the group permissions to take effect.

---

### STEP 2: HARDWARE-LEVEL TABLET PASSTHROUGH (PREVENTS KEYBOARD LOCKS)
Unlike WinBoat, Virt-Manager allows you to pass your tablet directly to Windows at the hardware level.

1. Open **Virtual Machine Manager**.
2. Create your VM and install Windows 10/11.
3. Before starting the VM, click the **"Lightbulb" icon** (Show virtual hardware details).
4. Click **"Add Hardware"** at the bottom.
5. Select **"USB Host Device"**.
6. Find your Tablet (Wacom, Huion, etc.) in the list and click **Finish**.
7. In the **Display Spice** section, set **"Keyboard Grab"** to **None** or **On Focus**. This ensures Linux always regains control the moment you move your mouse out of the window.

---

### STEP 3: OPTIMIZING CLIP STUDIO PAINT PERFORMANCE
To get "Better than WinBoat" speeds, you must install the VirtIO drivers inside the Windows Guest.

1. Download the VirtIO Win ISO (Stable) from: https://github.com/virtio-win/virtio-win-pkg-scripts
2. Inside the Windows VM, run the "virtio-win-gt-x64.msi" installer.
3. **GPU Acceleration:** In the VM settings (on the host), go to **Video** and set the model to **Virtio**. Check the box for **"3D Acceleration"**.
4. **CPU Speed:** In the **CPU** settings, uncheck "Copy host CPU configuration" and manually type **"host-passthrough"** into the model box. This allows CSP to use your full processor power.

---

### STEP 4: THE "CLEAN EXIT" WORKFLOW

1. When you are finished with Clip Studio Paint, simply close the app.
2. You can either:
   a) Shut down Windows normally (The VM window will close itself safely).
   b) Use the **"Pause"** button in Virt-Manager to "Freeze" the state. This allows you to reopen CSP in 1 second exactly where you left off.
3. If Windows ever hangs, use the **"Force Off"** option in the Virt-Manager menu. Because it doesn't use RDP, your Linux keyboard/mouse will **never** be locked again.

### STEP 5 (optional): Setup for internet and drivers for the VM/Host interaction
  COMPLETE GUIDE: MACVTAP & VIRTIO SETUP FOR WIN10 (NO-LOCK WORKFLOW)

---

### STEP 5.1: IDENTIFY THE HOST INTERFACE
First, we confirm your active internet connection name on Nobara.
1. Open your terminal.
2. Run: `nmcli device status`
3. Look for the "connected" device. 
   **Your result:** `censored_res`

---

### STEP 5.2: CONFIGURE MACVTAP (BYPASS FIREWALL)
This method lets the VM share your physical card to get its own IP address.
1. **Shut down** the Windows VM.
2. Open **Virt-Manager** -> Click the **Lightbulb icon** (Hardware Details).
3. Select **NIC** (Network Interface).
4. **Network source:** Choose **"Host device censored_res: macvtap"**.
5. **Source mode:** Choose **"Bridge"**.
6. **Device model:** Set to **`e1000e`** (Standard Intel driver for initial boot).
7. Click **Apply** and **Start** the VM.

---

### STEP 5.3: INSTALL VIRTIO-WIN DRIVERS
Now we install the high-performance drivers inside Windows.
1. **Mount ISO:** In Virt-Manager, click the **CD icon** -> Browse to your `virtio-win.iso` -> **Apply**.
2. **Inside Windows:**
   - Open **This PC** -> Double-click the **VirtIO-Win CD Drive**.
   - Run **`virtio-win-gt-x64.msi`**.
   - Choose **"Complete"** install and finish.
3. **Reboot Windows.**

---

### STEP 5.4: UPGRADE TO HIGH-SPEED VIRTIO
Now that drivers are installed, we switch to the fastest hardware mode.
1. Shut down the VM.
2. Go to **NIC** hardware settings.
3. Change **Device model** from `e1000e` to **`virtio`**.
4. **Apply** and Start.

---

### STEP 5.5: THE NO-LOCK ART WORKFLOW
- **Input Release:** If the VM ever hangs, press **`Left-Ctrl + Left-Alt`** to free your mouse.
- **Tablet Setup:** Click **Add Hardware** > **USB Host Device** > Select your **Tablet**.
- **Clip Studio Paint:** Open CSP. It will now have internet for assets/license and professional pressure sensitivity without ever locking your Linux keyboard.


---

### SUMMARY OF BENEFITS
* **Zero Input Lag:** Your tablet behaves as if it's plugged directly into a Windows PC.
* **No Stuck Keys:** No more `;6A` or stuck Ctrl/Shift keys.
* **Stable Clipboard:** Copy-pasting between Nobara and Windows works natively via Spice Guest Tools.