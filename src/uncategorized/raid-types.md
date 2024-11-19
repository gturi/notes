---
tags:
  - raid
---

# RAID types

https://www.seagate.com/products/nas-drives/raid-calculator/

### RAID 0
- Requires a minimum of 2 disks.
- Multiple disks are seen as one, data are split between all the disks.
- No redundancy! If one disk breaks, all data is lost.
- Big advantage: the data transfer throughput increases since the data are partially copied to each disk.
- With 2 disks the capacity is doubled, but the reliability is halved.

### RAID 1
- Requires a minimum of 2 disks.
- An exact copy of the data is stored on each disk, thus if one breaks you can replace it.
- Unfortunately, to store the same data you will need to spend X times based on how many disks you want to use as backups.
- With 2 disks the reliability is doubled, but also the expense.

### RAID 5
- Requires a minimum of 3 disks.
- Data is partially stored on each disk using XOR and "parity bits" based mechanism which allows to reconstruct the data of a disk based on the information stored on the others.
- Up to 1 disk can break before losing all the data.
- Usually, users opt for 3 or 4 disks based solutions, since the more hard drives connected to the array, the slower the read and write speeds become.
- Paranoid takes:
    - What if a second disk breaks while reconstructing the first one? All the data is still lost.
    - What if during the reconstruction of the broken disk an read error occurs? During this step, the other disks must be read in their entirety, just one read error can compromise the data. Disks specs like Unrecoverable Read Error (URE) or Nonrecoverable Read Error (NRE) provide information on the probability of read errors. Usually, it ranges from 1/10^14 or 1/10^15. The problem is that URE does not increase with the increase of the capacity of a disk, so with bigger disks there is an higher chance that an URE occurs (percentage-wise). Fortunatly, URE is a metric about bit read error, which is recoverable by integrity checks. Issues arise when two read errors happen on the same disk sector. Modern disks may implement strategies to recover also from those, but if too many read errors happen in a short period the data will probably be unrecoverable.
    - To avoid that too many read errors happen on the same sector, it is a good practice to schedule periodic disk scrubs (read the disk in its entirety).
    - For these reasons the suggested dimension for hard disks is 4TB.

### RAID 6
- Requires a minimum of 4 disks.
- Same as RAID 5, but uses Reed-Solomon algorithm for parity.
- Up to 2 disks can break before losing all the data.

### RAID 10 (1+0)
- Requires a minimum of 4 disks.
- Combines RAID 0 and RAID 1 by representing the disks as subgroups where the partial data will be stored.
- Reads and writes are faster than RAID 5 or 6 solutions, however only half of the space is available to store data.
- It handles the breakage of more than 1 disk, as long as it does not happen in the same subgroup.

## SnapRAID

https://www.snapraid.it/

- similar approach of RAID 5, but has the advantage that it does not require that all disks have the same size
- disks can be hotplugged in this RAID-like tool, since they are managed as indipendent storage units
- disks can have whatever filesystem they want, even different ones
- parity is easily configurable
