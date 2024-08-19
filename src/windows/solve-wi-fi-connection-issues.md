---
tags:
  - windows
  - networking
  - wi-fi
---

# Solve wi-fi connection issues

When connecting to wi-fi, sometimes Windows shows the following error message:

```
No Internet, Secured
```

Open cmd as administrator and run:
```batch
netsh int ip reset
netsh winsock reset
ipconfig /release
ipconfig /renew
ipconfig /flushdns
```
