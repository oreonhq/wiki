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

## Boot from the USB stick

1. Plug in the USB stick and turn on or restart the PC.
2. Right away, press the boot menu key over and over until a menu appears. Common keys are F12 (Dell, Lenovo, many others), F11 (MSI, ASRock), F8 (ASUS), F9 (HP) and Esc. If none work, look up the key for your PC model.
3. Pick the entry for your USB stick. If you see two entries for it, pick the one that starts with UEFI.
4. When the live desktop loads, open the installer from the desktop or app list.

You do not need to turn off Secure Boot. The installer copies the signed shim and GRUB boot loader from the live system, so Oreon 10 boots with Secure Boot turned on. Older PCs that only have legacy BIOS are also handled. Dual boot needs UEFI.

## How the installer is laid out

After the Welcome screen you land on the Installation Summary. It lists each setting as a row. Click a row to open its screen, choose your options, then press that screen's Apply button to come back to the summary. Rows marked Required must be done before you can start. Rows marked Configured are done.

At the bottom of the window are Back and Next buttons. On a settings screen the Back button says Cancel and takes you back to the summary without saving.

## Welcome

1. Pick your Installer Language if you want the installer in another language.
2. Check the Installation Overview. It shows the Oreon version and an estimated time of 15 to 30 minutes.
3. Click Next.

## Installation Summary

The summary has these rows.

1. Keyboard Layout (required)
2. System Language (optional)
3. Time & Date (required)
4. Network Connectivity (required)
5. Installation Destination (required)
6. Software Packages (required)

The Installation Status row at the bottom tells you how many required settings are left. When everything is done it says Ready for Installation and the Begin Installation button turns on.

## Keyboard Layout

1. Pick your layout from the Keyboard Layout list. You can type to search.
2. Try a few keys in the test box to make sure it is right.
3. Click Apply Keyboard Layout.

## System Language

1. Pick the language for your installed system from the Locale list.
2. Click Apply System Locale.

## Time & Date

1. Pick your Timezone from the list.
2. Leave Enable Network Time Protocol (NTP) turned on if you want the clock to set itself from the internet.
3. Click Apply Time & Date Settings.

## Network Connectivity

This screen shows whether you are online.

1. To use Wi-Fi, connect from the control center in the bottom panel below the installer, as the screen says.
2. If you are online and want extra software, click Apply next to Use network for additional software.
3. If you have no internet or only want the base system, click Continue without network.

## Installation Destination

This is where you choose the disk and how Oreon goes on it.

1. Click Scan for Disks if your disks are not listed yet. Refresh Disks scans again, for example after plugging in a drive.
2. Under Available Disks, pick the disk to install to. The installer picks your first internal drive for you. USB and other removable drives are marked Removable and are never picked for you. If you only have removable drives, nothing is picked and you need to choose one yourself. The USB stick you booted from is not listed.
3. Under Installation Mode, pick one of the two options described below.
4. Under Filesystem Configuration, leave Root Filesystem Type at ext4 (default) unless you have a reason to change it. You can also pick btrfs or xfs. With btrfs you can turn on BTRFS Snapshots (Rollback), which lets you roll the system back to an earlier state.
5. Click Apply Storage Plan.

### Clean Installation (erase the disk)

This is the recommended choice. It wipes the whole disk and installs Oreon on it. The installer creates a 512 MB EFI partition on UEFI PCs, a 2 GiB /boot partition, and uses the rest of the disk for the system. Custom Formatting Options lets advanced users change the EFI Partition Size.

### Dual Boot Installation (keep Windows)

This keeps Windows or another system and adds Oreon next to it. It needs a UEFI PC.

1. First make room. In Windows, open Disk Management, right-click your main partition and choose Shrink Volume. Leave at least 10 GiB unallocated, and more if you plan to keep files on Oreon. GParted works too.
2. Back in the installer, pick Dual Boot Installation.
3. In Install Location, click a free space block or an unused partition in the disk bar. Hover over a block to see why it can or cannot be used.
4. In EFI Partition Selection, pick the existing EFI partition that Windows uses.
5. Click Apply Storage Plan.

The installer puts a 2 GiB /boot partition and the system into the space you picked and leaves your other partitions alone. At startup you get a menu where you can choose Oreon or Windows.

## Software Selection

1. Under Installation Type, choose Desktop Installation for a normal PC with the GNOME desktop. Choose Server Installation for a computer you will run without a screen and manage over the network.
2. Under Additional Software, turn on any bundles you want, such as Multimedia Support, Development Tools, Productivity Suite, Gaming Support or Windows App Support. These need internet.
3. Under Application Store, leave Enable Flatpak Support on if you want to install apps from Flathub.
4. Under Web Browser, pick Firefox, Chrome, Brave, Edge or No web browser. You can always install one later.
5. If you have an NVIDIA graphics card, you can turn on NVIDIA Drivers under Advanced Options.
6. Click Apply Software Plan.

If you chose Continue without network earlier, the internet-only options are turned off and only the base system is installed.

## Server installs and accounts

A server install removes the GNOME desktop and starts the computer in text mode. There is no first-boot setup screen, so you create the login during install.

When you pick Server Installation, a Server Accounts section appears on the Installation Summary. Begin Installation stays off until it is filled in correctly.

1. Leave Create a user account on to make a normal user. Fill in Full name, Username, Password and Confirm password. Usernames must be lowercase, can use letters, numbers, dash and underscore, and must start with a letter or underscore. System names such as root are not allowed.
2. Leave Administrator ticked if this user should be able to run admin commands with sudo.
3. Turn on Enable the root account if you also want a root login, and type the root password twice.
4. You can set up a user, root, or both, but you need at least one.

If you set up only root, the installer allows root to log in over SSH with a password, so you can reach the server remotely. If you create a user, the normal SSH settings stay in place and you should log in as that user. If you do not enable root, the root account stays locked.

If something is wrong, a red message appears under the field, such as Passwords do not match.

## Begin the install

1. On the Installation Summary, check that every row says Configured.
2. Click Begin Installation.
3. Wait while the progress bar moves. The text under it shows what the installer is doing.

When it finishes you see Installation Complete.

## When it finishes

1. Click Reboot Now.
2. Remove the USB stick when the screen goes dark, so the PC starts from the disk.
3. Oreon starts up. On a dual boot PC, pick Oreon from the startup menu.

On a desktop install, GNOME setup opens on first boot. It walks you through creating your user account and a few basic settings. On a server install you get a text login, where you sign in with the account you made.

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
