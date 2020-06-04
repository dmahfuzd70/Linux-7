 Application => System Tools => Virtual Machine Manager:

Working with Linux File/Directory permission & owernship:
==========================================================
    [root@dektopX ~]# mkdir linux
    [root@dektopX ~]# cd linux
    [root@dektopX linux]# ls
    [root@dektopX linux]# mkdir lesson06
    [root@dektopX linuxY]# cd lesson06
    [root@dektopX lesson06]# touch test1
    [root@dektopX lesson06]# mkdir newdir
    [root@dektopX lesson06]# cp /etc/passwd  .          ; copy to current dir
    [root@dektopX lesson06]# ls -l

    d rwxr-xr-x. 2 root root    6 Sep 26 09:33 newdir
    - rw-r--r--. 1 root root    0 Sep 26 09:33 test1
    - rw-r--r--. 1 root root 1389 Sep 26 09:33 passwd
    1      2     3   4   5     6          7      8

    1 - Linux File/dir types
    2 - user/group/others permission, (.) => ACL Permission
    3 - file Hard link
    4 - file/dir owner 
    5 - file/dir group owner
    6 - file/dir size
    7 - created/modify date & Time
    8 - file/dir name

working with link file:
========================
Type of link file:
------------------
 1) hard link  - same inode number (backup)
 2) soft link - different inode number (if original file delete, linked file delete too).

        [root@dektopX lesson06]# ls -li 
        [root@dektopX lesson06]# cat passwd
        [root@dektopX lesson06]# ln -s  passwd passwd-soft        ;softlink
        [root@dektopX lesson06]# ln passwd passwd-hard		;hardlink
        [root@dektopX lesson06]# ll
        [root@dektopX lesson06]# cat passwd-hard
        [root@dektopX lesson06]# cat passwd-soft
        [root@dektopX lesson06]# ls -li
        [root@dektopX lesson06]# echo goodbye >> passwd
        [root@dektopX lesson06]# tail passwd
        [root@dektopX lesson06]# tail passwd-hard
        [root@dektopX lesson06]# tail passwd-soft
        [root@dektopX lesson06]# rm -f passwd
        [root@dektopX lesson06]# ll
        [root@dektopX lesson06]# tail passwd-soft
        [root@dektopX lesson06]# ln -s /home/student/Documents mylink

        Hardlink - file
        softlink - file & folder (start with 'l')

 Field no: 2 (Permission) 
 -----------------------
    [root@dektopX lesson06]#  ll

     - rw-r--r--. 1 root root    0 Sep 26 09:52 test1
     d rwxr-xr-x. 2 root root 4096 Sep 26 09:33 newdir
         2

 subfield:
 ---------
     - rw- r-- r-- .  = 644 (file)
     d rwx r-x r-x .  = 755 (dir)  
        u   g   o  A  

     u = user 
     g = group
     o = others
     r = read (4)
     w = write (2)
     x = execute (1)
     - = no permission (0)
     . = ACL Permission (+)

      **** Special Permission (S,s,T,t)

    Group:      users                   others
    ======     =========               ========
    students:   malek, sumon, sadat    all (except group members)

 File/dir permission for new file/dir:
=====================================
     dir: 755
     file: 644

    Maximum File Permission: 666      =>  (rw-rw-rw-)
    Maximum Directory Permission: 777 =>  (rwxrwxrwx)

    [root@dektopX lesson06]# groupadd students
    [root@dektopX lesson06]# useradd -G students malek
    [root@dektopX lesson06]# useradd -G students sadat
    [root@dektopX lesson06]# useradd -G students sumon

    [root@dektopX lesson06]# useradd tamim

    [root@dektopX lesson06]# grep students /etc/group
    students:x:5005:malek,sadat,sumon

     user: (malek) : full permission
     group: students: read
     others: others : no

    [root@dektopX lesson06]# ls -l
     -rw-r--r--. 1 root root 0 Jun 14 19:55 test1

    [root@dektopX lesson06]# chown malek test1
    [root@dektopX lesson06]# ls -l
    -rw-r--r--.  1 malek root 0 Jun 14 19:55 test1

    [root@dektopX lesson06]# chgrp students test1
    [root@dektopX lesson06]# ls -l 
    -rw-r--r--. 1 malek students 0 Sep 26 09:52 test1

    [root@dektopX lesson06]# chmod 740 test1
    [root@dektopX lesson06]# ls -l
    -rwxr-----. 1 malek students 0 Jun 14 19:55 test1

 Testing:
 --------
     (user)
    [root@dektopX lesson06]# su malek
    [malek@dektopX lesson06]$ ls
    [malek@dektopX lesson06]$ echo this is malek > test1       ; malek can rw
    [malek@dektopX lesson06]$ cat test1
    [malek@dektopX lesson06]$ exit

     (Group)
    [root@dektopX lesson06]# su sumon
    [sumon@dektopX lesson06]$ ls
    [sumon@dektopX lesson06]$ cat test1
    [sumon@dektopX lesson06]$ echo this is sumon > test1       ; read only
    [sumon@dektopX lesson06]$ exit

     (Others)

    [root@desktopX lesson06]# su tamim
    [tamim@desktopX lesson06]$ cat test1   ; access denied 
    [tamim@desktopX lesson06]$ echo this is tamim > test 
    [tamim@desktopX lesson06]$ exit

Linux SUID, SGID and Sticky Bit Concept:
----------------------------------------
     1 = Sticky bit (t, T)  - 3rd field
     2 = SGID  (s,S) 	- 2nd field
     4 = SUID (s,S)  	- 1st field

 Workint with Test SUID:
 ----------------------
    [root@dektopX lesson06]# which passwd
    [root@dektopX lesson06]# ls -l /bin/passwd
    -rwsr-xr-x. 1 root root 27832 Jun 10  2014 /bin/passwd

    [root@dektopX lesson06]# ls -l /usr/bin/ls
    -rwxr-xr-x. 1 root root 117616 Jun 10  2014 /usr/bin/ls

    [root@dektopX lesson06]# su student
    [student@dektopX lesson06]$ ls /root
    [student@dektopX lesson06]$ exit
    [root@dektopX lesson06]# ls -l /usr/bin/ls
    -rwxr-xr-x. 1 root root 117616 Jun 10  2014 /usr/bin/ls

    [root@dektopX lesson06]# chmod 4755 /usr/bin/ls       ; SUID  Permission

    [root@dektopX lesson06]# ls -l /usr/bin/ls
    -rwsr-xr-x. 1 root root 117616 Jun 10  2014 /usr/bin/ls

    [root@dektopX lesson06]# su student
    [student@dektopX lesson06]$ ls /root
    [student@dektopX lesson06]$ exit

 Working with SGID:
 ------------------
     [root@dektopX lesson06]# mkdir resource
     [root@dektopX lesson06]# ls -ld resource 
      drwxr-xr-x. 2 root root 6 Sep 18 22:13 resource

     [root@dektopX lesson06]# grep students /etc/group
      students:x:1008:malek,sumon,sadat

    [root@dektopX lesson06]# chmod 770 resource 
    [root@dektopX lesson06]# chgrp students resource 

    [root@dektopX lesson06]# ls -ld resource 
     drwxrwx---. 2 root students 6 Sep 18 22:13 resource

    [root@dektopX lesson06]# su malek
    [malek@dektopX lesson06]$ touch resource/malek1
    [malek@dektopX lesson06]$ exit
    [root@dektopX lesson06]# su sadat
    [sadat@dektopX lesson06]$ touch resource/sadat1
    [sadat@dektopX lesson06]$ exit
    [root@dektopX lesson06]# ll resource 

    [root@dektopX lesson06]# chmod 2770 resource   ; SGID Permission
    [root@dektopX lesson06]# ls -ld resource 

    [root@dektopX lesson06]# su sumon
    [sumon@dektopX lesson06]$ touch resource/sumon1
    [root@dektopX lesson06]# ll resource 

 Working with Sticky Bit:
 ------------------------
    [root@dektopX lesson06]# ls -ld /tmp

    [root@dektopX lesson06]# mkdir mydir1
    [root@dektopX lesson06]# mkdir mydir2
 
    [root@dektopX lesson06]# ll
    [root@dektopX lesson06]# chmod 777 mydir1     ; regular permission
    [root@dektopX lesson06]# chmod 1777 mydir2    ; sticky bit

    [root@dektopX lesson06]# ls -ld mydir1 mydir2
     drwxrwxrwx. 2 root root 6 Sep 18 21:53 mydir1
     drwxrwxrwt. 2 root root 6 Sep 18 21:53 mydir2

     [root@dektopX lesson06]# touch mydir1/file1
     [root@dektopX lesson06]# touch mydir2/file2

    [root@dektopX lesson06]# su student
    [student@dektopX lesson06]$ rm mydir1/file1
    [student@dektopX lesson06]$ rm mydir2/file2

     rm: cannot remove ‘mydir2/file2’: Operation not permitted

    ======================== Thank you ==================
