---
tags:
  - linux
  - bash
  - glob-pattern
  - appimage
---

# Get first file that matches a glob pattern and run it

Useful for dealing with Appimage packaged programs which add their version to the file name. Using the script in your `.application` file will avoid having to change the version number each time the Appimage is updated.

```bash
#!/bin/bash

cd "$(dirname "$0")" || exit

function run() {
  local pattern=$1
  # list files matching the pattern in reverse order, giving priority to the latest version
  local files=( $(ls -r $pattern) )
  # ignores the first argument (which is the pattern)
  local arguments="${@:2}"

  if [ ${#files[@]} -eq 0 ]; then
    echo "No files found matching pattern: $pattern"
    exit 1
  fi

  chmod u+x ./"${files[0]}"

  ./"${files[0]}" $arguments
}

run "OpenRGB_*.appimage" $@
```
