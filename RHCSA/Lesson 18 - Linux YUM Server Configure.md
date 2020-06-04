RPM Installation:
-----------------
 1. Manually (rpm command)
 2. Automatically (YUM Server)

Yum Server Configure:
---------------------
 => Requirments:
 ---------------
	=> DVD (centos/redhat)
	=> 5GB Free Space in "/var"
	=> Default FTP Dir: "/var/ftp/pub"
  	=> Packages: 
		1) createrepo - server (already installed)
		2) vsftpd (for yum client)

 Step 01: Check free space "/var"
 ======= ------------------------
 [root@hostX ~]# df -HT
 Filesystem    Type     Size   Used  Avail Use% Mounted on
 /dev/sda6     ext4      16G   7.1G   7.7G  48% /

            Note: By default "/var location under "/" partition"


Package Query:
--------------
 [root@hostX Packages]# rpm -qa | grep vsftpd
 [root@hostX Packages]# rpm -qa | grep createrepo

 Step 02: Mount DVD under "/mnt"
 ======= -------------------------  		
  [root@hostX ~]# mount /dev/sr0 /mnt   ; here "sr0" is dvd device.
  mount: block dvice /dev/sr0 is write-protected, monting read-only
  [root@hostX ~]# cd /mnt
  [root@hostX mnt]# ls
  [root@hostX mnt]# cd Packages
  [root@hostX Packages]#  ls

ISO Mount:
----------
  [root@hostX ~]# mount /root/Desktop/Cen....(Tab-.iso)  /mnt
  [root@hostX ~]# cd /mnt 
  [root@hostX mnt]# ls
  [root@hostX mnt]# cd Packages
  [root@hostX Packages]#  ls

 Step 03: Dependency Install 
 ======= -------------------
  [root@hostX Packages]# rpm -ivh vsftpd-(Press...Tab....from keyboard

  [root@hostX Packages]# systemctl restart vsftpd.service
  [root@hostX Packages]# systemctl enable vsftpd.service

  [root@hostX Packages]# setenforce 0
  [root@hostX Packages]# systemctl stop firewalld
  [root@hostX Packages]# systemctl disable firewalld

 Step 04: RPM copy to "/var/ftp/pub"
 ======= ---------------------------  
[root@hostX Packages]# cd /var/ftp/pub
[root@hostX pub]# ls
[root@hostX pub]# rm -rf * 
[root@hostX pub]# ls
[root@hostX pub]# cd /mnt
[root@hostX mnt]# cp -rv Packages /var/ftp/pub

Note: All "Packages" will be copy to "/var/ftp/pub". If old 
rpm exist, please delete first.

Step 05: Create YUM Repository 
======= ----------------------
[root@hostX ~]# createrepo -v /var/ftp/pub/Packages

Step 06: yum server confiugraiton file 
======= ------------------------------
[root@hostX mnt]# cd /etc/yum.repos.d
[root@hostX yum.repos.d]# ls
[root@hostX yum.repos.d]# mkdir old
[root@hostX yum.repos.d]# ls
[root@hostX yum.repos.d]# mv *.repo old
[root@hostX yum.repos.d]# ls
 old
[root@hostX yum.repos.d]# vim server.repo
 [server]
 name = yum server
 baseurl = file:///var/ftp/pub/Packages
 enabled = 1
 gpgcheck = 0

:x (save and exit)

[root@hostX yum.repos.d]# yum clean all  ; Previous yum server cache clear
[root@hostX yum.repos.d]# yum list all   ; list of available rpm current repo
[root@hostX yum.repos.d]# yum repolist  

YUM Server Test:
----------------
[root@hostX yum.repos.d]# yum info ftp
[root@hostX yum.repos.d]# yum install ftp -y  ; test command
[root@hostX yum.repos.d]# yum install gnote -y 

[root@hostX yum.repos.d]# yum remove gnote -y

[root@hostX yum.repos.d]# ifconfig br0

====> move to virtual machine 
 
YUM client Configure:
--------------------
[root@serverX ~]# ping 172.25.11.254   ;(your yum hostX br0 IP)

[root@serverX ~]# cd /etc/yum.repos.d
[root@serverX yum.repos.d]# ls
[root@serverX yum.repos.d]# mkdir old
[root@serverX yum.repos.d]# mv *.repo old
[root@serverX yum.repos.d]# ls
 old
[root@serverX yum.repos.d]# vim client.repo
 [client]
 name = yum client
 baseurl = ftp://172.25.11.X/pub/Packages           ;X= YUM Server's IP
 enabled = 1    
 gpgcheck = 0

:x

[root@serverX yum.repos.d]# yum clean all  ; cache clear 
[root@serverX yum.repos.d]# yum repolist 
[root@serverX yum.repos.d]# yum install php -y   ; test installation

[root@serverX yum.repos.d]# yum remove php -y

========== Configure DVD as YUM Repos =========
DVD Mount:
----------
[root@serverX ~]# mount /dev/sr0 /media

[root@serverX ~]# cd /etc/yum.repos.d
[root@serverX yum.repos.d]# ls
[root@serverX yum.repos.d]# mkdir old
[root@serverX yum.repos.d]# mv *.repo old
[root@serverX yum.repos.d]# ls
 old
[root@serverX yum.repos.d]# vim dvd.repo
[dvd]
 name=dvd repo
 baseurl=file:///media
 enabled=1
 gpgcheck=0

[root@serverX ~]# yum list all
[root@serverX ~]# yum install mysql

 --------------------- Thank you ------------------















