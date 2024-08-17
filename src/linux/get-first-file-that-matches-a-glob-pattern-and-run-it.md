---
tags:
  - linux
  - bash
  - glob-pattern
---

# Get first file that matches a glob pattern and run it

```bash
#!/bin/bash

cd "$(dirname "$0")" || exit

function run() {
  local pattern=$1
  local files=( $pattern )
  local arguments="${@:2}" # ignores the first argument (which is the pattern)
  ./"${files[0]}" $arguments
}

run "OpenRGB_*.appimage" $@
```
