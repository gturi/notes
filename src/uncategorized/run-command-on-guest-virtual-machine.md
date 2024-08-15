---
tags:
  - virtualbox
  - headless
  - virtual-machine
---

# Run command on guest VirtualBox virtual machine

**Guest** = virtualized OS

**WARNING**: even if you use a pin to log in, you need to provide your actual Windows password.

```bash
VM="windows64" # name of the virtual machine
USER="user" # username used to login in the virtual machine
PSW="password" # password used to login in the virtual machine 
CMD="C:\\absolute\\path\\to\\command"

VBoxManage guestcontrol "$VM" --username "$USER" --password "$PSW" run --exe "$CMD"
```


On Windows  `$USER` value can be retrieved by logging into the guest virtual machine and running:

```batch
echo %username%
```
