---
title: "Setting Up a Headless Raspberry Pi 4 Server"
date: 2025-04-29T17:48:00+02:00
draft: false
---

## Introduction

This guide provides a step-by-step process for setting up a Raspberry Pi 4 as a headless server, meaning it will operate
without a connected monitor, keyboard, or mouse.

## Prerequisites

- Raspberry Pi 4 Model B
- MicroSD card (16GB or larger recommended)
- Power supply for Raspberry Pi 4
- Computer with an SD card reader
- Ethernet cable or Wi-Fi network credentials
- SSH client on your computer (e.g., PuTTY for Windows, Terminal for macOS/Linux)

## Step 1: Download Raspberry Pi OS Lite

1. Go to the official Raspberry Pi
   website: [https://www.raspberrypi.com/software/operating-systems/](https://www.raspberrypi.com/software/operating-systems/)
2. Download the "Raspberry Pi OS Lite" image. This version doesn't include a desktop environment, which is ideal for a
   server.

## Step 2: Flash the OS Image to the MicroSD Card

1. Download and install the Raspberry Pi
   Imager: [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/)
2. Insert the MicroSD card into your computer's SD card reader.
3. Open the Raspberry Pi Imager.
4. Click "CHOOSE OS" and select the downloaded Raspberry Pi OS Lite image file (`.img` or `.zip`).
5. Click "CHOOSE STORAGE" and select your MicroSD card. **Warning:** This will erase all data on the card.
6. Click "WRITE" and wait for the process to complete.

## Step 3: Enable SSH for Headless Setup

1. Once the flashing is complete, **do not eject the MicroSD card yet**.
2. Open the file explorer and navigate to the boot partition of the MicroSD card (it should be labeled "boot" or
   similar).
3. Create an empty file named `ssh` (no file extension) in the root directory of this boot partition. This enables the
   SSH server on the first boot.

## Step 4: Configure Wi-Fi (Optional, if not using Ethernet)

1. If you plan to use Wi-Fi, create another file named `wpa_supplicant.conf` in the root directory of the boot
   partition.
2. Open this file with a plain text editor and add the following content, replacing `"Your_SSID"` and `"Your_Password"`
   with your actual Wi-Fi network name and password:

   ```conf
   country=US # Change to your 2-letter ISO country code (e.g., GB, DE, FR)
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1

   network={
       ssid="Your_SSID"
       psk="Your_Password"
   }
   ```

3. Save the file.

## Step 5: Boot the Raspberry Pi

1. Safely eject the MicroSD card from your computer.
2. Insert the MicroSD card into your Raspberry Pi 4.
3. Connect the Ethernet cable (if not using Wi-Fi).
4. Connect the power supply to boot the Raspberry Pi. Wait a few minutes for it to fully boot up.

## Step 6: Find the Raspberry Pi's IP Address

There are several ways to find the IP address:

- **Router Interface:** Log in to your router's web interface and look for connected devices. The Raspberry Pi might
  appear with the hostname "raspberrypi".
- **Network Scanner:** Use a network scanning tool (like `nmap` on Linux/macOS or "Advanced IP Scanner" on Windows) to
  scan your network for devices.
- **Bonjour/Avahi:** If your computer supports it, you might be able to connect using the hostname `raspberrypi.local`.

## Step 7: Connect via SSH

1. Open your SSH client (Terminal or PuTTY).
2. Use the following command, replacing `<IP_Address>` with the IP address you found:

   ```bash
   ssh pi@<IP_Address>
   ```

   Or, if using the hostname:

   ```bash
   ssh pi@raspberrypi.local
   ```

3. You might see a security warning about the host key. Type `yes` to continue.
4. When prompted for the password, the default password is `raspberry`.
5. You should now be logged into your Raspberry Pi's command line!

## Step 8: Initial Configuration (Important!)

1. **Change the Default Password:** Immediately change the default password for security:

   ```bash
   passwd
   ```

   Follow the prompts to set a new, strong password.

2. **Update System Packages:** Ensure your system is up-to-date:

   ```bash
   sudo apt update
   sudo apt full-upgrade -y
   ```

3. **Configure Raspberry Pi Settings (Optional):** Run the configuration tool:

   ```bash
   sudo raspi-config
   ```

   Here you can configure options like:

   - Localization (Timezone, Keyboard Layout)
   - Network Options (Hostname)
   - Interfacing Options (Enable VNC, SPI, I2C if needed)
   - Advanced Options (Expand Filesystem - usually done automatically now)
     Select "Finish" when done. You might need to reboot (`sudo reboot`).

## Conclusion

Your Raspberry Pi 4 is now set up as a headless server! You can disconnect the Ethernet cable if you configured Wi-Fi
and continue accessing it via SSH over your network. You can now install server software like web servers (Apache,
Nginx), file servers (Samba), media servers (Plex), or use it for various projects.
