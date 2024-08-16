---
tags:
  - linux
  - android
  - ssh
  - openssh
  - rclone
  - fuse
---

# Mount an Android device on your pc filesystem over ssh 

## Install scrcpy

Scrcpy allows to send keyboard input to the device, its installation is recommended since typing commands in the terminal from Android's keyboard it is quite uncomfortable:

```bash
scrcpy -K
```

## Install Termux and setup openssh server

- Install Termux from Github (or F-Droid) on the phone you want to SSH into. Don't use the Play Store version, it doesn't work
- run `pkg update` and `pkg upgrade`
- run `passwd` and set up a password
- run `pkg install openssh`
- run `ssh-keygen -A`
- run `termux-setup-storage` - this will make the app ask for storage permission
- run `sshd`
- run `ifconfig` to know you IP address - look at `inet` field that looks like 192.168.X.Y


## Test ssh connection

```bash
port=8022
android_ip=192.168.X.Y
ssh $android_ip -p $port
```

The last command will prompt to digit the password you have previously entered.
Usually ssh specifies a username (username@$server_ip), if your ssh client doesn't allow you to not enter a username, just use a blank space or asterisk.

Once connected, you can display your device internal directory tree as follows:
```bash
cd /storage/emulated/0
ls
```

## Mount android 

### With rclone

Run `rclone config` and select `sftp` option and follow the guided setup.

```bash
rclone config
```


Performing a minimal setup will produce something like this at `$HOME/.config/rclone`:

```
[ssh-connection-name]
type = sftp
host = $android_ip
port = $port
```

By default rclone will attempt to connect using `ssh-agent`. To connect using a password use `--sftp-ask-password` flag:

```bash
android_dir=/storage/emulated/0
mount_point=/mnt/android-device

curr_user="$(whoami)"

id "$curr_user"

sudo mkdir "$mount_point"
sudo chown "$user:$user" "$mount_point"

rclone mount android-ssh:$android_dir "$mount_point" --sftp-ask-password
```

### With sshfs (deprecated)

Unfortunately sshfs is no longer maintained, but the setup script is still left for reference.

```bash
android_dir=/storage/emulated/0
mount_point=/mnt/android-device

user="$(whoami)"

id "$user"

sudo mkdir "$mount_point"
sudo chown "$user:$user" "$mount_point"

sshfs \
  -o idmap="$user" \
  -p $port \
  "$android_ip:$android_dir" \
  "$mount_point"
```

If you need to troubleshoot `sshfs` connection you can add the following options:

```bash
-o debug,sshfs_debug,loglevel=debug
```

If you want to specify a custom ssh config:

```bash
-F /path/to/.ssh/config
```

## Stop Android ssh server

```bash
pkill sshd
```

## Unmount directory

```bash
fusermount -u "$mount_point"
sudo umount "$mount_point"
sudo rmdir "$mount_point"
```
