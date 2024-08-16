---
tags:
  - linux
  - games
  - steam
  - proton
  - backup
---

# Backup saves of steam games run via proton


## 1) Find the game id

### a) Use steam db

Search the game on https://steamdb.info, its game id is the one listed into App ID section.

### b) Use your local games directory

Under `"$HOME/.local/share/Steam/steamapps/common"`, locate the game directory of your interest.
Usually it has a file named `steam_appid.txt`, which stores the identifier used by steam to create the directory where game save data is stored. 

If `steam_appid.txt` is not present in the game directory, you can still identify the game id by searching it into `"$HOME/.local/share/Steam/userdata/XXXXXXXXX"` directory. 

`XXXXXXXXX` is your steam ID. There you will find one directory for each game, the directory names are the game ids.
Inside each directory there is usually a `remotecache.vdf` file that has some information on the game.
If you are lucky enough you will infer which game `remotecache.vdf` is referring to by reading its content.


## 2) Backup the save

Steam creates a new windows-like directory tree for each game save.

Games saves are stored inside `"$HOME/.local/share/Steam/steamapps/compatdata"` directory.

Use the `$steam_appid` to identify the correct game directory.

After that, the save files can be found in `/pfx/drive_c/users/steamuser/AppData/Roaming/$GAME_NAME` (`$GAME_NAME` format varies game by game).

Just backup that directory and restore it when needed.
