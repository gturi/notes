---
tags:
  - linux
  - kde
  - plasma
---

# Restart KDE plasma

Put the following function into `.bashrc` to restart KDE plasma when glitches occur. 

```bash
function restartPlasma {  
    kquitapp5 plasmashell  
    kstart5 plasmashell  
}  
  
alias reloadPlasma=restartPlasma
```
