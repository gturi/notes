---
tags:
  - linux
  - systemd
  - systemctl
  - out-of-memory
---

# Disable systemd-oomd (out-of-memory daemon)

It happens that `systemd-oomd` may kill application when the system runs low on resources. For example when IDEs and terminals use a lot of threads or memory. If that happens it can be disabled with:

```bash
systemctl disable --now systemd-oomd

systemctl is-enabled systemd-oomd
```

To avoid other services restarting `systemd-oomd`, it can be permanently disabled with:

```bash
systemctl mask systemd-oomd

systemctl is-enabled systemd-oomd
```

To undo the operations:

```bash
systemctl enable systemd-oomd

systemctl unmask systemd-oomd
```

## Reference

- https://askubuntu.com/a/1405588/1198282