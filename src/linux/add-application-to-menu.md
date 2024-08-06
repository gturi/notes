---
tags:
  - linux
---

# Add application to menu entries

Create a new file named `${app-name}.desktop` inside `/$HOME/.local/share/applications`.

Modify the Desktop Entry according to your application and copy it inside `${app-name}.desktop`.

```
[Desktop Entry]
Version=version-number
Name=program-name
Comment=what the application is
Exec=/path/to/application-launcher
Path=/path/to/application-folder/
Icon=/path/to/application-folder/icon.xpm
Terminal=false
Type=Application
Categories=Utility;Application;Development;
```

Then add execution permissions to it with `chmod u+x ${app-name}.desktop`.
