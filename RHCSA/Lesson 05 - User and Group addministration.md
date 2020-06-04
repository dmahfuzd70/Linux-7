Lesson: User and Group Administration
-------------------------------------
[root@desktopX ~]# less /etc/passwd  

 UID
 ------
 root : 0
 system user: 1 - 999
 regular user: 1000 +

[root@desktopX ~]# id root
[root@desktopX ~]# id student
[root@desktopX ~]# id bin 

[root@desktopX ~]# useradd tarek
[root@desktopX ~]# tail /etc/passwd    
[root@desktopX ~]# grep tarek /etc/passwd 

tarek: x: 1001: 1001:   :/home/tarek  :/bin/bash
  1    2   3     4    5       6           7

1 - username 
2 - user password info (/etc/shadow)
3 - userid (UID)
4 - groupid (GID): primary 
5 - user's comment/descriptions
6 - user's home dir
7 - user's shell

[root@desktopX ~]# id tarek
uid=1001(tarek) gid=1001(tarek) groups=1001(tarek)

[root@desktopX ~]# tail /etc/shadow    ; user password related info
[root@desktopX ~]# grep tarek /etc/shadow    ; tarek password related info
  
[root@desktopX ~]# useradd mahfuz   ; user create
[root@desktopX ~]# useradd mamun

[root@desktopX ~]# passwd mahfuz
[root@desktopX ~]# tail /etc/shadow 

Alt+Ctrl+F2 -- Alt+Ctrl+F6

login: mahfuz        ; login as regular user
pass: 123            ; (note: please numlock on)

[mahfuz@desktopX ~]$ exit


[root@desktopX ~]# groupadd trainer      ; group add
[root@desktopX ~]# groupadd staff         ; group add

[root@desktopX ~]# tail /etc/group     ; group related info

trainer :x:  1003: 
   1     2    3   4
 1 - group name
 2 - group password info (/etc/gshadow)
 3 - gid 
 4 - group members
 
[root@desktopX ~]# grep trainer /etc/group  ; check trainer group
trainer:x:1003:

[root@desktopX ~]# usermod -G trainer tarek  ;existing user modify
[root@desktopX ~]# useradd -G trainer  belal  ; newuser to group

[root@desktopX ~]# grep trainer /etc/group 
trainer:x:1003:tarek,belal

[root@desktopX ~]# useradd ikbal
[root@desktopX ~]# passwd ikbal

[root@desktopX ~]# usermod -G trainer,staff  ikbal ; single user assign to multiple groups
[root@desktopX ~]# grep staff /etc/group 
[root@desktopX ~]# grep trainer /etc/group 

[root@desktopX ~]# id ikbal

[root@desktopX ~]# useradd -u 3000 roman ; user careate with UID 
[root@desktopX ~]# grep roman /etc/passwd

[root@desktopX ~]# groupadd -g 3100 admin
[root@desktopX ~]# tail /etc/group

[root@desktopX ~]# groupmod -n faculty trainer  ;change group name
[root@desktopX ~]# tail /etc/group

[root@desktopX ~]# gpasswd -d ikbal staff       ; remove from group
[root@desktopX ~]# grep staff /etc/group 

[root@desktopX ~]# tail /etc/shadow
[root@desktopX ~]# passwd -d ikbal               ; password remove 
[root@desktopX ~]# tail /etc/shadow

Login regular user:
-------------------
Linux GUI terminal: 1   (Alt + Ctrl + F1)
Linux Command Terminal: (Alt+Ctrl+F2  -  Alt + Ctrl + F6)

Login as root user
-----------------
[root@desktopX ~]# id rafat
[root@desktopX ~]# useradd rafat
[root@desktopX ~]# passwd rafat
[root@desktopX ~]# grep rafat /etc/passwd 
rafat:x:1003:1003::/home/rafat:/bin/bash

[root@desktopX ~]# usermod -c "Linux X student" rafat

[root@desktopX ~]# grep rafat /etc/passwd 
rafat:x:1000:1000:Linux X student:/home/rafat:/bin/bash

[root@desktopX ~]# grep rafat /etc/passwd
rafat:x:1003:1003::/home/rafat:/bin/bash

[root@desktopX ~]# mkdir -p /newhome/rafat2 
[root@desktopX ~]# usermod -d /newhome/rafat2 rafat

[root@desktopX ~]# grep rafat /etc/passwd
rafat:x:1003:1003: :/newhome/rafat2:/bin/bash

[root@desktopX ~]# cat /etc/shells 

[root@desktopX ~]# id student

[root@desktopX ~]# grep student /etc/passwd  
[root@desktopX ~]# usermod -s /sbin/nologin  student     ;change user shell
[root@desktopX ~]# grep student  /etc/passwd  
student :x:1002:1002::/home/student :/sbin/nologin

Check: Alt + Ctrl +  F3 (use username password)

[root@desktopX ~]# usermod -s /bin/bash student    ;shell enable

Check: Alt + Ctrl +  F3  (use username password)

[root@desktopX ~]#  usermod -L student     ; user  account    lock
[root@desktopX ~]# grep student /etc/shadow
student: ! $.............../:16106:10:30:7:::

Check: Alt + Ctrl +  F3 (use username password)

[root@desktopX ~]#  grep student /etc/shadow
[root@desktopX ~]# tail /etc/shadow

[root@desktopX ~]#  usermod -U student    ; user  account  unlock
[root@desktopX ~]#  grep student /etc/shadow
student: $.............../:16106:10:30:7:::

[root@desktopX ~]# userdel rafat ; user delete without home dir

or

[root@desktopX ~]# userdel -r rafat  ; delete user with home dir
[root@desktopX ~]# cat /etc/passwd

[root@desktopX ~]# groupdel admin	; groupdel 
[root@desktopX ~]# tail /etc/group        

[root@desktopX ~]# useradd mahedi
[root@desktopX ~]# passwd mahedi

Alt+Ctrl+F3

Login:  mahedi
 Pass: *****

[mahedi@desktopX ~]$ useradd rasel
 -bash: /usr/sbin/useradd: Permission denied

 What is SUDO do ?
 -----------------
 Sudo allows an regular user to execute a specifc command or a 
 group of commands or all commands as the superuser.
 
  regular user: rumon, rony, lucky
	=> rm,cp,mv,
	=> mkdir,touch
	=> pwd,free -m,
	=> ping, df -HT
	=> ip addr, tail

 Command run from: /bin/

  super user: root
	=> useradd, passwd, groupadd
	=> reboot, systemctl 
	=> shutdown, poweroff
 	=> setenforce 

 Command run from: /sbin

Note:  [root@desktopX ~]# which useradd  ; command for location of   useradd command 
       [root@desktopX ~]# which pwd

 Editing sudo configuration File:
 --------------------------------
 Rules 1: permit for all
 -----------------------
 [root@desktopX ~]# visudo 

 :set nu
 
 92   root    ALL=(ALL)    ALL
 93   mahedi  ALL=(ALL)    ALL    ; mahedi allow for any command

:x (save and exit)
 
Test:
-----
Send Kye (VM): Alt + Ctrl + F3

[mahedi@desktopX ~]$ useradd rasel
 
[mahedi@desktopX ~]$ sudo useradd rasel
[sudo] password for mahedi: ****
[mahedi@desktopX ~]$ tail /etc/passwd
[mahedi@desktopX ~]$ exit

Working with /etc/shadow file:
------------------------------
[root@desktopX ~]# useradd lucky
[root@desktopX ~]# passwd lucky

[root@desktopX ~]# tail /etc/shadow | grep lucky
lucky:$6$ciiMIfom$cPpqBIf2NOwan2byi5BUA.G6D0iM/g.tw7fcUyLDWIs.nbp0:17711:0:99999:7:::


[root@desktopX ~]# chage -l lucky          ;password info 

Last password change                                    : MM DD, YYYY
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7

password: P@ssword123  (new password)

[root@desktopX ~]# chage lucky
 Minimum Password age [0]: 3
 Maximum Password age [99999]: 30
 Last Password Changed (YYYY-MM-DD): Press Enter (today)
 Password Expiration Warning [7]: 5 
 Password Inactive [-1]: 5 
 Account Expiration Date (YYYY-MM-DD) [-1]: YYYY-MM-DD

 note: If press Enter account never expire 


[root@desktopX ~]# tail /etc/shadow | grep lucky

[root@desktopX ~]# date
[root@desktopX ~]# date MMDDHHMMYY    
[root@desktopX ~]# date

[root@desktopX ~]# vim /etc/login.defs  ;user password related info 

 25  PASS_MAX_DAYS   99999
 26  PASS_MIN_DAYS   0
 27  PASS_MIN_LEN    5
 28  PASS_WARN_AGE   7

[root@desktopX ~]# chage -d 0 lucky

===================================  x  ===========================

Extra:

 Rules 2: shutdown disallow
 --------------------------
 52  Cmnd_Alias SHUTDOWN = /usr/sbin/shutdown, /usr/sbin/poweroff, /usr/sbin/reboot
 92  mahedi ALL=(ALL) ALL, !SHUTDOWN
 
  Rules 3: permit for specific command
 ------------------------------------
 52  Cmnd_Alias MAHEDI = /usr/sbin/useradd, /usr/sbin/userdel 
 92  mahedi ALL=(ALL)  MAHEDI

Rules 4: permit group (support) for specific command
 ----------------------------------------------------

 52  Cmnd_Alias SUPPORT = /usr/sbin/fdisk, /usr/sbin/passwd, 
 109 %support  ALL=(ALL)       SUPPORT

 =================== The End ===============







