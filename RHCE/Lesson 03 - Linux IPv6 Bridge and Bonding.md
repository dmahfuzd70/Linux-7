
        ###################################
	######  Bridge Interface ##########
	###################################

Verify Bridge Interface (serverX):
----------------------------------
[root@desktopX ~]# ifconfig
 
          br0: IP, MASK, GW, DNS
	 enp2s0/ens33: MAC

Bridge Interface Configure:
---------------------------
	[root@serverX ~]# systemctl stop NetworkManager.service 
	[root@serverX ~]# systemctl disable NetworkManager.servie
	[root@serverX ~]# ip link

	[root@serverX ~]# cd /etc/sysconfig/network-scripts
	[root@serverX network-scripts]# ls 
	ifcfg-eth0


	[root@serverX network-scripts]# cp ifcfg-eth1 ifcfg-br0
	[root@serverX network-scripts]# ls 
	ifcfg-eth0
	ifcfg-br0

	[root@serverX network-scripts]# vim ifcfg-eth0          ;physical INT
	DEVICE=eth0
	TYPE=Ethernet
	ONBOOT=yes
	BRIDGE=br0

	[root@serverX network-scripts]# vim ifcfg-br0           ; Bridge Int
	DEVICE=br0
	TYPE=Bridge
	IPADDR=172.25.11.200+X   
	NETMASK=255.255.255.0
	GATEWAY=172.25.11.1
	DNS1=8.8.8.8
	BOOTPROTO=none
	ONBOOT=yes
	DELAY=0

	[root@serverX network-scripts]# systemctl restart network.service 
	[root@serverX network-scripts]# cd
	[root@serverX ~]# ifconfig 

	[root@desktopX ~]# ping -I br0 172.25.11.254

        ###################################
	######   IPv6 Configure  ##########
	###################################

IPv6 Addressing:
================

	Description	IPv4			IPv6
	-----------	----			----z
	No bits		32			128
	Part		4			8
	Format		Dot (.)			(:)
	Number		Decimal			Hexa
	Class		5			N/A
	Example		192.168.11.10		2001:db8:0:abcd:12c::1

Types of IPv4
-------------
	 => Private IP (10.0.0.0/8, 172.16.0.0 - 172.31.255.255, 192.168.0.0-192.168.255.255)
	 => Public IP (except private block)

Types of IPv6:
-------------
	 => :: - all IP (similar as 0.0.0.0)
	 => ::1 - loopback (Similar as 127.0.0.1)
	 => fe80 - Link local address (similar as 169.254....)
	 => ff00 - Multicast (Similar as 224.0.0.0)
	 => fd00 - Unique local address (same as IPv4 Private)
	 => 2000 - Global unicast address (same as public IP)

	 # ping6 ::1
	 # ping 127.0.0.1 

Configure IPv6 to eth1 Interface:
=================================
	[root@serverX ~]# systemctl restart NetworkManager 
	[root@serverX ~]# systemctl enable NetworkManager

	[root@serverX ~]# nmcli connection show 
	NAME        	    UUID                         TYPE          DEVICE 
	eth0    5fb06bd0-7ffb-45f1-65f3e03              802-3-ethernet  eth0

	[root@serverX ~]# nmcli con modify "eth0" ipv6.addresses fd00:db08:abcd::1234:X/64

	[root@serverX ~]# nmcli con modify "eth0" ipv6.gateway fd00:db08:abcd::1234:1

	[root@serverX ~]# nmcli con modify "eth0" ipv6.dns fd00:db08:abcd::1234:254

	[root@serverX ~]# nmcli con modify "eth0" ipv6.method manual

	[root@serverX ~]# nmcli con up "eth0"

	[root@serverX ~]# ifconfig 

	[root@serverX ~]# ping6 fd00:db08:abcd::1234:254

Test with SSH:
-------------
	[root@serverX ~]# ssh fd00:db08:abcd::1234:254


	#######################################
	######  NIC Teaming (Bonding)   ########
	########################################

	Note: Add a NIC card Bridge mode from VM Option

Network Teaming:
----------------
	  -> Redundency 
	  -> Higher Throughput 

	=> Broadcast
	=> RoundRobin
	=> Active Backup - Failover    (HSRP)
	=> Load Balacing - GLBP
	=> LACP (etherchannel) 

	[root@serverX ~]# systemctl restart NetworkManager.service 
	[root@serverX ~]# systemctl enable NetworkManager.servie 

	[root@serverX ~]# nmcli connection show

	[root@serverX ~]# nmcli connection del 'NAME'

	 ******** Delete all device less connection 

	[root@serverX ~]# systemctl restart NetworkManager.service 

	[root@serverX ~]# nmcli connection show

	NAME                UUID                         TYPE            DEVICE 
	Wired connection 1  21899a89-65e3--69ce38a67fa0  802-3-ethernet  eth0   
	Wired connection 2  5fb06bd0-0bb0--d6edd65f3e03  802-3-ethernet  ens9 

	[root@serverX ~]# nmcli connection add con-name team0 ifname team0 type team config '{"runner": {"name": "activebackup"}}'

	[root@serverX ~]# ip addr
	[root@serverX ~]# nmcli connection show 

	NAME                UUID                    TYPE             DEVICE 
	Wired connection 1  21899a89-65e3-ce38a67fa 802-3-ethernet   eth0   
	Wired connection 2  5fb06bd0-0bb0-d6edd65f3  802-3-ethernet  eth1   
	team0               3a549377-21cb-22020aecb  team            team0

	[root@serverX ~]# nmcli connection modify team0 ipv4.addresses 172.25.11.200+X/24

	[root@serverX ~]# nmcli connection modify team0 ipv4.method  manual

	[root@serverX ~]# ip link 
	[root@serverX ~]# ip addr

	[root@serverX ~]# nmcli con add type team-slave con-name team0-port1 
	ifname eth0 master team0

	[root@serverX ~]# nmcli con add type team-slave con-name team0-port2 
	ifname ens9 master team0

	[root@serverX ~]# systemctl restart network
	[root@serverX ~]# ip addr

	[root@serverX ~]# teamdctl team0 state

	[root@serverX ~]# ping -I team0 172.25.11.254

	[root@serverX ~]# ip link set eth0 down
	[root@serverX ~]# teamdctl team0 state view

	[root@serverX ~]# teamdctl team0 state 

Connection Verify:
------------------
	[root@serverX ~]# cd /etc/sysconfig/network-scripts/
	[root@serverX network-scripts]# cat ifcfg-team0

	########################## Thanks ##########################


