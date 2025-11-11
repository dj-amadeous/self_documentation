# external hard drive will not mount
1) search for list of hard drives recognized by system
$sudo fdisk -l
in the case of 11/11/2025, I was presented a list like the following...
------------------------------------------------------------
Disk /dev/loop13: 50.93 MiB, 53399552 bytes, 104296 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop14: 500 KiB, 512000 bytes, 1000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop15: 576 KiB, 589824 bytes, 1152 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sda: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: PSSD T7 Shield  
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: dos
Disk identifier: 0x398be4da

Device     Boot Start        End    Sectors   Size Id Type
/dev/sda1  *     2048 1953522112 1953520065 931.5G 83 Linux
------------------------------------------------------------
(Note: Not all output is displayed)
In this case we are looking for the t7 shield, so we will see an entry like the following
------------------------------------------------------------

Disk /dev/nvme0n1: 1.82 TiB, 2000398934016 bytes, 3907029168 sectors
Disk model: Samsung SSD 9100 PRO with Heatsink 2TB  
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 392618D1-2056-4726-8C55-58CEEB471C88

Device           Start        End    Sectors  Size Type
/dev/nvme0n1p1    2048    2203647    2201600    1G EFI System
/dev/nvme0n1p2 2203648 3907028991 3904825344  1.8T Linux filesystem
-----------------------------------------------------------

The whole entry is the t7_shield, butthe /dev/nvem0n1 entry is the whole entry NOT the *partition*
we want to mount the partition, in this case the "/dev/nvme0n1p2" partition.

perform the following
1) create mount point
$sudo mkdir -p /media/$user/samsung_t7
(in this case, $user is evan (or me))
2) try to mount the disk
$sudo mount -t auto /dev/nvme0n1p1 /media/$user/samsung_t7

the partition should be mounted.
note: the auto keyword allows linux to automatically detect the partition format and is better than forcing it to a particular format with ntfs.
