---
tags:
  - linux
  - guide
  - LUKS
  - disk-encryption
  - intel-rapid-storage-technology
  - ahci
---

# Dual boot Linux with LUKS full disk encryption

Hi I would like to share with you some useful reference if you want to work with a Linux based OS on your computer, since I had some hassles while doing it (especially by switching from RAID/IDE to AHCI). As the title says we will use full disk encryption: I know it is a bit more bothersome, since you will need two passwords to login (one for unencrypting your disk, the other one to actually login in your user profile), but I think it is a better practice to protect potentially confidential information. As a matter of fact, just using home encryption does not cover your swap partition or folders like `/tmp` or `/var/log` to be read by externally booting your disk.

**DISCLAIMER**: what I want to share is not a full in depth guide (for that I will link some useful ones), I assume you are a bit familiar with dual booting. For any imprecision feel free to point it out, I am always willingful to learn something new :)

My reference Distro will be Kubuntu, but this guide should work for every other stable distro. So let's get started!


The standard steps to make a dual install are:

- shrink your C: drive from Windows Disk Management to create free space for Linux OS
- even though it might not be necessary, I temporary disabled BitLocker encryption (after successfully dual booting I re-enabled it).
To do so you need to retrieve your BitLocker password: `manage-bde -protectors C: get`
- disable secure boot (go to your BIOS settings)
- start the live iso of your distro and proceed with the guide installation

Well here started my problems:
- the live installer could not recognize the configuration of my drive, since it was using Intel Rapid Storage Technology. Long story short Intel Rapid Storage Technology allows the system to manage different partitions on the same drive as if they were a single one. This setup is often not recognized by live installers and it needs to be changed to AHCI mode.
- Kubuntu installer allows full disk encryption only by completely formatting the entire drive, to make a dual boot setup this must be done manually.


## Switching from Intel Rapid Storage Technology to AHCI mode

This operation must be done from administrator command prompt and changing the BIOS Operation mode entry from Intel Rapid Storage Technology to AHCI.

1. run `bcdedit /set {current} safeboot minimal`
    -  if it fails run `bcdedit /set safeboot minimal`
2. restart and enter in the BIOS and change the BIOS Operation mode entry from Intel Rapid Storage Technology to AHCI.
3. `bcdedit /deletevalue {current} safeboot`
    -  if it fails run `bcdedit /deletevalue safeboot`

More in depths details of these steps can be found at http://triplescomputers.com/blog/uncategorized/solution-switch-windows-10-from-raidide-to-ahci-operation/


If something goes wrong, I.E Windows boot looping, changing back the BIOS Operation mode entry should fix it (obviously you will need to start over the procedure trying something else).

Actually there is another guide to achieve the operation mode switch, but it did not work for me (maybe you can be luckier) https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-with-intel-r-rst-enabled/15347


## Setup dual boot with full disk encryption

For this I assume you have already shrinked your disk to have space for the distro of your choice.

For this step I highly recommend to follow the instruction of this guide: https://www.mikekasberg.com/blog/2020/04/08/dual-boot-ubuntu-and-windows-with-encryption.html

I will give some tips that helped me during this phase:
- be careful to just copy/paste the commands the guide gives: as it is said in the guide, the author always refer to `/dev/sda`, but in my case the partition was mounted on `/dev/nvme0n1`. Probably creating an alias could help avoiding some mistakes
- at first you will need to create `/boot` and `rootfs` partitions, for this step I preferred using GParted so it was easier to figure out what I was doing instead of squashing around some commands
- do not save disk space by making `/boot` partition as small as possible: I suggest to make it at least `1000 MB` or even `1250 MB`. "Wasting" a few hundred megabytes usually does not hurt as bad as a `/boot` filesystem that turns out to be too small in maybe 5 or 10 years.
