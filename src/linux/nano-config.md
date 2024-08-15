---
tags:
  - linux
  - nano
  - nanorc
  - config
---

# nano config

Nano configs are stored into `$HOME/.nanorc` file.

```nanorc
# Show line numbers
set linenumbers
  
# Show trailing whitespaces
syntax "default"
color green "[[:space:]]+$"
```

External configs can be included using `include` followed by the absolute path of the config to load.

```nanorc
include /path/to/external/config
```

Builtin configs are stored into `/usr/share/nano` directory.


## Git commit syntax

Nice syntax for git commit, for further improvements this repo can be used as a reference https://github.com/scopatz/nanorc/blob/master/git.nanorc

git-commit.nanorc

```nanorc
syntax "gitcommit" "COMMIT_EDITMSG$"
  
color cyan "^\$.*"
color blue "\$.new file.*"
color green "\$.modified.*"
color red "\$.deleted.*"
color yellow start="\$ Changes.*" end="\$ Changed.*"
color cyan start="\$ Untracked.*" end="diff"
color cyan start="\$ Untracked.*" end="$$"
color brightred "^deleted file mode .*"
color brightgreen "^\+.*"
color brightred "^-.*"
color brightyellow "^(diff|index|---|\+\+\+).*"
color brightmagenta "@@.*@@"

color brightblue "^\$ On branch .+$"
color brightblue "^\$ Your branch is ahead of .+$"

color white "\$ Change[ds] .*"
color white "^\$ Untracked files:"
color white "\$.*\(use .*"
color cyan "^\$"
```
