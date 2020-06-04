
 SSH - Secure Shell
-------------------
    Application - Remotely server/router/firewalld administration using CLI mode
    Port no: 22 (default)
    Package: openssh-server (server), openssh-client(client)
    Configuration file: /etc/ssh/sshd_config 
    Daemon: sshd

Step by step configuration:
==========================

Step 01: Host Name Change:
-------------------------
    [root@localhost ~]# hostnamectl set-hostname ssh-serverX.example.com
    [root@localhost ~]# bash
    [root@ssh-serverX ~]# 

Step 02: Static IP Configure:
-----------------------------
    [root@ssh-serverX ~]# nmtui
        IP - 172.25.11.200+X/24
        GW - 172.25.11.1
        DNS - 8.8.8.8

    [root@ssh-serverX ~]# nmtui
        => activate

Step 03: RPM Query, install
---------------------------------
    [root@ssh-serverX ~]# rpm -qa | grep openssh-server
    openssh-server-6.4p1-8.el7.x86_64

    *** [root@ssh-serverX ~]# yum install openssh* -y  [if not found]

Step 04: Service Restart
------------------------
    [root@ssh-serverX ~]# systemctl restart sshd.service
    [root@ssh-serverX ~]# systemctl enable sshd.service
    [root@ssh-serverX ~]# systemctl status sshd.service

    [root@ssh-serverX ~]# netstat -ntlp | grep ssh

SSH Client:
-----------
    => Linux Client (defautl installed)
    => Windows Client (putty, ssh client)
       > putty > ssh > ip > port (22)

Move to hostX
----------------
    > ping 172.25.11.200+X   (ssh server)

SSH Login with Root User
---------------------------------
    [root@hostX ~]# ssh root@172.25.11.200+X

    Are you sure you want to continue connecting (yes/no)? yes
    root@172.25.11.200+X's password: ****** (remote PC) 

    [root@ssh-serverX ~]# who
    [root@ssh-serverX ~]# useradd user1
    [root@ssh-serverX ~]# passwd user1
     : 123
     : 123

    [root@ssh-serverX ~]# exit 

Linux with user1:
-----------------
    [root@hostX ~]# ssh  user1@172.25.11.200+X

    [user1@serverX~]$ su -
     : ********

    [root@ssh-serverX ~]# who
    [root@ssh-serverX ~]# exit
    [root@hostX ~]# 

Secure Copy (scp) from server:
-------------------------------
    [root@hostX ~]# scp root@172.25.11.200+X:/etc/passwd /root/Desktop

Secure Copy (scp) to serverX:
-------------------------------
    [root@hostX ~]# scp /etc/passwd root@172.25.11.200+X:/root/Desktop

Type of SSH Authentication:
---------------------------
    => Public key authentication (main authentication method)
    => Password authentication
    => Host-based authentication
    => Keyboard authentication

 Key Based Login:
 ----------------
    -> Public key: stored on each remote system
    -> Private key: stored only on local system

Encryption Algorithm Type:
--------------------------
    -> DSA and RSA
    -> ECDSA and Ed25519

    * Commonly used RSA

Password Less ssh login:
------------------------
    [root@hostX ~]# cd
    [root@hostX ~]# ls 
    [root@hostX ~]# ls -la
    [root@hostX ~]# cd .ssh
    [root@hostX .ssh]# ls 
     known_host (list of known hosts)  id_rsa.pub   id_rsa

    [root@hostX .ssh]# rm -rf * 

    [root@hostX .ssh]# ls
    [root@hostX .ssh]# ssh user1@172.25.11.200+X

    Are you sure you want to continue connecting (yes/no)? yes

    Ctlrl+C
 
    [root@hostX .ssh]# ls
     known_host (created)

    [root@hostX .ssh]# ssh-keygen  ; (Enter 3 Times)
    id_rsa.pub id_rsa

    [root@hostX .ssh]# cat id_rsa
    [root@hostX .ssh]# cat id_rsa.pub

    [root@hostX .ssh]# ssh-copy-id user1@172.25.11.200+X  (serverX)
    [root@hostX .ssh]# ssh user1@172.25.11.200+X

    [user1@ssh-serverX ~]$ cd .ssh 
    [user1@ssh-serverX .ssh]$ ls
     authorized_keys

    [user1@serverX .ssh]$ cat authorized_keys   ; same as public key of hostX

    =====>  Move to serverX (Virtual)

Change Default Port:
-------------------
    [root@ssh-serverX ~]# netstat -ntlp | grep ssh
    [root@ssh-serverX ~]# ss -ltn 

    [root@ssh-serverX ~]# vim /etc/ssh/sshd_config 
    :set nu

     17 #Port 22         ; old
     17  Port 2222      ; remove '#'

    [root@ssh-serverX ~]# systemctl restart sshd.service
    [root@ssh-serverX ~]# setenforce 0
    [root@ssh-serverX ~]# systemctl stop firewalld
    [root@ssh-serverX ~]# systemctl disable firewalld
    [root@ssh-serverX ~]# systemctl restart sshd.service

Verify curren SSH port:kc
-----------------------
    [root@ssh-serverX ~]# netstat -ntlp | grep ssh

    =====>  Move to hostX (Physical)

    SSH Server Login with Specif Port:
    ---  ------------------------------
    [root@hostX ~]# ssh user1@172.25.11.200+X   (default port)  - Refused

    [root@hostX ~]# ssh -p  2222  user1@172.25.11.200+X	; if user1 user

    [user1@ssh-serverX ~]$ exit

Disabled Root Login:
--------------------
    [root@ssh-serverX ~]# vim /etc/ssh/sshd_config 
    :set nu

     48 #PermitRootLogin yes        ;old
     48  PermitRootLogin no         ;new

    [root@ssh-serverX ~]# systemctl restart sshd.service

Test:
----
    [root@hostX ~]# ssh -p 2222 root@172.25.11.200+X

    [root@hostX ~]# ssh -p 2222 user1@172.25.11.200+X

          =============== Done =============



