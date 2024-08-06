---
tags:
  - udev
  - linux
  - usb
---

# Run script when device is plugged in


1. list all the plugged devices with `lsusb` command and identify the vendor id and product id (e.g. 046d:c339) of your device.
2. create a new udev rule inside `/etc/udev/rules.d` directory and name it `${rule-priority}-${rule-name}.rules`
3. write the following content inside the rule (replace XXXX and YYYY with the 4 digits respectively before and after the `:` of the previously identified device ID):
   ```bash
   SUBSYSTEMS=="usb", \
   ACTION=="add", \
   ATTRS{idVendor}=="XXXX", \
   ATTRS{idProduct}=="YYYY", \
   RUN+="/path/to/script.sh"
   ```
4. reload udev rules running:
   ```bash
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

## Example with the previous values:

```bash
SUBSYSTEMS=="usb", \
ACTION=="add", \
ATTRS{idVendor}=="046d", \
ATTRS{idProduct}=="c339", \
RUN+="/path/to/script.sh"
```

## Notes

By default, udev runs everything as root. Thus, the script execution may fail when it invokes programs which store their configurations into the current user home directory (i.e. OpenRGB).

To solve this issue, you can change run param as follows:
```bash
# replace $myUser with the username you need
RUN+="/bin/su $myUser -c '/path/to/script.sh'"
```

## Useful resources

- [http://reactivated.net/writing_udev_rules.html#sysfstree](http://reactivated.net/writing_udev_rules.html#sysfstree)
