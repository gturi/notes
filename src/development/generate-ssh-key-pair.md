---
tags:
  - development
  - ssh
  - ssh-keygen
  - git
---

# Generate ssh key pair

Template for generating ssh keys:

generate-ssh-key.sh
```bash
#!/bin/bash

echo "Guided procedure to generate a new ssh key pair"

read -r -p "Enter your email: " EMAIL

SSH_FILE_BASENAME="$HOME/.ssh/id_"

echo "The ssh keys file basename will be $SSH_FILE_BASENAME"

read -r -p "Enter ssh file identifier (i.e. github_yourUserId): " SSH_FILE_ID

ssh-keygen -t ed25519 -C "$EMAIL" -f "${SSH_FILE_BASENAME}${SSH_FILE_ID}"

echo "Key pair generated successfully, add ${SSH_FILE_BASENAME}${SSH_FILE_ID}.pub to your account"

ls -la "$HOME/.ssh"
```


## Permissions

| file               | permission   |       |
| ------------------ | ------------ | ----- |
| `.ssh/config`      | `-rw-r--r--` | `644` |
| `.ssh/id_xxx`      | `-rw-------` | `600` |
| `.ssh/id_xxx.pub`  | `-rw-r--r--` | `644` |
| `.ssh/known_hosts` | `-rw-------` | `600` |


## References

- https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
