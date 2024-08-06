---
tags:
  - windows
  - networking
  - port
---

# Show busy ports

```bat
netstat -abno
```

To filter only for port 8080:

```bat
netstat -abno | FINDSTR 8080
```

To kill a program by PID:

```bat
taskkill /PID %pid%
```

To find busy port via GUI: https://stackoverflow.com/a/23718720/7380828
