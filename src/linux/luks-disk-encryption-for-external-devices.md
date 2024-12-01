---
tags:
  - LUKS
  - btrfs
  - backup
  - mkfs
  - cryptsetup
---

# LUKS disk encryption for external devices


## Find your device

```bash
lsblk
```

Output:

```
sda               8:0    0   1.8T  0 disk
└─sda1                               part /myDevice1
sdb               9:0    0   950G  0 disk
└─sdb1                               part /myDevice2
nvme0n1         259:0    0 953.9G  0 disk
├─nvme0n1p1     259:1    0   300M  0 part
├─nvme0n1p2     259:2    0   128M  0 part
├─nvme0n1p3     259:3    0 930.2G  0 part
├─nvme0n1p4     259:4    0     1G  0 part
└─nvme0n1p5     259:5    0  22.3G  0 part
```

From here on `X` will indicate the device to format.

For example if the device is mounted on `sda`, `X` will need to be replaced with that. Thus, the target of the commands will be `/dev/sda`.


## Format the device (optional)

```bash
sudo dd if=/dev/zero of=/dev/X count=4096
```


## Format the device for LUKS

```bash
sudo cryptsetup luksFormat /dev/X
sudo cryptsetup isLuks /dev/X && echo Success
```

Output:

```
WARNING!
========
This will overwrite data on /dev/X irrevocably.

Are you sure? (Type uppercase yes): YES
Enter passphrase: 
Verify passphrase:
```


## Open the LUKS volume

Replace `$myDevice` with a user friendly name, since it will be used to mount the unencrypted device under `/dev/mapper/$myDevice`:

```bash
sudo cryptsetup open /dev/sdX $myDevice
```


## Create the filesystem

```bash
sudo mkfs.btrfs -f -L "$deviceLabel" /dev/mapper/$myDevice
```

The `$deviceLabel` is user friendly name to assign to the device, which will be used by your desktop environment when mounting the device.

## Backup LUKS headers

If the sectors containing the LUKS headers are damaged - by user error or HW failure - all data in the encrypted block device is lost. Backing up the headers can help recovering data in such cases.

To backup the LUKS headers, use the following command:

```bash
sudo cryptsetup luksHeaderBackup --header-backup-file "$HOME/Desktop/$deviceLabel.luksHeaderBackup" /dev/X
```

To restore the LUKS headers, use the following command:

```bash
cryptsetup luksHeaderRestore --header-backup-file "$HOME/Desktop/$deviceLabel.luksHeaderBackup" /dev/X
```


## Give your current user read and write access to the device

For removable devices, it is normal that a file system that supports Linux permissions is mounted for `root`. Only file systems not supporting Linux permissions will be mounted for the user connecting the device.

To change ownership of the entire device by changing the ownership of the mount point, execute:

```bash
sudo chown $USER:$USER "/run/media/$USER/$deviceLabel"
```

Mount points of removable devices are automatically created and removed when the device is plugged in/out.
The ownership and the permissions, however, are remembered.


## Maintenance

### btrfs

In a btrfs raid setup it is necessary to frequently (i.e. monthly) run a `btrfs scrub` to check for corrupted blocks/flipped bits and repair them using a healthy copy from one of the mirror disks. 

```bash
btrfs scrub start -Bd "/run/media/$USER/$deviceLabel"
```


## References

- https://fedoraproject.org/wiki/Disk_Encryption_User_Guide#Creating_Encrypted_Block_Devices_on_the_Installed_System_After_Installation
- https://opensource.com/article/21/3/encryption-luks
- https://gist.github.com/MaxXor/ba1665f47d56c24018a943bb114640d7
