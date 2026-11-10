# Installing Oreon

This guide is a step-by-step walkthrough of installing Oreon 10 using a USB drive.

## Before you start

Before you begin, please back up your files to an external drive or cloud service, as installing Oreon may erase all of your files.

## What you need

- A PC/laptop with a modern (Haswell+ for Intel, or Ryzen+ for AMD) processor.
- A USB stick with at least 2.5 GB of space (4 GB recommended) - ALL DATA ON THIS DRIVE WILL BE ERASED.
- The Oreon 10 ISO file (Download [here](https://oreonhq.com/en_us/oreon/download))
- A storage device (SSD or hard drive)
  - If installing Oreon alongside Windows: You will need at least 10 GiB of unallocated space, or an empty 9 GiB+ partition
  - If replacing Windows with Oreon: A drive with a total size of at least 10 GiB (recommended 30 GiB+ for installing apps and adding extra files)
- An internet connection (Optional for basic installation, but required for extra software)

## Make a bootable USB stick

You may choose one of the following 3 options depending on if you're using Windows, macOS, or Linux.

### Option A. Fedora Media Writer (Windows, macOS or Linux)

1. Install and open Fedora Media Writer (Download [here](https://fedoraproject.org/workstation/download/)).
2. Choose the "Select .iso file" option, then navigate to and choose the Oreon 10 ISO file.
3. Select your USB drive and press the "Write" button (WARNING: This will clear ALL data on the drive).

### Option B. Rufus (Windows)

1. Install and open Rufus (Download [here](https://rufus.ie/en/)) and select your USB stick under Device.
2. Press "SELECT" and choose the Oreon 10 ISO.
3. Press "START", and if asked to choose between DD and ISO mode, choose DD mode.

### Option C. dd (Linux)

1. Find the name (1st column) of your USB drive with `lsblk` (match it to the drive mount point on the right)
2. Double check that you have selected the right drive, and NOT your hard drive, as dd will erase all data on the selected drive.
3. Open a terminal in the folder you have the ISO in, and then run the command below, replacing the ISO's name with the one you downloaded `/dev/sdX` with `/dev/<your device's name>`:
```
sudo dd if=Oreon-10-BUILD-ARCH.iso of=/dev/sdX bs=4M status=progress oflag=sync
```

## Boot from the USB drive

### On Windows
1. Plug in your USB drive.
2. Open the Start menu and press the power icon, then hold down Shift and press "Restart". This will reboot you into the Advanced Startup Options.
3. Go to Troubleshoot > Advanced Options, and press a button that directs you to something along the lines of "Boot Menu" or "Boot from Another Device".
4. The previous step should reboot you into a new menu, where you can now select the USB device you put the ISO on previously and press Enter.
5. This will boot you into a live version of Oreon, where you can now open the installer from either the desktop or the app list and continue the guide.

### Universal Method
1. Plug in the USB drive and turn on or reboot your device.
2. As the device turns on, repeatedly press your device's boot menu key (see below) until the boot menu opens.
  - Common keys are F12 (Dell, Lenovo, many others), F11 (MSI, ASRock), F8 (ASUS), F9 (HP) and Esc. If none work, look up the key for your PC model.
4. Select the USB device you put the ISO on previously and press Enter.
5. This will boot you into a live version of Oreon, where you can now open the installer from either the desktop or the app list and continue the guide.

:::note
FOR ADVANCED USERS:
You do not need to turn off Secure Boot. Both the installer and the installed system are signed with Secure Boot keys. Additionally, dual booting requires UEFI, but BIOS systems are still supported on single boot systems.
:::

## Installation process

1. Pick your Installer Language if you wish to use the installer in another language.
2. Check the Installation Overview. It shows the Oreon version and an estimated time of 15 to 30 minutes.
3. Click Next to go to the Installation Summary, where you must complete all required options (everything except System Language).
   - Choose a keyboard layout.
   - Choose a system language from the locale list (optional).
   - Choose your timezone (leave NTP enabled if you don't know what it is)
   - Either choose a network in the bottom panel for installing additional software or choose to Continue Without a Network for a basic installation.
   - Choose your drive (the one you wish to install to, not the USB), and select one of the following:
     - Clean installation - Delete everything on the disk and replace the whole system with Oreon
     - Dual boot installation - Only available if you have unallocated space or a free partition on your hard drive, one of the two being at least 10 GiB in size. This option
       keeps Windows and installs Oreon alongside it. Choose the unallocated space or free partition to install to it.
   - Choose Desktop Installation for a normal PC (most users), or choose Server Installation if you are an experienced user who wishes to use the device as a server.
   - Choose the apps you want, do not disable Flatpak support unless you know what you're doing, and if you have an NVIDIA graphics card, enable the NVIDIA drivers under Advanced Options.
   - (ADVANCED USERS ONLY) A Server Accounts section will appear for server installations, allowing you to set up the user accounts at install time.
5. Once all settings are complete, you will be able to press "Begin Installation" to continue. Unless you are looking at the additional information, you may skip the next section.
6. After some time, the installation will finish and you will be shown an "Installation Complete" screen. From here, you may now reboot.
7. Remove the USB drive when the screen turns off so that, on clean installations, the system starts up from Oreon, or on dual boot installations, the system opens the boot menu where you can now choose Oreon.
8. On desktop installations, you may now set up your user account and begin using the system, or on server installations, you may now login using the account you made in the installer.

## Troubleshooting

### No suitable disks found

The installer did not find any disk it can use. Check that the drive is connected and shows up in your firmware settings, then click Refresh Disks. Read-only drives, CD drives and the USB stick you booted from are not listed.

### Please select a disk for installation.

No disk is selected. This happens when only removable drives are present. Pick a disk under Available Disks.

### Dual boot requires UEFI firmware.

Your PC started the installer in legacy BIOS mode. Restart, open the boot menu again and pick the USB entry that starts with UEFI.

### Select an existing EFI partition for dual boot.

Pick the Windows EFI partition under EFI Partition Selection before applying.

### Dual boot needs free space (at least 10 GiB) or an unused partition (at least 9 GiB).

There is not enough room. Shrink a Windows partition in Disk Management to leave more unallocated space, then click Refresh Disks. A block in the disk bar may also say it cannot be used, for example because it is too small, mounted, in use or already holds files.

### Additional software was selected, but no internet connection is available.

You picked extra software but the PC went offline. Connect to Wi-Fi or Ethernet and try again, or go back to Network Connectivity, choose Continue without network, and install the base system only.

### Installation failed

If the progress screen shows Installation failed followed by a message, note the message. Restart the live USB and try again. If it keeps failing, share the full message with the Oreon community when asking for help.

### The PC starts the USB again after install

The USB stick is still plugged in or is first in the boot order. Remove it and restart.
