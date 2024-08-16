---
tags:
  - linux
  - chroot
  - vpn
  - networking
  - fedora
---

# Setup chroot environment on Fedora

Chroot is a tool that can be used to run software in a sandboxed environment but also to setup an environment to run outdated software.
The script will target Fedora 28, since at the time I had to install an outdated VPN client that was supported up until that version.

References used:
- https://wiki.archlinux.org/title/chroot
- https://unix.stackexchange.com/questions/198590/what-is-a-bind-mount


## Setup chroot environment

```bash
#!/bin/bash

tty -s || exec konsole -e "$0" "$@"

RELEASE_SERVER=28
chr="$HOME/chroot-test"

mkdir -p "$chr"

function install_in_chroot {
    sudo dnf install -y --installroot="$chr" --releasever=$RELEASE_SERVER "$1"
}

install_in_chroot sudo
install_in_chroot libgcc
install_in_chroot which
install_in_chroot systemd
install_in_chroot iputils
install_in_chroot nano
install_in_chroot top
install_in_chroot psmisc # killall command
# install any other application you need in the chroot environment

# used later on to setup chroot networking
sudo touch "$chr/etc/resolv.conf"
sudo touch "$chr/usr/bin/resolvectl"
```


## Binding main filesystem files and run command in chroot

Summary of what `bind` operation does: "The directories and files in the bind mount are the same as the original.  Any modification on one side is immediately reflected on the other side, since the two views show the same data."

```bash
#!/bin/bash

tty -s || exec konsole -e "$0" "$@"

chr="$HOME/chroot-test"

function mount_in_chroot() {
    local mainSystemFile="$1"
	local chrootTargetFile="${chr}${mainSystemFile}"
    mountpoint "$chrootTargetFile"
    mountpoint -q "$chrootTargetFile" || sudo mount --bind "$mainSystemFile" "$chrootTargetFile"
    mountpoint "$chrootTargetFile"
}

mount_in_chroot /dev
mount_in_chroot /proc
mount_in_chroot /sys
mount_in_chroot /etc/resolv.conf
mount_in_chroot /usr/bin/resolvectl

sudo chroot "$chr" ./command-to-run
```
