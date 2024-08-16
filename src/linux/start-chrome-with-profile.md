---
tags:
  - linux
  - chrome
---

# Start chrome with profile

chrome-profile.desktop

```bash
[Desktop Entry]
Version=1.0
Terminal=false
Type=Application
Name=Google Chrome $ID
Exec=/usr/bin/google-chrome-stable --ozone-platform=wayland --profile-directory=Default --app-id=$ID %U
#Exec=/usr/bin/google-chrome-stable --ozone-platform=wayland --profile-directory="Profile 1" --app-id=$ID %U
Icon=$HOME/.config/google-chrome/Default/Google Profile Picture.png
StartupWMClass=crx_$ID
```

Replace `$HOME` with your home directory, `$ID` with with a custom profile id.

Save the file into `$HOME/.local/share/applications` and give it execution permissions.

Unfortunately `--profile-directory` flag does not seem to work with recent chrome versions...
