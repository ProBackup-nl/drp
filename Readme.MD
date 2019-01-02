Bare metal disaster backup and recovery plan for Arch Linux. 
===

USAGE: drp [--prepare /dev/<UNMOUNTED_DESTINATION_DEVICE>]


DRP keeps: 
* hourly backups for the past 24 hours,
* daily backups for the past month,
* weekly backups for all previous months. 
The oldest backups are deleted when your disk becomes full.

DRP:
- creates a (UEFI) bootable recovery on the block device
- updates the bootable kernel to keep up with your Linux kernel version
- saves target disk space by using BTRFS snapshots
- uses a systemd service and timer 

REQUIREMENTS: 2nd block device (f.e. USB drive), UEFI, Arch Linux, systemd-boot, btrfs, dhcpcd, mkinitcpio-nfs-utils, <...more see source code...>

---
# Motivation
This project started for several reasons: 
- ReaR is too immature for Arch Linux+UEFI, 
- I couldn't find another disaster recovery solution for Linux that is low on dependencies, 
- to mimic Time Machine, 
- to optimise for SSD/nvme/flash storage, 
- to be low on (destination) disk space (hope that a btrfs snapshot without changes consumes less disk space then
re-creating all directories and hard-links for files.

# ToDo
- thin weekly snapshots when low on disk space
- verify rescue medium configs file integrity before usage (as well as the MD5sum file itself)
- store textual configs multiple times in different compressed formats to prevent smart controllers from de-duplicating the bit-stream (and have backup copies in place)
- at config file or integrity errors auto fall back to alternative (compressed) copies
- cleanup logging / use journald logging
- backup:snapshot source
- backup:search subvolumes on source and snapshot these too
- recover:ssd detected, ask for secure erase?
- recover:no erase && partitions available -> rsync restore instead of copy
