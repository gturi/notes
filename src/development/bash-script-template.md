---
tags:
  - linux
  - bash
  - template
---

# Bash script template

```bash
#!/bin/bash

set -o errexit
set -o nounset
set -o pipefail
if [[ "${TRACE-0}" == "1" ]]; then set -o xtrace; fi
export SHELLOPTS

cd "$(dirname "$0")" # cd into current script directory

SCRIPT_DIR="$(pwd)"
```


## Notes

- `set -o errexit`: 
  - when a command fails, bash exits instead of continuing with the rest of the script
- `set -o nounset`:
  - the script will fail when accessing an unset variable
  - to access a variable that may or may not have been set, use "${VARNAME-}" instead of "$VARNAME"
- `set -o pipefail`:
  - a pipeline command is treated as failed, when one of the commands in the pipeline fail
- `cd "$(dirname "$0")"`:
  - change to the script's directory
- `if [[ "${TRACE-0}" == "1" ]]; then set -o xtrace; fi`:
  - enables debug mode by running `TRACE=1 ./script.sh`
- remember to redirect error logs to stderr
  - i.e. `echo 'Something unexpected happened' >&2`
- `export SHELLOPTS`:
  - makes sub scripts inherit `set` options
