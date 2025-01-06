---
tags:
  - linux
  - shell
  - bash
  - zsh
  - tab-completion
---

# Command alias tab completion

## Bash

The easiest way to have tab completion for command aliases in bash is to download `complete_alias` utility and source it inside your `~/.bashrc`.

```bash
mkdir "$HOME/.bash_completion.d"
curl https://raw.githubusercontent.com/cykerway/complete-alias/master/complete_alias \
     > "$HOME/.bash_completion.d/complete_alias"
```

Usage example:

```bash
# ~/.bashrc
source "$HOME/.bash_completion.d/complete_alias"

alias g=git
complete -F _complete_alias g

alias ga="git add"
complete -F _complete_alias ga

alias gc="git commit"
complete -F _complete_alias gc

# to automatically complete every alias put this line after all the aliases have been declared (at the end of .bashrc)
complete -F _complete_alias "${!BASH_ALIASES[@]}"
```

## Zsh

If configured correctly, zsh automatically supports tab completion for declared aliases.
