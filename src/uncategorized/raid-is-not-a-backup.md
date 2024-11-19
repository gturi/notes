---
tags:
  - raid
  - backup
---

# RAID is not a backup

https://www.raidisnotabackup.com/

RAID is a common technique to provide **resiliency** and **availability** to a set of data and protect against one of the most common data loss scenarios: the failure of a disk.

The simplest type of RAID is a "mirror", which keeps two or more copies of data on two or more different disks. If one disk fails, the second copy is still available and no availability or data loss has occurred. You would usually see this for system disks in uptime-critical servers.

## Why do I need a backup?

Having a number of disks in RAID may seem like a backup, especially if you’re using a mirrored RAID mode like RAID-1 or RAID-10. But this is wrong!

RAID protects you against one and only one thing: a disk failure. It does not protect you against any of the following things:

- Multiple disk failures beyond the RAID level chosen.
- Failure of the RAID controller itself (if applicable), the computer running the RAID, or the environment containing the servers (e.g. a flood, fire, or theft).
- Data corruption from filesystem bugs, cosmic rays, or minor hardware or firmware failures, which can and do happen all the time - you usually just don’t notice and software works around it.
- Malicious or accidental deletion or modification of files, including by viruses, bad application writes, or administrative mistakes (e.g. `rm -ing` the wrong file or `mkfs` on an existing filesystem).

The adage is simple: "RAID replicates everything, instantly, even the stuff you don’t want it to."

## Fancy filesystems

There exist a number of storage systems with advanced, RAID-like features, including ZFS, btrfs, and Ceph. On the surface, features of these systems, like snapshots, might give you the illusion of additional protection, but don’t be deceived. Even the smartest most advanced storage engine is still susceptible to at least one, and almost always several, fatal failure modes that can destroy your data.

Just like RAID, advanced storage systems still aren’t backups.

## How to correctly back up?

A good rule of thumb is "3-2-1" rule: 3 copies, 2 different media types, 1 offsite (I.E. cloud).

Test your backups regularly, at least once a month; a backup is worthless if you can’t restore from it.
