Linux Firewall with Firewalld:
------------------------------
 Types of Linux Firewall:
------------------------
	=> Application based (squid/PAM/tcpwrappers)
	=> kernel based: firewalld

	 firewalld which also called linux firewall/netfilter integrated with linux kernel.

 Filtering:
 ---------
	 => layer3: IP/Subnet/icmp
	 => layer4: tcp/udp/port/tls/ssl or 21,22,25,53,80,443,161,110,123,
	 => Layer7: dns,ftp,http,smtp,pop3,squid,imap,snmp,nfs,tftp,ntp,ssh,https

 NAT & Portforwarding:
---------------------
	 => Masquerading   (PAT/NAT Overload)
	 => Portforwarding (port map)

Working with Firewalld:
-----------------------
	=> By directly editing configuration files in '/etc/firewalld/'.
	=> By using the graphical firewall-config tool
	=> By using firewall-cmd from the command line

Physical Machine:
=================
Firealld enabled and check:
---------------------------
	[root@localhost ~]# systemctl enable firewalld
	[root@localhost ~]# systemctl restart firewalld
	[root@localhost ~]# systemctl status firewalld

Mask the iptables to avoid conflict: (Optional)
------------------------------------
	[root@localhost ~]# systemctl status iptables
	[root@localhost ~]# systemctl mask iptables
	[root@localhost ~]# systemctl mask ip6tables

	[root@localhost ~]#  firewall-cmd --state

	[root@localhost ~]# hostnamectl set-hostname hostX.example.com
	[root@localhost ~]# bash
	[root@hostX ~]#

	Applications > Sundry > Firewall

	or

	[root@desktopX ~]# firewall-config 

Firewalld Zones:
---------------
	 -> public (default)
	 -> internal
	 -> external
	 -> trusted
	 -> home
	 -> DMZ
	 -> block
	 -> work
	 -> drop

	=============== Move to Virtual Machine =========

	[root@localhost ~]#  hostnamectl set-hostname firewallX.example.com
	[root@localhost ~]#  bash

	[root@firewallX ~]# ifconfig
	[root@firewallX ~]# ifup eth0
	[root@firewallX ~]# ifconfig

	[root@firewallX ~]# firewall-cmd --state

Check the Firewalld in CMD Mode:
--------------------------------
	[root@firewallX ~]# systemctl enable firewalld
	[root@firewallX ~]# systemctl restart firewalld
	[root@firewallX ~]# systemctl status firewalld

Mask the iptables to avoid conflict: (Optional)
------------------------------------
	[root@firewallX ~]# systemctl mask iptables
	[root@firewallX ~]# systemctl mask ip6tables

Check the firewall configure in CMD Mode:
-----------------------------------------
	[root@firwallX ~]# firewall-cmd --state
	[root@firwallX ~]# firewall-cmd --list-all 

	[root@firwallX ~]# firewall-cmd --get-default-zone
	   public (output)

	[root@firwallX ~]# firewall-cmd --permanent --zone=public --list-all
	  interfaces: 
	  sources: 
	  services: dhcpv6-client ssh 
	  ports: 
	  masquerade: no
	  forward-ports: 
	  icmp-blocks: 
	  rich rules: 

Set the zone, if no zone defined:
---------------------------------
	[root@firwallX ~]# firewall-cmd --set-default-zone home

Check the firewall configure in CMD Mode:
-----------------------------------------
	[root@firwallX ~]# firewall-cmd --get-default-zone

Set the zone, if no zone defined:
---------------------------------
	[root@firwallX ~]# firewall-cmd --set-default-zone public

Firewalld Testing with HTTPs service:
-------------------------------------
	[root@firwallX ~]# yum install httpd mod_ssl -y

	[root@firewallX ~]# echo 'welcome to https' > /var/www/html/index.html

	[root@firewallX ~]# systemctl restart httpd.service
	[root@firewallX ~]# systemctl enable httpd.service

Allow port through firewall-cmd:
-------------------------------
	[root@firewallX ~]# firewall-cmd --permanent --zone=public --add-port= 443/tcp
	success
	[root@firewallX ~]# firewall-cmd --reload
	success

	or

Allow service through firewall-cmd:
----------------------------------
	[root@firewallX ~]# firewall-cmd --permanent --zone=public --add-service=https

	success

	[root@firewallX ~]# firewall-cmd --reload
	success

	[root@firewallX ~]# firewall-cmd --zone=public --list-all

	****** Move to Graphical

	 => Open Firefox
	 => http://172.25.11.200+X
	 => https://172.25.11.200+X

Remove port through firewall-cmd: (Optional)
--------------------------------
	[root@serverX ~]# firewall-cmd --permanent --zone=public --remove-port= 443/tcp
	success
	[root@serverX ~]# firewall-cmd --reload
	success

Remove service through firewall-cmd: (Optional)
----------------------------------- 
	[root@serverX ~]# firewall-cmd --permanent --zone=public --remove-service=https
	success

	[root@serverX ~]# firewall-cmd --reload
	success

	[root@firewallX ~]# firewall-cmd --zone=public --list-all

Configure Firewalld in GUI Mode:
--------------------------------
	[root@desktop0 ~]# firewall-config

	Applications > Sundry > Firewall

	Configuration: Permanent
	Zone: public
	Ports => add => port & protocol (443) => ok
	Options => Reload firewalld

	[root@desktop0 ~]# firewall-cmd --list-all

Testing:
--------
	=> http://172.25.11.X or http://serverX.example.com (should fail)
	=> https://172.25.11.X or http://serverX.example.com (should succeed)

Filtering & Port Forwarding:
===========================
	01. SSH Reject Configure:
	-------------------------
	[root@desktopX~]# Firewall-config (GUI)
		=> Rich Rule: 
		=> Rich Rule => Add => Family [IPv4]=> Element [Port]: 22
		=> Action: Reject
		=> Source: X.X.X.X 
		=> OK
		=> Reload Firewall

02.Local Port Forward (Basic Rule):
------------------------------------
	[root@desktopX~]# Firewall-config (GUI)
	   =><= Permanent => Public
			  => Port Forwarding
			  => Add 
			  => Source => Protocol: tcp, Port: 5080 (src)
			  => Local Forwarding: Yes 
			  => Port: 22
			  => Reload Firewall

	Note: Remove ssh (uncheck from service)

	[root@serverX ~]# ssh 172.25.11.100+X (refused)
	[root@serverX ~]# ssh -p 5080 172.25.11.100+X 

03.Remote Port Forward (Basic Rule):
-------------------------------------
	[root@desktopX~]# Firewall-config (GUI)
	   =><= Permanent => Public
			  => Port Forwarding
			  => Add 
			  => Source => Protocol: tcp, Port: 5080 (src)
			  => Local Forwarding: [No] 
			  => Forward to Another Port - Yes
			  => IP Address:  172.25.11.254 (Remote Machine)
			  => Port: 22
			  => Reload Firewall
	Masquarading: Yes
	Note: SSH Must Enabled on Destination

	[root@serverX ~]# ssh 172.25.11.200+X (refused)  - Server IP
	[root@serverX ~]# ssh -p 5080 172.25.11.200+X  
 
04.Port Forward (Rich Rule):
-----------------------------
	   =><= Permanent => Public
			  => Rich Rule
			  => Add => Family (IPv4) => [Element] => Forward Port
			  => Source => Protocol: tcp, Port: 5080 (src)
			  => Local Forwarding: 22
			  => Source: Source IP - 172.25.11.100+X  (Neibough)
			  => Reload Firewall

	[root@server200 ~]# ssh 172.25.11.200+X (refused)
	[root@server100 ~]# ssh -p 5080 172.25.11.200+X (allowed)

	Or:
	--
	[root@server100 ~]# firewall-cmd  --permanent --add-rich-rule 'rule family="ipv4" 
	source address="192.168.X.0/24" service name="ssh" -j reject'

Deploy Outbound NAT Gateway 
----------------------------
Disable NetworkManager Service: 
-------------------------------
	[root@serverX ~]# systemctl stop NetworkManager.service
	[root@serverX ~]# systemctl disable NetworkManager.service
	[root@serverX ~]# systemctl status NetworkManager.service

Linux Gateway Machine (Public Network): eth0
--------------------------------------
	[root@serverX ~]# ip addr

	[root@serverX ~]# nmtui  
	IP: 172.25.11.200+X/24
	GW: 172.25.11.1
	DNS1: 8.8.8.8
	
	[root@serverX ~]# nmtui
	     => Activate 

Linux Gateway Machine (Private Network): eth1
---------------------------------------
	[root@serverX ~]# nmtui

	IP: 10.10.X.1/24

	[root@serverX ~]# 
	[root@serverX ~]# ip addr

	or

	[root@serverX ~]# ifconfig eth1 10.10.X.1 netmask 255.255.255.0

	########### Client Setting (Windows/Linux) ##############

	IP: 10.10.X.2/24
	GW: 10.10.X.1
	DNS1: 8.8.8.8
	DNS2: 4.2.2.2

	>ping 10.10.X.1

Enable IP Forwarding between Interfaces (Routing):
-------------------------------------------------
	[root@serverX ~]# cat /proc/sys/net/ipv4/ip_forward
	 0

	[root@serverX ~]# vim /etc/sysctl.conf
	 :set nu

	 12   net.ipv4.ip_forward = 1

	[root@serverX ~]# systemctl restart network

	[root@serverX ~]# cat /proc/sys/net/ipv4/ip_forward
	 1

Enable NAT/MASQUERADE:
---------------------
	[root@serverX ~]# systemctl restart firewalld.service
	[root@serverX ~]# systemctl enable firewalld.service
	[root@serverX ~]# systemctl status firewalld.service

	[root@serverX ~]# firewall-cmd --get-active-zones
	 Interfaces: eth0 eth1

	[root@serverX ~]# firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o 
	eth0 -j MASQUERADE -s 10.10.X.0/24

	[root@serverX ~]# firewall-cmd --permanent --direct --passthrough ipv4 -I FORWARD -i eth1 -j ACCEPT

	[root@serverX ~]# firewall-cmd --reload


	 Client
	 ------
	  XP> ping 10.10.X.1
	  XP> ping 172.25.11.1 
	  XP> ping 8.8.8.8

	##############################  Thank You  ############################



