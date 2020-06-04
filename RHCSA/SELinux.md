Modes of SELinux:
-----------------
=> Off - Doesn't do anything
=> Permissive - On but doesn't block
=> Enforcing - Turned on and blocks

Labels:
-------
=> Every object is labeled, taken all together it is called a context
=> system_u:object_r:syslog_t:s0 (MLS)

 - SELinux User      - (_u)
 - SELinux Role      - (_r)
 - SELinux Type      - (_t)
 - SELinux Sensitivity 

Packages:
---------
 => setools-console        - (role,policy)
 => Policycoreutils-python

View SELinux Security:
---------------------
[root@desktop ~]# cd /
[root@desktop /]# ls -Z

 - SELinux User      - (_u)
 - SELinux Role      - (_r)
 - SELinux Type      - (_t)
 - SELinux Sensitivity 

Related Package Install:
-----------------------
[root@desktop ~]# yum install setools-console policycoreutils-python

View SELinux Context:
---------------------
[root@desktop ~]# ls -laZ
[root@desktop ~]# ps -eZ

SELinux Related Information:
----------------------------
[root@desktop ~]# seinfo
[root@desktop ~]# seinfo -t
[root@desktop ~]# seinfo -t | wc -l

[root@desktop ~]# seinfo -t | grep sshd

[root@desktop ~]# seinfo -c  (class)
[root@desktop ~]# seinfo -cfile -x
[root@desktop ~]# seinfo -u

Users: 8
   sysadm_u
   system_u
   xguest_u
   root
   guest_u
   staff_u
   user_u
   unconfined_u

[root@desktop ~]# semanage port -l
[root@desktop ~]# semanage port -l | grep httpd

[root@desktop ~]# yum install httpd
[root@desktop ~]# ps -eZ | grep httpd
