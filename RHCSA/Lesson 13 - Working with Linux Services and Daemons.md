Linux Boot up Process:
----------------------
							     ==> CMD (tty)		
BIOS/UEFI => MBR/GPT => GRUB2 => Kernel => systemd => Target =>
						 	     ==> GUI (pts)
Change Default Boot Time:
-------------------------
[root@serverX ~]#  vim /boot/grub2/grub.cfg 
 :set nu

 63   set timeout=40

[root@serverX ~]# reboot

Working with Linux Kernel:
--------------------------
[root@serverX ~]# ifconfig
[root@serverX ~]# ping 8.8.8.8
[root@serverX ~]# ifup eth0
[root@serverX ~]# ifconfig 
[root@serverX ~]# ping 8.8.8.8

[root@serverX ~]# uname -r
[root@serverX ~]# yum list installed kernel-*
[root@serverX ~]# yum update kernel -y  
[root@serverX ~]# reboot
[root@serverX ~]# yum list installed kernel-*
[root@serverX ~]# uname -r 

systemd:
--------
[root@serverX ~]# pstree

One of the major changes in RHEL/CentOS 7.0 is the swtich to systemd,
a system and service manager, that replaces SysV and Upstart used in previous releases of Red Hat Enterprise Linux. systemd is compatible with SysV and Linux Standard Base init scripts. 

=> Fedora 17/RHEL/CentOS6 = SystemV/init
=> Ubuntu/SUSE/Debian = upstart
=> Fedora 18+/RHEL/CentOS7/Ubuntu-15 = systemd

RHEL7 or New Version:
---------------------
[root@serverX ~]# ls /lib/systemd/system/*.service

[root@serverX ~]# systemctl -t service | wc -l

Common Services:
----------------
 => Network: netwrok.service
 => Dynamic Network: NetworkManager.service
 => SSH Service: sshd.service
 => Web/WWW: httpd.service
 => Mail (Outgoing): postfix.service
 => Mail (Incoming): dovecot.service
 => Virtualization: libvirtd.service
 => NFS Service: nfs.service 
 => Samba Server: smb.service 
 => NetBIOS: nmb.service 
 => Firewall: firewalld.service 
 => Firewall: iptables.service 
 => FTP Server: vsftpd.service 
 => Time Server: ntpd.service 

Start/Stop/Restart/status Services with systemctl:
-------------------------------------------------
[root@desktopX ~]# systemctl status crond.service

[root@desktopX ~]# systemctl stop crond.service
[root@desktopX ~]# systemctl status crond.service

[root@desktopX ~]# systemctl restart crond.service
[root@desktopX ~]# systemctl reload XXXX.service

Enable / Disable services to run at boot time:
----------------------------------------------
[root@desktopX ~]# systemctl enable crond.service
[root@desktopX ~]# systemctl status crond.service

[root@desktopX ~]# systemctl disable crond.service
[root@desktopX ~]# systemctl status crond.service


Working with Runlevels:
-----------------------
[root@desktopX ~]# systemctl get-default 
graphical.target

[root@desktopX ~]# free -m

[root@desktopX ~]# systemctl set-default multi-user.target
[root@desktopX ~]# systemctl get-default 
multi-user.target

[root@desktopX ~]# reboot

[root@desktopX ~]# systemctl get-default 
multi-user.target

[root@desktopX ~]# free -m

[root@desktopX ~]# systemctl set-default graphical.target

[root@desktopX ~]# reboot

Recover Root Password:
---------------------
 => Reboot your system by pressing 'Ctr+Alt+Del 
 => Edit the default boot loader entry
 => Press 'e' to edit the current entry
 => Cursor navigate to the line that starts with 'linux16'.
 => Press 'End' button to move the cursor to the end of the line.
 => Append 'rd.break' to the end of the line.
 => Press 'Ctrl+x' to boot using the modified config.

switch_root:/# mount -o remount,rw /sysroot
switch_root:/# chroot /sysroot

sh-4.2# passwd    [press enter]

  : ******* (123456)
  : ******* (123456)

sh-4.2# touch /.autorelabel 
sh-4.2# exit 

switch_root:/# exit

=> Wait 2 Min

=> Login with new password

[root@desktopX ~]# passwd   ; press Enter

  : ******* (centos)
  : ******* (centos)
