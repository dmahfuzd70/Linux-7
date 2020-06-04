 IPV4 (LAN/MAN/WAN)
 ----------------------
 Class A: 0.0.0.0  - 126.255.255.255
 Class B: 128.0.0.0 - 191.255.255.255
 Class C: 192.0.0.0 - 223.255.255.255

 Private:
 --------
  A: 10.0.0.0 - 10.255.255.255
  B: 172.16.0.0 - 172.31.255.255
  C: 192.168.0.0 - 192.168.255.255

  Linux Network Management
  ------------------------:
  Windows NIC1: Ethernet1
  Windows NIC2: Ethernet2

[root@hostX ~]# ip link 
[root@hostX ~]# ip addr show         ; CMD or GUI 
[root@hostX ~]# ifconfig             ; (active)

[root@hostX ~]# ifconfig  eth0  ; sepecific LAN (br0, eth0, ens33, enp2s0)
 
  Linux (RHEL/CentOS-7)
 ---------------------
  Physical NIC : enp2s0 or ens33 (start with 'en')
  Virtual Machine NIC: eth0
  Virtual IP: enp2s0:1 or ens33:1 or eth0:1
  loopback: lo
  Bridge  : br0
  Wireless: wl
  VMware NIC: eno16777736 - ethernet card information 

[root@hostX ~]#  ifconfig 
[root@hostX ~]#  ip addr 
 
  ether 8C:89:A5:E4:F3:64 => MAC
  inet addr:172.25.11.X => IP Address
  Bcast:172.25.11.255
  Mask:255.255.255.0
  inet6 addr: IPv6 Link Local address 

[root@hostX ~]# route -n
[root@hostX ~]# route -n

Check Physical Connectivity:
---------------------------- 
[root@hostX ~]# ip link 
 state "UP"  or State "Down"

[root@hostX ~]# mii-tool enp2s0    ;(physical Machine)

enp2s0 : negotiated 1000baseT-FD flow-control, link ok
enp2s0 : no link (not connected)

Host Name Configure (Desktop Machine)
---------------------------
[root@localhost ~]# hostname 
 localhost.localdomain

[root@localhost ~]# hostnamectl set-hostname desktopX.example.com
[root@localhost ~]# bash

[root@desktopX ~]# cat /etc/hostname
 deskotpX.example.com

[root@desktopX ~]# hostname 

IP Configure:
-------------
[root@desktopX ~]# ip link
[root@desktopX ~]# ip addr

[root@desktopX ~]# ifconfig eth0

 IP Configure 
 --------------------------
  => Tempoary  (IP remove after system reboot)
  => Parmanet 

Temporary IP Address Configure:
-------------------------------
[root@desktopX ~]# ifconfig eth0
[root@desktopX ~]# ifconfig eth0  172.25.11.200+X/24
[root@desktopX ~]# ifconfig 

Gateway Testing
--------------------------
[root@desktopX ~]# ping 172.25.11.1 
64 bytes from 172.25.11.1 : icmp_seq=1 ttl=64 time=0.451 ms
64 bytes from 172.25.11.1 : icmp_seq=2 ttl=64 time=0.317 ms
C^

[root@desktopX ~]# ping -c 4 172.25.11.1 

Check Default Gateway:
---------------------
[root@desktopX ~]# route -n
[root@desktopX ~]# ping 8.8.8.8

[root@desktopX ~]# route add default gw 172.25.11.1

[root@desktopX ~]# ping 8.8.8.8

[root@desktopX ~]# ping www.google.com

[root@desktopX ~]# vim /etc/resolv.conf
 nameserver 8.8.8.8
 nameserver 4.2.2.2

[root@desktopX ~]# cat /etc/resolv.conf 
[root@desktopX ~]# ping www.google.com

[root@desktopX ~]# reboot

[root@desktopX ~]# ifconfig eth0

Delete: (Optional) 
-------
[root@desktopX ~]# route del default gw 172.25.11.1
[root@desktopX ~]# route -n


Enable and Disable:
-------------------
[root@desktopX ~]# ifdown eth0
[root@desktopX ~]# ifup eth0 

[root@desktopX ~]# ifconfig

Or:
---
[root@desktopX ~]# ip link set eth0 up
[root@desktopX ~]# ip link set eth0 down

 IP Client Configure
 -------------------
  => Static
  => dhcp : autmatically ip configure

[root@desktopX ~]# systemctl restart NetworkManager.service  
[root@desktopX ~]# systemctl enable NetworkManager.service 

[root@desktopX ~]# nmtui
                  => select 'Interface'
		  => edit connection
		  => Manual
		  => Add
		  => 172.25.11.200+X/24
		    GW - 172.25.11.1
		    DNS - 8.8.8.8

[x] Automatically Connect

[root@desktopX ~]# nmtui
             => Activate Connection 

[root@desktopX ~]# ifconfig eth0

Working with NetworkManager:
---------------------------
[root@desktopX ~]# nmcli connection show 

[root@desktopX ~]# systemctl restart NetworkManager 

[root@desktopX ~]# nmcli connection show 
NAME        	    UUID                     TYPE        DEVICE 
eth0  5fb06bd0-0bb0-7ffb  802-3-ethernet   eth0

[root@desktopX ~]# nmcli con modify 'eth0' ipv4.addreses 172.25.11.150+X/24 ipv4.gateway  172.25.11.1 ipv4.dns 8.8.8.8

[root@desktopX ~]# nmcli con mod 'eth0' ipv4.method manual
[root@desktopX ~]# nmcli con down 'eth0'
[root@desktopX ~]# nmcli con up 'eth0'

[root@desktopX ~]# nmcli con modify eth0 autoconnect yes

NAME = 'eth0'

[root@desktopX ~]# ifconfig eth0



[root@desktopX ~]# systemctl stop NetworkManager.service 
[root@desktopX ~]# systemctl disable NetworkManager.service 
[root@desktopX ~]# systemctl status NetworkManager.service 

Static (parmanent) IP configure:
--------------------------------
[root@desktopX ~]# cd /etc/sysconfig/network-scripts/
[root@desktopX network-scripts]# ls
[root@desktopX network-scripts]# vim ifcfg-eth0

	DEVICE=eth0                    ; no change
	TYPE=Ethernet                 ; no change
        HWADDR=AA:BB:CC:DD:EE:FF      ; no change
	BOOTPROTO=none               ; none=static, dhcp for dynamic 
	IPADDR=X.X.X.X    (172.25.11.200+X)  ;X is your IP address
	NETMASK=Y.Y.Y.Y   (255.255.255.0)
	GATEWAY=X.X.X.G   (172.25.11.1)
	DNS1=A.A.A.A      (4.2.2.2)
	DNS2=B.B.B.B	  (8.8.8.8)
	ONBOOT=yes                   ; must yes 

[root@desktopX ~]# systemctl restart network
[root@desktopX ~]# ifconfig eth0

==================== Thank you =======================



