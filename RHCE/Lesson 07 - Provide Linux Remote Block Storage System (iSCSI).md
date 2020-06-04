Reference Table:
===============
    Packages: targetcli (server),  iscsi-initiator-utils (Client) 
    Daemon: target (server), iscsi (client)
    Port: 3260
    Protocol: TCP 
    Configuraiton file (client): /etc/iscsi/initiatorname.iscsi (Client)
    New Commands: targetcli (server), iscsiadm(client)  

Step 01: Package Install and Service enable
-------------------------------------------
    [root@localhost ~]# hostname
    localhost.localdomain

    [root@localhost ~]# hostnamectl set-hostname iscsiX.example.com
    [root@localhost ~]# bash

 **** Check IP Address

    [root@iscsi ~]# ifconfig
    [root@iscsi ~]# nmtui

    [root@iscsi ~]# rpm -qa | grep targetcli

    [root@iscsi ~]# yum install targetcli
    [root@iscsi ~]# systemctl enable target.service
    [root@iscsi ~]# systemctl start target.service

Step 02: Port Open on System Firewall 
------------------------------------------
    [root@iscsi ~]# firewall-cmd --permanent --add-port=3260/tcp
    [root@iscsi ~]# firewall-cmd --reload

Step 03: Create a 1GB Diks Space for LVM 
----------------------------------------
    [root@iscsi ~]# fdisk -l
    [root@iscsi ~]# fdisk /dev/vda

    => Create 1GB Partition primary partition (n, p (primary), (Press Enter), +1G, p, w)

    [root@iscsi ~]# partprobe /dev/vda

    => Make a PV for VG
    => Create a VG (iscsi_vg1)
    => Create a LV (iscsi_lv1)

    [root@iscsi ~]# pvcreate /dev/vda4            
    [root@iscsi ~]# vgcreate iscsi_vg1 /dev/vda4
    [root@iscsi ~]# lvcreate -n iscsi_lv1 -L 500M iscsi_vg1

Step 04: Configure iSCSI Target
-------------------------------
    1. Create Backstore device (serverX.disk1)
    2. Create iqn name (iqn.YYYY-MM.com.example:serverX)
    3. ACL for specific Client
    4. LUN Maping (map storage with name)
    5. Create Portal

    [root@serverX ~]# targetcli 
    /> ls
    o- / ........................................... [...]
      o- backstores ................................ [...]
      | o- block ................................... [Storage Objects: 0]
      | o- fileio .................................. [Storage Objects: 0]
      | o- pscsi ................................... [Storage Objects: 0]
      | o- ramdisk ................................. [Storage Objects: 0]
      o- iscsi ..................................... [Targets: 0]
      o- loopback .................................. [Targets: 0]

Creating block storage:
----------------------
    /> /backstores/block create serverX.disk1 /dev/iscsi_vg1/iscsi_lv1 
    /> ls

Create IQN for the target: (iSCSI Qualified Name)
--------------------------
    /> /iscsi create iqn.2019-MM.com.example:serverX
    /> ls

Create a ACL for desktopX:
--------------------------
    /> /iscsi/iqn.2019-MM.com.example:serverX/tpg1/acls create
                                    iqn.2019-MM.com.example:desktopX
    /> ls 

LUN Mapping:
-----------
    /> /iscsi/iqn.2019-MM.com.example:serverx/tpg1/luns create 
                                         /backstores/block/serverX.disk1

Create a Portal for Server on port 3260:
----------------------------------------
    /> /iscsi/iqn.2019-MM.com.example:serverx/tpg1/portals create 172.25.11.200+X (server)

    Note1: Here 'X' is server IP (vm)

    /> saveconfig
    /> exit

    Note: /> /iscsi/iqn.2019-MM.com.example:serverx/tpg1/portals
                                                     delete 0.0.0.0 3260
    [root@serverX ~]# setenforce 0

Client Configuration Part:
=========================
Step 05: Package Installation
-----------------------------

    [root@hostX ~]# rpm -qa | grep iscsi-initiator-utils

    [root@hostX ~]# yum install iscsi-initiator-utils

Step 06: Change default Initiator Name (Defined in ACL)
-------------------------------------------------------
    [root@hostX ~]# vim /etc/iscsi/initiatorname.iscsi

    InitiatorName=iqn.2019-MM.com.example:desktopX

Step 07: Service Restart and Check Status
-----------------------------------------
    [root@hostX ~]# systemctl enable iscsi
    [root@hostX ~]# systemctl restart iscsi
    [root@hostX ~]# systemctl status iscsi

Step 08: Target Discover
------------------------
    [root@hostX ~]# iscsiadm -m discovery -t st -p 172.25.11.200+X  

    172.25.11.200+X:3260,1 iqn.2019-MM.com.example:serverX (output)

Step 09: Connect the iSCSI Target and Verify:
---------------------------------------------
    [root@hostX ~]# lsblk 
    [root@hostX ~]# iscsiadm -m node -T iqn.2019-MM.com.example:serverX 172.25.11.200+X -l
    [root@hostX ~]# lsblk  
    [root@hostX ~]# systemctl restart iscsi
    [root@hostX ~]# fdisk -l 

Step 10: Create a Partition and Mount
-------------------------------------

    [root@hostX ~]# fdisk /dev/sdb          (i.e: sdb)
    [root@hostX ~]# partprobe /dev/sdX      (i.e: sdb)
    [root@hostX ~]# mkfs.xfs /dev/sdb       (i.e: sdb1)
    [root@hostX ~]# mkdir /iscsidisk1
    [root@hostX ~]# mount /dev/sdX /iscsidisk1
    [root@hostX ~]# df -HT

Step 11: Parmanent Mount:
------------------------
    [root@desktopX ~]# vim /etc/fstab

    /dev/sdX	/iscsidisk1        xfs    defaults        0 0

    [root@desktopX ~]# mount -a


    ================== Thanks =================











