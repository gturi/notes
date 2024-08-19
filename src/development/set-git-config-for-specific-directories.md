---
tags:
  - development
  - git
  - config
---

# Override global git config for specific directories

```.gitconfig
# .gitconfig
[user]
  name = username
  email = name.surname@example.com

[includeIf "gitdir:*/private-projects/*"]
  path = ~/.gitconfig-private
[includeIf "gitdir:*/secret-projects/*"]
  path = ~/.gitconfig-secret
# on windows git bash it works by specifying the absolute path
# of the directory and using forward slashes as path separators
[includeIf "gitdir:C:/path/to/windows-projects/"]
  path = .gitconfig-windows
```

```.gitconfig
# .gitconfig-private
[user]
  name = private-username
  email = private.name.surname@example.com
```

```.gitconfig
# .gitconfig-secret
[user]
  name = secret-username
  email = secret.name.surname@example.com
```
