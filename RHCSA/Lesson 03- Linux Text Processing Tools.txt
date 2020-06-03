Text Processing tools: (echo/Grep/tail/cat/head/less/cut/more/locate/find)
-------------------------------------------------------------------------
[student@desktopX ~]$ cd 
[student@desktopX ~]$ mkdir -p linux/lesson03
[student@desktopX ~]$ cd linux/lesson03/
[student@desktopX lesson03]$ ls
[student@desktopX lesson03]$ cp /etc/passwd .
[student@desktopX lesson03]$ ls
[student@desktopX lesson03]$ touch test
[student@desktopX lesson03]$ ls
 passwd test
[student@desktopX lesson03]$ cat test    
[student@desktopX lesson03]$ cat passwd
[student@desktopX lesson03]$ cat /etc/hostname

[student@desktopX lesson03]$ clear
[student@desktopX lesson03]$ echo hello         
[student@desktopX lesson03]$ echo hello > test
[student@desktopX lesson03]$ cat test
[student@desktopX lesson03]$ echo welcome > test         ; replace
[student@desktopX lesson03]$ cat test
[student@desktopX lesson03]$ echo hello world >> test    ; append 
[student@desktopX lesson03]$ cat test
[student@desktopX lesson03]$ history
[student@desktopX lesson03]$ history > command-list
[student@desktopX lesson03]$ ls
[student@desktopX lesson03]$ cat command-list

[student@desktopX lesson03]$ cat passwd        ; concatinate
[student@desktopX lesson03]$ cat -n passwd     ; concatinate
[student@desktopX lesson03]$ less passwd    ; Scrolling 
[student@desktopX lesson03]$ more passwd    ; file reading without cat (down)
[student@desktopX lesson03]$ head  passwd   ; 1st 10 l ines
[student@desktopX lesson03]$ tail passwd    ; last 10 lines read

[student@desktopX lesson03]$ tail -5 passwd    ; last 5 lines
[student@desktopX lesson03]$ head -5 passwd    ; 1st 5 lines
[student@desktopX lesson03]$ cat -n passwd | tail -5 

[student@desktopX lesson03]$ grep -n root passwd  ; search root keyword in passwd file
1 root:x:0:0:root:/root:/bin/bash
10 operator:x:11:0:operator:/root:/sbin/nologin

[student@desktopX lesson03]$ tail passwd | grep root  ;search root keyword in last 10 lines
[student@desktopX lesson03]$ head passwd | grep root

[student@desktopX lesson03]$ cat -n passwd  
[student@desktopX lesson03]$ cut -d ":" -f 1 passwd (Show as a culumn like nod1:1000)
[student@desktopX lesson03]$ cut -d ":" -f 1,3 passwd 
[student@desktopX lesson03]$ cut -d ":" -f 1-4 passwd 
[student@desktopX lesson03]$ cut -d ":" -f 1 passwd > userlist
[student@desktopX lesson03]$ cat -n userlist

[root@desktopX lesson03]# updatedb 
[root@desktopX lesson03]# locate sshd_config 
[root@desktopX lesson03]# locate -i .exe               ;including case sensitve

[root@desktopX lesson03]# find / -name mail            ; mail name file   
[root@desktopX lesson03]# find /var -name mail
[root@desktopX lesson03]# find /var -type d -name mail   
[root@desktopX lesson03]# find / -type f -name passwd 
[root@desktopX lesson03]# find / -size +100M            ; size more than 100MB
[root@desktopX lesson03]# find / -size -10M             ; size less than 10M 

Working with "Help and Manual":
------------------------------
[root@desktopX lesson03]# man passwd

[root@desktopX lesson03]# useradd --help
[root@desktopX lesson03]# passwd -?

[root@desktopX lesson03]# mandb 

[root@desktopX lesson03]# whatis yum.conf
[root@desktopX lesson03]# whatis passwd

The Info Command:
-----------------
[root@desktopX lesson03]# info passwd

Online Documentation:
---------------------
 docs.redhat.com
 
Class work:
===========
[student@desktopX lesson03]$ su 
  :****** 
[root@desktopX lesson03]# cat -n /var/log/messages

[root@desktopX lesson03]# grep "MM DD" /var/log/messages | cut -d " " -f 1-3 > log 


    ----------------------  Thank you -----------------











