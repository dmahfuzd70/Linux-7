Configure Bridge Nework:
============================
    First check network interface

    [root@hostX ~]# rpm -qa | grep birdge-utils
    [root@hostX ~]# yum install bridge-utils -y

    [root@hostX ~]# systemctl stop NetworkManager.service
    [root@hostX ~]# systemctl disable NetworkManager.service

    [root@hostX ~]# cd /etc/sysconfig/network-scripts
    [root@hostX network-scripts]# ls

    [root@hostX network-scripts]# cp ifcfg-en*** ifcfg-br0
    [root@hostX network-scripts]# ls

    [root@hostX network-scripts]# vim ifcfg-en***
    DEVICE=en***
    HARDWARE=aa:bb:cc:dd:ee:ff
    TYPE=Ethernet
    BOOTPRO=none
    ONBOOT=yes
    BRIDGE=br0 (It will be bridge file name)

    [root@hostX network-scripts]# vim ifcfg-br0
    DEVICE=br0
    TYPE=Bridge
    IPADDR=192.168.11.X 
    NETMASK=255.255.255.0
    GATEWAY=192.168.11.1
    DNS1=8.8.8.8
    BOOTPRO=none
    ONBOOT=yes
    DELAY=0

    [root@hostX network-scripts]# systemctl restart network.service
    [root@hostX network-scripts]# cd
    [root@hostX]# ifconfig(Check for interface) 

Configure Virtualization
========================
    [root@hostX ~]# yum remove virt-manager* libvirt* qemu-kvm* -y

Check VT Support
-----------------
    [root@hostX ~]# grep -i vmx /proc/cpuinfo --color

    vmx - intel
    svm - amd

    [root@hostX ~]# cat /proc/cpuinfo 
    [root@hostX ~]# free -m

Packages
========
    => virt-manager (GUI interface)
    => libvirt (daemon + API collection)
    => qemu-kvm (emulator for virtual OS)

Package Installation
--------------------
    [root@hostX ~]# yum install virt-manager* libvirt* qemu-kvm* -y

    [root@hostX ~]# systemctl restart libvirtd.service  
    [root@hostX ~]# systemctl enable libvirtd.service 

Run Virtual Machine Mananger:
-----------------------------
    Application => System Tools => Virtual Machine Manager (VMM)

Download CentOS ISO:
--------------------
    => Open Mozila Firefox
    => ftp://172.25.11.254/pub

Create a ISO (Optional):
------------------------ 
    [root@hostX ~]# dd if=/dev/sr0 of=/root/Desktop/centos7.iso 

Run VM:
=======
    Application => System Tools => VMM

     -> VM Name: studentX

 Install Method: iso
 --------------
    -> IOS:/root/Desktop/Ce.........iso 

 Install Method: Network (FTP/HTTP/NFS)
 -----------------------

    Path: ftp://172.25.11.254/pub/centos7

    -> OS=Linux & RHEL 7
    -> Disk: /dev/sda* (10.GB)
    -> Interface: br0

    Time: Asia/Dhaka
    Installation Method: Minimal
    Size: /boot (500 MB), / (5GB), swap (512)
    Hostname: serverX.example.com 

Virtual Machine Location:
=========================
    => /var/lib/libvirt/images
 
IP configure:
============
    [root@localhost ~]# systemctl restart NetworkManager
    [root@localhost ~]# systemctl enable NetworkManager

    [root@localhost ~]# nmtui
     => Select eth0
     => edit
      IP: 192.168.11.200+X/24
      GW: 192.168.11.1
      DNS: 8.8.8.8

    [root@localhost ~]# nmtui
      => Activate 

    [root@localhost ~]# ip addr
    [root@localhost ~]# ping www.google.com

    [root@localhost ~]# ifconfig 

    [root@localhost ~]# yum install net-tools
    [root@localhost ~]# ifconfig 

Hostname:
---------
    [root@localhost ~]# hostnamectl set-hostname serverX.example.com
    [root@localhost ~]# bash 

=================== Thank you =====================





















