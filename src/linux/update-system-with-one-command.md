---
tags:
  - linux
  - update
  - dnf
  - apt
  - snap
  - flatpak
---
# Update system with one command

## dnf based

```bash
function syu {
   sudo -- bash -c 'dnf upgrade --refresh; dnf autoremove; snap refresh; rclone selfupdate'
   flatpak update; flatpak uninstall --unused;
}
```

## apt based

```bash
function syu {
    sudo -- bash -c 'apt update; apt upgrade; apt autoremove; snap refresh; rclone selfupdate';
    flatpak update; flatpak uninstall --unused;
}
```
