SteamOS community tracker
=========================

This issue tracker is meant for community support and requests for improvements.

Hardware and Software Requirements
----------------------------------
SteamOS requires the following:

- Processor:  Intel or AMD 64-bit capable processor
- Memory:     4GB or more RAM
- Hard Drive: 500GB or larger disk
- Video Card: NVIDIA graphics card (AMD and Intel graphics support coming soon!)
- Additional: UEFI boot support, USB port for installation 

Install SteamOS
===============
There are two different installation methods for SteamOS. The recommended method is the Default Installation method, which is a pre-configured image-based install using CloneZilla. The other method uses Debian Installer, which allows for customization after an automated install step. Please choose one of those methods below.

Default Installation:
---------------------
You will need to create a SteamOS System Restore USB stick to perform this install. The image provided here requires at least a 1TB disk.

1. Download the default SteamOS beta installation
2. Format a 4GB or larger USB stick with the FAT32 filesystem. Use "SYSRESTORE" as the partition name.
3. Unzip the contents of SYSRESTORE.zip to this USB stick to create the System Restore USB stick.
4. Put the System Restore USB stick in your target machine. Boot your machine and tell the BIOS to boot off the stick. (usually something like F8, F11 or F12 will bring up the BIOS boot menu).
5. Make sure you select the UEFI entry, it may look something like "UEFI: Patriot Memory PMAP". If there is no UEFI entry, you may need to enable UEFI support in your BIOS setup.
6. Select "Restore Entire Disk" from the GRUB menu.
7. When it is complete it will shutdown. Power on the machine to boot into your freshly re-imaged SteamOS.


Custom Installation:
--------------------
The second method is based on the Debian Installer. It requires multiple configuration steps:

1. Download the custom SteamOS beta installation
2. Unzip the SteamOS.zip file to a blank, FAT32-formatted USB stick.
3. Put the USB stick in your target machine. Boot your machine and tell the BIOS to boot off the stick. (usually something like F8, F11, or F12 will bring up the BIOS boot menu).
4. Make sure you select the UEFI entry, it may look something like "UEFI: Patriot Memory PMAP". If there is no UEFI entry, you may need to enable UEFI support in your BIOS setup.
5. Selected "Automated install" from the menu.
6. The rest of the installation is unattended and will repartition the drive and install SteamOS.
7. After installation is complete, log onto the resulting system (using the Gnome session) with the predefined "steam" account. The password is "steam". Run steam, accept the EULA, and let it bootstrap. Logoff the steam account.
8. Log on with the "desktop" account. The password is "desktop".
9. From a terminal window, run ~/post_logon.sh. This will prompt for a password - enter "desktop". This script will perform the post-install customizations, delete itself, then reboot into the recovery partition capture utility.
10. Confirm "y" to continue and the recovery partition will be created. When it is finished, reboot into your freshly installed SteamOS.

Reporting Issues
----------------

If you encounter an issue while using Steam for Linux, first search the [issue list](https://github.com/ValveSoftware/SteamOS/issues) to see if it has already been reported. Include closed issues in your search.  Some issues may have already been reported to other trackers, so it may be worth checking the following:

- [Steam for Linux](https://github.com/ValveSoftware/steam-for-linux/issues) for Steam Client issues
- [Dota 2] (https://github.com/ValveSoftware/Dota-2/issues) for issues regarding Dota 2.
- [HalfLife] (https://github.com/ValveSoftware/halflife/issues) for issues regarding Half-Life 1 engine (GoldSrc) games.
- [Source-1 Games] (https://github.com/ValveSoftware/Source-1-Games/issues) for issues regarding Source games (HL2, TF2, CS:S, etc)

If it has not been reported, create a new issue with at least the following information:

- a short, descriptive title;
- a detailed description of the issue, including any output from the command line;
- steps for reproducing the issue; and
- your system information.\*

Please place logs either in a code block (press `M` in your browser for a GFM cheat sheet) or a [gist](https://gist.github.com).

\* The preferred and easiest way to get this information is from Steam's Hardware Information viewer in the standard Steam client from the menu (`Help -> System Information`). Once your information appears: right-click within the dialog, choose `Select All`, right-click again, and then choose `Copy`. Paste this information into your report, preferably in a code block.  If accessing from Big Picture Mode, go to `Settings -> System` to view the information. Unfortunately, that data must be copied by hand.

Conduct
-------


There are basic rules of conduct that should be followed at all times by everyone participating in the discussions.  While this is generally a relaxed environment, please remember the following:

- Do not insult, harass, or demean anyone.
- Do not intentionally multi-post an issue.
- Do not use ALL CAPS when creating an issue report.
- Do not repeatedly update an open issue remarking that the issue persists.

Remember: Just because the issue you reported was reported here does not mean that it is an issue with Steam.  As well, should your issue not be resolved immediately, it does not mean that a resolution is not being researched or tested.  Patience is always appreciated.
