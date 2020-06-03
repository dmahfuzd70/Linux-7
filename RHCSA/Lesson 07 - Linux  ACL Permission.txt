Linux Advnced File Permission (ACL)
-----------------------------------

[root@desktopX ~]# cd
[root@desktopX ~]# mkdir linux/lesson07 -p
[root@desktopX ~]# cd linux/lesson07
[root@desktopX lesson07]# ls 
[root@desktopX lesson07]# touch tutorial profile
[root@desktopX lesson07]# ll

-rw-r--r--. 1 root root 0 Jun 28 17:00 tutorial
-rw-r--r--. 1 root root 0 Jun 28 17:00 profile
 
 [root@desktopX lesson07]# useradd jack
 [root@desktopX lesson07]# useradd rose
 [root@desktopX lesson07]# useradd tomy

ACL Test:
----------
[root@desktopX lesson07]# getfacl tutorial
 # owner: root
 # group: root 
 user::rw-
 group::r--
 other::r--

User ACL:
---------
[root@serverX lesson07]# setfacl -m u:rose:rw,jack:rw  profile
[root@serverX lesson07]# ll
-rw-rw-r--+ 1 root root 0 Jun 28 17:00 profile

[root@serverX lesson07]# getfacl profile

[root@serverX lesson07]# su jack
[jack@serverX lesson07]$ cat profile
[jack@serverX lesson07]$ echo hello > profile
[jack@serverX lesson07]$ cat profile
[jack@serverX lesson07]$ exit

[root@serverX lesson07]# su rose
[rose@serverX lesson07]$ cat profile
[rose@serverX lesson07]$ echo hi >> profile
[rose@serverX lesson07]$ exit

[root@serverX lesson07]# su tomy
[tomy@serverX lesson07]$ cat profile
[tomy@serverX lesson07]$ echo welcome >> profile
[tomy@serverX lesson07]$ exit

***** Permission Denied

Group acl:
----------
[root@serverX lesson07]# groupadd staff
[root@serverX lesson07]# setfacl -m g:staff:rw-  tutorial 
[root@serverX lesson07]# getfacl tutorial

Directory Permission:
---------------------
[root@serverX lesson07]# mkdir acldir
[root@serverX lesson07]# touch acldir/file1
[root@serverX lesson07]# touch acldir/file2
[root@serverX lesson07]# setfacl -R -m u:rose:rwx acldir    ;-R (recursively)
[root@serverX lesson07]# getfacl acldir
[root@serverX lesson07]# ls -l acldir

ACL Remove (user):
-------------------
[root@serverX lesson07]# setfacl -x u:rose: profile
[root@serverX lesson07]# getfacl profile

ACL Remove (Gruop):
------------------
[root@serverX lesson07]# setfacl -x g:staff: tutorial
[root@serverX lesson07]# getfacl tutorial 

Remove ACL from File:
---------------------
[root@serverX lesson07]# setfacl -b tutorial 


        ------------ Thank you  ----------














