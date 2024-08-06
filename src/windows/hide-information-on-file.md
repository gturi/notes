---
tags:
  - windows
  - ntfs
---
#  Hide information on a file using ADS

Every file in an NTFS file system has at least one stream, its default stream, which is the normal viewable content.
Files can also contain one or more **alternate data streams** (ADSs), which leave the default one unchanged. Each ADS needs to be named.

With the following command, you can create an ADS called "hiddenInfo" and edit it with notepad.

```bat
notepad file-name:hiddenInfo
```

Running again the command will show the previously stored hidden information.

This technique can be leveraged for malicious purposes: to know if some files in a directory use it you can use: 

```bat
dir /r
```
