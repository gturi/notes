---
tags:
  - linux
  - grub2
  - windows-to-go
---

# Add Windows To Go entry to grub 2

Mount the external drive and take note of its `hints_string` and `uuid` (change `MEDIA_LABEL` value with the name of the external drive):

```bash
MEDIA_MOUNTPOINT="/media/$USER/"
MEDIA_LABEL=ESD-ISO
WINDOWS_BOOT=/EFI/Microsoft/Boot/bootmgfw.efi

sudo grub-probe --target=hints_string ${MEDIA_MOUNTPOINT}${MEDIA_LABEL}${WINDOWS_BOOT}
# hints_string

sudo grub-probe --target=fs_uuid ${MEDIA_MOUNTPOINT}${MEDIA_LABEL}${WINDOWS_BOOT}
# fs_uuid
```

Update the content of `/etc/grub.d/40_custom` with:

```bash
# notice that hints_string values are quoted
hints_string="hist-string-values"
fs_uuid=uuid-value

menuentry "Windows To Go" {
    insmod part_gpt
    insmod ntfs
    insmod fat
    insmod search_fs_uuid
    insmod chain
    search --fs-uuid --set=root $hints_string $fs_uuid
    chainloader /EFI/Microsoft/Boot/bootmgfw.efi
    configfile /boot/grub/grub.cfg
}
```

Finally update grub with the new entry:

```bash
sudo update-grub
```

## References

https://askubuntu.com/a/805090/1198282
