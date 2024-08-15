---
tags:
  - linux
  - grub2
---

# Display grub menu at startup

Open `/etc/default/grub` as root with your favourite text editor.

Replace `GRUB_TIMEOUT_STYLE=hidden` with `GRUB_TIMEOUT_STYLE=menu`.


## Method 1: update-grub

Apply the changes by running: 

```bash
update-grub
```


## Method 2: grub-mkconfig

**WARNING**: The suggested way to update grub is via `update-grub` since your distribution could have customized `update-grub` script to run additional configuration commands/scripts.

If your distribution does not have `update-grub` in its package manager repository, you can also use: 

```bash
exec grub-mkconfig -o /boot/grub/grub.cfg "$@"
``` 
