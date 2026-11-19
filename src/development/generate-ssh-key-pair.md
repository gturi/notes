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
set -euo pipefail

SSH_DIR="$HOME/.ssh"
SSH_CONFIG="$SSH_DIR/config"

echo "Guided procedure to generate a new ssh key pair"

read -r -p "Enter your email: " EMAIL
read -r -p "Enter ssh file identifier (i.e. github_yourUserId): " SSH_FILE_ID
[[ -z "$SSH_FILE_ID" ]] && { echo "Identifier cannot be empty" >&2; exit 1; }

KEY_FILE="$SSH_DIR/id_${SSH_FILE_ID}"
[[ -e "$KEY_FILE" ]] && { echo "$KEY_FILE already exists, aborting" >&2; exit 1; }

read -r -p "Host alias(es) this key is for, space separated (i.e. github.com): " HOSTS
[[ -z "$HOSTS" ]] && { echo "At least one host is required" >&2; exit 1; }
read -r -p "HostName (real hostname, leave empty if same as alias): " HOSTNAME_REAL
read -r -p "Remote user (leave empty to use default): " REMOTE_USER

for h in $HOSTS; do
  if grep -qE "^Host(\s.*)?\s${h//./\\.}(\s|$)" "$SSH_CONFIG" 2>/dev/null; then
    echo "Host '$h' already has a block in $SSH_CONFIG, aborting" >&2
    exit 1
  fi
done

mkdir -p "$SSH_DIR"
chmod 700 "$SSH_DIR"

ssh-keygen -t ed25519 -C "$EMAIL" -f "$KEY_FILE"

BLOCK="Host $HOSTS"$'\n'
[[ -n "$HOSTNAME_REAL" ]] && BLOCK+="    HostName $HOSTNAME_REAL"$'\n'
[[ -n "$REMOTE_USER" ]] && BLOCK+="    User $REMOTE_USER"$'\n'
BLOCK+="    IdentityFile ${KEY_FILE/#$HOME/~}"$'\n'
BLOCK+="    IdentitiesOnly yes"$'\n'
BLOCK+="    AddKeysToAgent yes"$'\n'

touch "$SSH_CONFIG"
cp "$SSH_CONFIG" "$SSH_CONFIG.bak"
TMP="$(mktemp "$SSH_DIR/config.XXXXXX")"
{ printf '%s\n' "$BLOCK"; cat "$SSH_CONFIG"; } > "$TMP"
chmod 600 "$TMP"
mv "$TMP" "$SSH_CONFIG"

echo "Added to $SSH_CONFIG (backup at $SSH_CONFIG.bak):"
echo "$BLOCK"

FIRST_HOST="${HOSTS%% *}"
ssh -G "$FIRST_HOST" | grep -E '^(hostname|user|identityfile|identitiesonly) '

read -r -p "Copy the public key to $FIRST_HOST with ssh-copy-id? [y/N] " COPY_ANSWER
if [[ "$COPY_ANSWER" =~ ^[Yy]$ ]]; then
  ssh-copy-id -i "$KEY_FILE.pub" "$FIRST_HOST"
  ssh -o BatchMode=yes "$FIRST_HOST" true && echo "Key login to $FIRST_HOST works"
else
  echo "Add this public key to your account:"
  cat "$KEY_FILE.pub"
fi
```


## Permissions

| file               | permission   |       |
| ------------------ | ------------ | ----- |
| `.ssh/config`      | `-rw-r--r--` | `644` |
| `.ssh/id_xxx`      | `-rw-------` | `600` |
| `.ssh/id_xxx.pub`  | `-rw-r--r--` | `644` |
| `.ssh/known_hosts` | `-rw-------` | `600` |

## Copying ssh public key to a server

```bash
ssh-copy-id -i ~/.ssh/${my_custom_name}.pub ${user}@${server_ip}
```


## References

- https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
