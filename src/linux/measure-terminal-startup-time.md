---
tags:
  - linux
  - bash
  - bashrc
---

# Measure terminal startup time

Add the following code to `.bashrc` to measure terminal startup time.

```bash
startTime=$(($(date +%s%N)/1000000))

# code loaded at startup

endTime=$(($(date +%s%N)/1000000))  
     
echo "Terminal prompt ready in: $(( endTime - startTime )) ms (Start: ${startTime}, End: ${endTime})"
```
