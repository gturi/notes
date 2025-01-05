---
tags:
  - windows
  - powershell
  - case-sensitive
---

# Make directory case sensitive

```powershell
$path = "C:\your\path"

# this will make a directory case sensitive and new directories will inherit this property
fsutil.exe file SetCaseSensitiveInfo "$path" enable

cd "$path"

# this will make existing sub directories case sensitive
(Get-ChildItem -Recurse -Directory).FullName | ForEach-Object {fsutil.exe file setCaseSensitiveInfo $_ enable}
```
