---
tags:
  - linux
  - bash
  - bashrc
  - config
---

# Lazy load .bashrc scripts

## nvm example

```bash
export NVM_DIR="$HOME/.nvm"

function loadNvm() {
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
}

# careful: bash completion can't be lazily loaded or otherwise it will not work!
# (maybe there is a trick to do that but it needs further investigations...)
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion

loadNvm &
```
