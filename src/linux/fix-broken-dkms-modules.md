---
tags:
  - linux
  - dkms
  - fix
  - openrazer
---

# Fix broken dkms modules

DKMS: **Dynamic Kernel Module Support** is a program/framework that enables generating Linux kernel modules whose sources generally reside outside the kernel source tree. DKMS modules should be automatically rebuilt when a new kernel is installed.

During a kernel upgrade the package manager may complains about broken dkms modules like this:

```bash
openrazer-driver/3.4.0: broken
Error! openrazer-driver/3.4.0: Missing the module source directory or the symbolic link pointing to it.
Manual intervention is required!
openrazer-driver/3.5.1: broken
Error! openrazer-driver/3.5.1: Missing the module source directory or the symbolic link pointing to it.
Manual intervention is required!
openrazer-driver/3.6.1: broken
Error! openrazer-driver/3.6.1: Missing the module source directory or the symbolic link pointing to it.
Manual intervention is required!
openrazer-driver/3.7.0, 6.7.7-1-default, x86_64: installed
```

To see the status of broken dkms modules:

```bash
sudo dkms status
```

To remove the broken module you can try:

```bash
sudo dkms remove openrazer-driver/3.4.0 --all
```

If `Manual intervention is required!` error pops out, you need to manually remove the module directory located at `/var/lib/dkms/${module-name}`.

I.E.

```bash
sudo rm -rf /var/lib/dkms/openrazer-driver/3.4.0/
sudo rm -rf /var/lib/dkms/openrazer-driver/3.5.1/
sudo rm -rf /var/lib/dkms/openrazer-driver/3.6.1/
sudo rm -rf /var/lib/dkms/openrazer-driver/3.7.0/
sudo rm -rf /var/lib/dkms/openrazer-driver/
```
