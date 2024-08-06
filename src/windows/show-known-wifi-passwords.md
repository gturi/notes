---
tags:
  - windows
  - password
---
# Show known Wi-Fi passwords

```bat
:: display all known wi-fi profiles (which are the networks you connected to)
netsh wlan show profiles
```

```bat
:: display password of a profile
netsh wlan show profile name=%profilename% key=clear
```
