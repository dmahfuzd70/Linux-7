NFS (Network File System):
=========================
    NFS, stands for Network File System, is a server-client protocol used for 
    sharing files between linux/unix to unix/linux systems. NFS enables you to mount a 
    remote share locally. You can then directly access any of the files on that remote share.

Reference Table:
----------------
    Package: nfs-utils
    Daemon: nfs-server, rpcbind, 

    NFS Server: 172.25.11.200+X (Virtual Machine)
    NFS Client: 172.25.11.100+X (Desktop Machine)

Change Host name:
-----------------
    [root@localhost ~]# hostnamectl set-hostname nfs-serverX.example.com
    [root@localhost ~]# bash
    [root@nfs-serverX ~]# ip addr

    [root@nfs-s ~]# nmtui

    [root@nfs-serverX ~]# ifup eth0

Step 01: Query and Package Install  
----------------------------------
    [root@nfs-serverX ~]# rpm -qa | grep nfs-utils
    [root@nfs-serverX ~]# yum install nfs-utils -y
    [root@nfs-serverX ~]# cat /proc/fs/nfsd/versions : Check NFS Version

    By default, NFS versions 3 and 4.x are enabled, version 2 is disabled. 
    NFSv2 is pretty old now, and there is no reason to enable it. 
    To verify it run the following cat command:


Step 02: Service Restart and enable
-----------------------------------
    [root@nfs-serverX ~]#  systemctl enable rpcbind.service
    [root@nfs-serverX ~]#  systemctl enable nfs-server.service

    [root@nfs-serverX ~]# systemctl start rpcbind.service
    [root@nfs-serverX ~]# systemctl start nfs-server.service

SELinux Disable: 
----------------
    [root@nfs-serverX ~]# setenforce 0

Allow NFS Service in Firewall:
------------------------------
    [root@nfs-serverX ~]# systemctl restart firewalld.service
    [root@nfs-serverX ~]# systemctl enable firewalld.service
    [root@nfs-serverX ~]# firewall-cmd --permanent --add-service=nfs
    [root@nfs-serverX ~]# firewall-cmd --permanent --add-service=rpc-bind
    [root@nfs-serverX ~]# firewall-cmd --permanent --add-service=mountd
    [root@nfs-serverX ~]# firewall-cmd --reload

Step 03: Create a shared directory
-----------------------------------
    [root@nfs-serverX ~]# mkdir /nfsshare -p
    [root@nfs-serverX ~]# cd /nfsshare
    [root@nfs-serverX nfsshare]# ls
    [root@nfs-serverX nfsshare]# mkdir download documents software project office 
    [root@nfs-serverX nfsshare]# ls
    documents  download  office  project  software
    [root@nfs-serverX nfsshare]# cd documents/
    [root@nfs-serverX documents]# ls
    [root@nfs-serverX documents]# touch doc{1..10}
    [root@nfs-serverX documents]# cd ..
    [root@serverX nfsshare]# cd download/
    [root@serverX download]# touch file1 file2 file3
    [root@serverX download]# cd ..
    [root@serverX nfsshare]# cd office
    [root@serverX office]# touch job cv profile
    [root@serverX office]# cd ..
    [root@serverX nfsshare]# cd project/
    [root@serverX project]# touch project{1..5}
    [root@serverX project]# cd ..
    [root@serverX nfsshare]# cd software/
    [root@serverX software]# touch office.exe vlc.exe skype.exe
    [root@serverX software]# cd ..
    [root@serverX nfsshare]# 

Step 04: Export shared directory on NFS Server:
----------------------------------------------
    [root@nfs-serverX ~]# vim /etc/exports

    /nfsshare/software      172.25.11.0/24(rw,sync,no_root_squash)
    /nfsshare/project       172.25.11.0/24(ro,sync,no_root_squash)
    /nfsshare/download      *(ro,sync,no_root_squash)
    /nfsshare/documents     172.25.11.100+X(rw,sync,no_root_squash)
    /nfsshare/office        *.example.com(rw,sync,no_root_squash)

Note:
====
    => /nfshare/* – shared directory
    => 172.25.11.0/24 – IP address range of clients
    => rw – Writable permission to shared folder
    => sync – Synchronize shared directory
    => no_root_squash – Enable root privilege
    => no_all_squash - Enable user’s authority

Step 05: Restart the NFS service and verify:
--------------------------------------------
    [root@nfs-serverX ~]# systemctl restart nfs-server
    [root@nfs-serverX ~]# exportfs -ra
    [root@nfs-serverX ~]# showmount -e localhost

Client Side Confiugration:
=========================

Step 06: Install NFS packages in your client system
---------------------------------------------------
    [root@hostX ~]# yum install nfs-utils 

Step 07: Service Restart and enable
-----------------------------------
    [root@hostX ~]# systemctl enable rpcbind.service
    [root@hostX ~]# systemctl enable nfs-server.service

    [root@hostX ~]# systemctl start rpcbind.service
    [root@hostX ~]# systemctl start nfs-server.service

    (Optional) systemctl start nfs-idmap.service

Step 08: View NFS Share on NFS Server 
-------------------------------------
    [root@hostX ~]# showmount -e 172.25.11.200+X     ; X is your NFS Server IP

    Export list for 172.25.11.200+X:
    /nfsshare/download  *
    /nfsshare/office    *.example.com
    /nfsshare/project   172.25.11.0/24
    /nfsshare/software  172.25.11.0/24
    /nfsshare/documents 172.25.11.100+X        ; X = Specific an IP address

Step 09: Mount NFS shares On clients
------------------------------------
    [root@hostX ~]# mkdir /nfsdata1   

    [root@hostX ~]# mount -t nfs 172.25.11.200+X:/nfsshare/project /nfsdata1
    [root@hostX ~]# cd /nfsdata1
    [root@hostX nfsdata1]# ls
    [root@hostX nfsdata1]# df -HT
    [root@hostX nfsdata1]# touch newproject         ; read only
    [root@hostX nfsdata1]# cd 
    [root@hostX ~]# umount /nfsdata1

    [root@hostX ~]# mkdir /nfsdata2  
    [root@hostX ~]# mount -t nfs 172.25.11.200+X:/nfsshare/software   /nfsdata2
    [root@hostX ~]# cd /nfsdata2
    [root@hostX nfsdata2]# df -HT
    [root@hostX nfsdata2]# touch newsoft          ;created
    [root@hostX nfsdata2]# cd 

Permanent Mount:
----------------
    [root@hostX ~]# vim /etc/fstab
     172.25.11.200+X:/nfsshare/software    /nfsdata2   nfs   defaults 0 0

    [root@hostX ~]# mount -a 

                                            ============== Thank You ================ 
                                            


AUTOFS Mount
------------

    One drawback of using /etc/fstab is that, regardless of how infrequently a user accesses the NFS mounted file system, the system must dedicate resources to keep the mounted file system in place. This is not a problem with one or two mounts, but when the system is maintaining mounts to many systems at one time, overall system performance can be affected. An alternative to /etc/fstab is to use the kernel-based automount utility. An automounter consists of two components:
    
    1. a kernel module that implements a file system, and
    2. a user-space daemon that performs all of the other functions.
    
    The automount utility can mount and unmount NFS file systems automatically (on-demand mounting), therefore saving system resources. It can be used to mount other file systems including AFS, SMBFS, CIFS, and local file systems.

                                            
