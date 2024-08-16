---
tags:
  - linux
  - terminal
  - konsole
---

# Open konsole and run script if it was not started from konsole session

This does not alter the script behaviour when running it directly inside a konsole session with `/path/to/script.sh`

```bash
#!/bin/bash

tty -s || exec konsole -e "$0" "$@"

# script logic
```
