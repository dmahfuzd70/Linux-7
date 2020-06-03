 Linux Partition Management:
============================
 Window File System: FAT32, NTFS
 Linux File sytem: ext2, ext3, ext4, XFS (current), 
		   vfat, swap, ZFS, GlusterFS.

 All device files location: /dev/*
 	     * hdd, dvd, cdrom, usb, serial, swap, tty
 
 Firmware:
 --------------
 Firmware is a software program or set of instructions programmed on a hardware device. 
 It provides the necessary instructions for how the device communicates with the other 
 computer hardware.
 Two Types firmware software:
 BIOS (Basic Input Output System)
 UEFI (Unified Extensitble Firmware Interface)

 Partition Table Stucture:
 ------------------------
 BIOS ==> MBR (Master Boot Record)   
 UEFI ==> GPT(GUID Partition Table) 

 MBR ==> 512-byte sectors, 2TB Max Partition Size
 GPT ==> 512-byte sectors, 9.4 Zettabytes Max Partition Size

 Total Partition: MBR - BIOS
 ----------------------------------------------------------
 Linux Partition = 15 (4 Primary + 11 Logical)
 Windows Partition: 63 C-Z, A- Floppy, B-zip

 Total Partition: GPT 
 -------------------------------------------------------------
 Total Partition: 128

 IDE/SATA/SAS/SCSI HDD: sda, sdb, sdc
 Virtual Machine: vda, vdb
 USB: sda1, sdb1
 DVD: sr0 
  
[root@desktopX ~]# fdisk -l ; all partition

 sda = 1st sata
 sdb = 2nd sata
 sdc = 3rd sata
 vda = 1st virtual disk
 vdb = 2nd virtual disk

 Linux partition ID:
 ------------------
 NTFS - 7
 Extended - 5
 ext3/ext4/xfs  - 83
 swap - 82
 LVM - 8e
 vfat - f
 RAID - fd

[root@hostX ~]# lsblk
[root@hostX ~]# df -HT | grep sda

[root@desktopX ~]# fdisk -l
[root@desktopX ~]# df -HT | grep vda

/dev/vda2      xfs       5.3G  1.1G  4.2G  20% /
/dev/vda1      xfs       521M   94M  427M  19% /boot

[root@desktopX ~]# lsblk

Create New Extended Partition:
-----------------------------
[root@desktopX ~]# fdisk -l

[root@desktopX ~]# parted
 (parted) print free

 Note: 'q' fore quite

[root@desktopX ~]# fdisk /dev/vda           ; MBR based 

 Command (m for help): m

   d   delete a partition
   l   list known partition types 
   m   print this menu
   n   add a new partition
   p   print the partition table
   q   quit without saving changes
   t   change a partition's system id
   w   write table to disk and exit

 Command (m for help): p
 Command (m for help): n

Partition type:
   p   primary (3 primary, 0 extended, 1 free)
   e   extended

Select (default e): e

Selected partition 4
First sector (12314624-16777215, default 12314624): {press Enter}
Using default value 12314624
Last sector, +sectors or +size{K,M,G} (12314624-16777215, default 16777215): {press Enter}
Using default value 16777215
Partition 4 of type Extended and of size 4.1 GiB is set

 Command (m for help): p
 Command (m for help): w

[root@desktopX ~]# fdisk -l

[root@desktopX ~]# partprobe /dev/vda       ; partition table update

Create New Logical Partition:
-----------------------------
[root@desktopX ~]# fdisk /dev/vda

Command (m for help): n
All primary partitions are in use
Adding logical partition 5  
First sector (12316672-16777215, default 12316672): {press Enter}

Last .. +sectors or +size{K,M,G}. default 16777215): +350M {press Enter}  
Partition 5 of type Linux and of size 350 MiB is set

Command (m for help): p
Command (m for help): w

[root@serverX ~]# partprobe /dev/vda

Do: 400 MiB Another Partition 

Partition Format Command:
-------------------------
[root@desktopX ~]# mkfs.xfs  /dev/vda5  

[root@desktopX ~]# mkfs.ext4  /dev/vda6

Partition Mount:
----------------
[root@desktopX ~]# mkdir /data1
[root@desktopX ~]# mkdir /data2
[root@desktopX ~]# df -HT | grep vda

[root@desktopX ~]# mount /dev/vda5 /data1
[root@desktopX ~]# mount /dev/vda6 /data2

[root@desktopX ~]# df -HT | grep vda

Filesystem    Type     Size   Used  Avail Use% Mounted on
/dev/vda2      xfs       5.3G  1.1G  4.2G  20% /
/dev/vda1      xfs       521M   94M  427M  19% /boot
/dev/vda5      xfs       364M   19M  345M   6% /data1
/dev/vda6      ext4      416M   19M  396M   1% /data2

[root@desktopX ~]# seq 10000000 > /data2/test
[root@desktopX ~]# df -HT | grep data
[root@desktopX ~]#  reboot

[root@desktopX ~]# df -HT | grep vda

Filesystem    Type     Size   Used  Avail Use% Mounted on
/dev/vda2      xfs       5.3G  1.1G  4.2G  20% /
/dev/vda1      xfs       521M   94M  427M  19% /boot

Parmanent Mount:
---------------
[root@desktopX ~]# blkid /dev/vda5
[root@desktopX ~]# vim /etc/fstab 

:set nu

UUID=1b42c7df-717a-420d-b054-81d5a594b5   /dataX  xfs   defaults  0 0

or 
  
########### Add the following lines ############

 /dev/vda5    			/data1    xfs     defaults     0  0
 /dev/vda6    			/data2    ext4     defaults    0  0

    1				 2	  3         4          5  6
  
 1 - partition
 2 - mountpoint
 3 - filesystem
 4 - options(quota,acl,ro,luks)
 5 - Dumping 

 *** specifies the option that need to be used by the dump program. 
 If the value is set to 0, then the partition is execluded from 
 taking backup and if the option is a nonzero value, the device will be backed up

 6 - file system check options

 *** mentions the fsck option. That is if the value is set to zero, 
 the device or partition will be excluded from fsck check and if 
 it is nonzero the fsck check will be run in the order in which 
 the value is set. The root partition will have this value set to
 one so that it will be checked first by fsck.

[root@desktopX ~]# mount -a  ;fstab file update
[root@desktopX ~]# mount 
[root@desktopX ~]# reboot

[root@desktopX ~]# df -HT | grep vda

 Partition delete
 ================

*** Warning ****

=> Remove fstab entry  (vim /etc/fstab)

=> Unmount        

=> Then delete

[root@desktopX ~]# vim /etc/fstab

[root@desktopX ~]# umount /dataX

[root@desktopX ~]# df -HT

[root@desktopX ~]# fdisk /dev/vda

Command (m for help): d
Partition number (1-5): 6

Command (m for help): p
Command (m for help): d
Partition number (1-4):4
Command (m for help): w

 Note: Before delete, you should unmount partion and delete fstab entry.

[root@desktopX ~]# partprobe /dev/vda

[root@desktopX ~]# fdisk -l
 /dev/vda1
 /dev/vda2
 /dev/vda3

Mount USB pendrive:
-------------------
[root@desktopX ~]# fdisk -l
Disk /dev/sdb: 32.2 GB, 32176472064 bytes
[root@desktopX ~]# mount /dev/sdb1 /mnt
[root@desktopX ~]# cd /mnt
[root@desktopX mnt]# ls
[root@desktopX mnt]# cp cv.docx /root/Desktop
[root@desktopX mnt]# cp /etc/passwd /mnt
[root@desktopX mnt]# cd
[root@desktopX ~]# umount /mnt
[root@desktopX ~]# cd /mnt
[root@desktopX mnt]# ls

 Mount DVD:
------------
[root@desktopX ~]# mount /dev/sr0 /media
[root@desktopX ~]# cd /media
[root@desktopX media]# ls
[root@desktopX media]# cd Packages
[root@desktopX Packages]# ls
[root@desktopX Packages]# cd 
[root@desktopX ~]# umount /media
[root@desktopX ~]# 

 ==============  The End ==============


   








































