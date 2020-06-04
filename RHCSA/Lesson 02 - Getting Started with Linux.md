Working with Linux CLI:
=======================

[student@hostX Desktop] $ 
[root@hostX    Desktop] # 
  1       2        3    4 
 
 	1: user name
 	2: hostname
	3: user's current location
	4: user types (root: #, regular user: $)

Linux User's Types:
-------------------
 => root user: Administrator (#)
 => system user: service (mail/ftp/games/daemon)-cannot login 
 => regular user: student, guest, sakib ($)

Working with Linux Shells & Terminal:
------------------------------------
[student@hostX Desktop] $ echo $SHELL
[student@hostX Desktop] $ chsh -l

Working with linux CLI:
-----------------------
 => Physical Consoles (Alt + Ctrl+ F1 - Alt + Ctrl + F6) 
	=> Alt + Ctrl + F1: GUI
	=> Alt + Ctrl + F2-F6: CMD

 => Pseudo Consoles (/dev/pts/x) or connection from GUI 
 => Telnet (unencrypted, TCP port 23)
 => SSH (encrypted, TCP Port 22)

 Alt + Ctrl + F1 => GUI                       - :0
 Alt + Ctrl + F2 => new CMD terminal (F2-F6)  - tty2-tty6

 => Press Ctl + Alt + F2
 
 Login: student
 pass: ****** 

 [student@hostXDesktop]$ su 
  Password: ******

 [root@hostX ~]# exit 
 [student@hostX ~]$ exit

 screen:
 ------
 $ ctrl + l = srceen clear
 $ ctrl + shift + "t" => new terminal (tab) (GUI)

Linux Command Syntax/Pattern:
-----------------------------
    # command [optoin (-)] argument

 ex # ping -c 4 172.25.11.254

[student@hostX Desktop]$ cd 
[student@hostX ~]$ ls      ;list of files and dir.
[student@hostX ~]$ ll      ;file and dir properties
[student@hostX ~]$ ls -l
[student@hostX ~]$ ls -la  ; details list with hidden files and dir

      blue - dir  
      b&w - file
      red - compress (rpm/zip/rar)
      green - execute file
      yellow - device (terminal/cd/dvd/usb/hdd)
      cyan - link file
      magenta - Picture/image/media

[student@hostX ~]$ pwd   ; present working directory

  "~" 	  => home dir
  "/" 	  => root partition (My Computer)
  "/root" => root's home dir
  "/home" => user's home
    i.e.: /home/student

[student@hostX~]$ => user's home dir 

 Working with Linux More Commands:
 ================================
[student@hostX ~]$ w
[student@hostX ~]$ who
[student@hostX ~]$ whoami 
[student@hostX ~]$ hostname
[student@hostX ~]$ tty
[student@hostX ~]$ date
[student@hostX ~]$ cal 
[student@hostX ~]$ cal 2017
[student@hostX ~]$ runlevel              ;(5-GUI, 3-CMD, n-none)
[student@hostX ~]$ uname -r       	  	  ; kernel version
[student@hostX ~]$ uname        		  ; OS name
[student@hostX ~]$ cat /etc/redhat-release     ; redhat/centos version
[student@hostX ~]$ top			  ; task manager
[student@hostX ~]$ lastlog			  ; login details
[student@hostX ~]$ free -m			  ; RAM Info
[student@hostX ~]$ uptime			  : system UPtiem info
[student@hostX ~]$ ip addr

[student@hostX ~]$ history  			; list of privious command
[student@hostX ~]$ !25			; 45 no command
[student@hostX ~]$ history -c 		; clear all previous history
[student@hostX ~]$ history

 Shutdown
===========
[root@hostX ~]# init 0
[root@hostX ~]# poweroff
[root@hostX ~]# shutdown -h now
[root@hostX ~]# shutdown -h 5 now  ; shutdown after 5 min

restart:
=======
[root@hostX ~]# reboot
[root@hostX ~]# init 6
[root@hostX ~]# shutdown -r now
[root@hostX ~]# shutdown -r 5 now    ; restart after 5 min
   
Linux Directory Structure:
-------------------------
[student@hostX ~]$ cd /
[student@hostX /]$ ls

bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr

 bin - user binary files ( executed by regular user)
 boot - system boot related file 
 dev - system device files (dvd/cd/hdd/fd)
 etc - all server & system configuration file
 home - regular user home dir
 lib - system libary files locations. libraries needed to execute the binaries in /bin/ and /sbin/. 
 media - system defaut mount point (DVD/ISO/SOFTware)
 mnt - mount point (DVD/HDD/USB)
 opt - optional (empty)
 proc - Also called 'proFS' system process related info (CPU,RAM, Process, Driver, Modules and Kernel)
 root - root user (superuser) home dir.
 run - service running data. Runtime data for processes started since the last boot.  
 sbin  - system binary ( used by root user)
 srv - Sort for Service. User's (/home/*) service related data. Like WWW, FTP etc.
 sys - Sort for system. '/sys' directory as a virtual filesystem (sysfs) mounted under /sys. similar as proc.
 tmp - temporary files (deleted after 10 days)
 usr - thirdparty software install location
 var - varibale file (mail/log/hosting/ftpdata)

[student@hostX ~]$ cd  [enter]  ; back to home dir
[student@hostX ~]$ cd /
[student@hostX /]$ ls
[student@hostX /]$ cd

        =>  cd  /dir1/dir2/dir3 [path]  ; Linux

	=>  C:\dir1\dir2\dir3>       ; Windows 

[student@hostX ~]$ cd /var/log
[student@hostX log]$ ls
[student@hostX log]$ cd  ..  ; one step/dir back 
[student@hostX var]$ cd  -  ; back to previous dir
[student@hostX ~]$ cd
[student@hostX ~]$ ls
[student@hostX ~]$ cd Music
[student@hostX Music]$ cd ../Videos
[student@hostX Videos]$ ls
[student@hostX Videos]$ pwd
[student@hostX Videos]$ cd 
[student@hostX ~]$ mkdir linux
[student@hostX ~]$ ls
[student@hostX ~]$ cd linux
[student@hostX linux]$ ls
[student@hostX linux]$ pwd
[student@hostX linux]$ mkdir lesson02
[student@hostX linux]$ cd lesson02
[student@hostX lesson02]$ ls
[student@hostX lesson02]$ pwd

Test: Graphically (home/...........)

 class work:
 -----------
[student@hostX lesson02]$ touch file1              ; file create
[student@hostX lesson02]$ ll
[student@hostX lesson02]$ touch test1 test2 test3    ; multiple files create with single command
[student@hostX lesson02]$ ll
[student@hostX lesson02]$ mkdir dir1 dir2 dir3 ; multiple dirs create with single command
[student@hostX lesson02]$ ll
[student@hostX lesson02]$ touch file{1..10}
[student@hostX lesson02]$ touch user{1,2,3}
[student@hostX lesson02]$ touch user-{tarek,lima,liza}
[student@hostX lesson02]$ ll
[student@hostX lesson02]$ mkdir -p dir1/dir2/dir3 
[student@hostX lesson02]$ ll dir1
[student@hostX lesson02]$ touch dir1/dir2/test
[student@hostX lesson02]$ cd dir1/dir2/
[student@hostX dir2]$ pwd
[student@hostX dir2]$ ll 
[student@hostX dir2]$ cd /home/student/linux/lesson02

or

[student@hostX dir2]$ cd ../../
[student@hostX lesson02]$ rm -rf *
[student@hostX lesson02]$ ls

Working with Hidden file/dir:
----------------------------
[student@hostX lesson02]$ mkdir  .training
[student@hostX lesson02]$ touch  .csl
[student@hostX lesson02]$ ll
[student@hostX lesson02]$ ls -la   ; a for hidden file/dir
[student@hostX lesson02]$ mv .training training	 ; file rename
[student@hostX lesson02]$ ll

[student@hostX lesson02]$ touch test1
[student@hostX lesson02]$ mkdir linux1

[student@hostX lesson02]$ ll

 d rwx rwx  r-x .  2   student student   6 	Feb  4 18:06 	linux1
 - rw- rw-  r-- .  1   student student   0 	Feb  4 18:06 	testY

 1 2a  2b   2c 2d  3    4        5       6 	    7	          8

 1: file/dir types
 2: file/dir permission:   2a: user permission, 2b: group permission, 2c: others permission 2d: ACL Permission
 3: file/dir link (Hard Link)
 4: file/dir owner
 5: file/dir group owner
 6: file/dir size (byte)
 7: file/dir modify date
 8: file/dir name

Linux FIle & Dir types:
------------------------------
 d = directory     : regular directory
 l = link file    : /dev/stdin 
 s = socket        : /dev/log     
 - = regular file : text/any file
 p = Pipe file  : /dev/initctl        
 b = device CD/DVD/HDD : /dev/sdb, /dev/sr0
 c = character device (serial/prallel/printer): /dev/tty

s - A socket file is used to pass information between applications/process for communication purpose

[root@hostX lesson02]$ cd /dev
[student@hostX dev]$ ll
[student@hostX dev]$ cd -  
[root@hostX lesson02]$ rm -rf *
[root@hostX lesson02]$ ls 

 FIle/dir Permission:
 ---------------------------
 r = read
 w = write
 x = execute
 - = no permission
 . = ACL File Permission (+)

Copy/paste/remove/rename/delete
===============================
[student@hostX lesson02]$ touch file1
[student@hostX lesson02]$ mkdir dir1
[student@hostX lesson02]$ ls
[student@hostX lesson02]$ cp file1 file2          ; file copy
[student@hostX lesson02]$ ll
[student@hostX lesson02]$ cp file1 /home/student       ; same name
[student@hostX lesson02]$ cp file1 /home/student/file3  ; diffrent name
[student@hostX lesson02]$ cd 
[student@hostX ~]$ ls
file1
file3
[student@hostX ~]$ cd -
[student@hostX lesson02]$ cp /etc/passwd  .  ; copy to current dir
[student@hostX lesson02]$ ls 
[student@hostX lesson02]$ cp /etc/hostname  /home/student
[student@hostX lesson02]$ cd 
[student@hostX ~]$ ls
[student@hostX ~]$ cd -
[student@hostX lesson02]$ cp dir1 linux99      ; wrong command
[student@hostX lesson02]$ cp -r dir1 linux99   ; copy directory 
[student@hostX lesson02]$ cp -r /etc .         ; full '/etc' directory copy to 'current location'
[student@hostX lesson02]$ ls 
[student@hostX lesson02]$ mv file2 file4   ; move or rename
[student@hostX lesson02]$ ls 
[student@hostX lesson02]$ mv file4 /home/student
[student@hostX lesson02]$ ls
[student@hostX lesson02]$ rm file1            ; file remove 
[student@hostX lesson02]$ ls
[student@hostX lesson02]$ rm linux99        ; wrong command
[student@hostX lesson02]$ rm -r linux99     ; delete empty dir
[student@hostX lesson02]$ rm -r etc         ; delete dir with content and confirmation
 C^
[student@hostX lesson02]$ rm -rf etc        ; delete dir with contains and without confirmation
[student@hostX lesson02]$ ls
[student@hostX lesson02]$ rm -rf *  ; delete everything from curent dir
[student@hostX lesson02]$ ls

 ===================== The End =======================


























