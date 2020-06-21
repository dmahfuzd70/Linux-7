 Mail Server:
 ============
 
 Mail terminology
 ----------------
 <img src="/RHCE/Images/Mail-Infrustrucre.jpg" alt="Mail Terminology" width="816" height="300">
   
 
### MUA (Mail User Agent)
	Client application that allows receiving and sending emails. It can be a desktop application 
	such as Microsoft Outlook/Thunderbird/eudora or web-based such as Gmail/Hotmail 
	(the latter is also called Webmail). 

### MSA (Mail Submission Agent)
	A server program that receives mail from an MUA, checks for any errors, and transfers 
	it (with SMTP) to the MTA hosted on the same server.

### MTA (Mail Transfer Agent)
	A server application that receives mail from the MSA, or from another MTA. It will find 
	(through name servers and the DNS) the MX record from the recipient domain's DNS zone in
	order to know how to transfer the mail. It then transfers the mail (with SMTP) to another 
	MTA (which is known as SMTP relaying) or, if the recipient’s server has been reached, to the MDA. 

	Examples of MTAs are MS exchange, Postfix, Exim, Sendmail, qmail,Lotus.

### MDA (Mail Delivery Agent)
	A server program that receives mail from the server’s MTA, and stores it into the mailbox. 
	MDA is also known as LDA (Local Delivery Agent).

	An example is Dovecot, which is mainly a POP3 and IMAP server allowing an MUA to retrieve mail, 
	but also includes an MDA which takes mail from an MTA and delivers it to the server’s mailbox.

	Examples of POP3 Server (Dovecot)

### Mailbox: maildir/mbox
	The server’s mail storage. Maildir is a way of storing email messages. It is usually preferable over mbox.

### SMTP
	Protocol used by MUAs to send emails to an MSA. The recommended SMTP port for sending mail 
	(from an MUA to an MSA) is the port 587, which uses TLS encryption.

### IMAP/POP3
	Protocols used by MUAs to retrieve emails from a server mailbox. POP3 deletes the email messages from 
	the server after they have been downloaded. IMAP is usually preferable as it maintains all email messages 
	on the server, permitting management of a mailbox by multiple email clients.

### MX (Mail Exchanger) record
	A Mail Exchanger (MX) record in the DNS specifies which server is responsible for accepting email 
	addresses on behalf of a domain. The host name from the MX record must map to one or more address 
	record (A or AAAA) in the DNS, and must not point to any CNAME records.
 
 
 Email Protocols:
 ----------------
#### What is POP3 and which are the default POP3 ports
	Post Office Protocol version 3 (POP3) is a standard mail protocol used to receive emails from a remote 
	server to a local email client. POP3 allows you to download email messages on your local computer and read them 
	even when you are offline. Note, that when you use POP3 to connect to your email account, messages are downloaded 
	locally and removed from the email server. This means that if you access your account from multiple locations, 
	that may not be the best option for you. On the other hand, if you use POP3, your messages are stored on your 
	local computer, which reduces the space your email account uses on your web server.

	By default, the POP3 protocol works on two ports:

	Port 110 - this is the default POP3 non-encrypted port
	Port 995 - this is the port you need to use if you want to connect using POP3 securely

### What is IMAP and which are the default IMAP ports
	The Internet Message Access Protocol (IMAP) is a mail protocol used for accessing email on a remote web 
	server from a local client. IMAP and POP3 are the two most commonly used Internet mail protocols for 
	retrieving emails. Both protocols are supported by all modern email clients and web servers.

	While the POP3 protocol assumes that your email is being accessed only from one application, IMAP 
	allows simultaneous access by multiple clients. This is why IMAP is more suitable for you if you're 
	going to access your email from different locations or if your messages are managed by multiple users.

	By default, the IMAP protocol works on two ports:

	Port 143 - this is the default IMAP non-encrypted port
	Port 993 - this is the port you need to use if you want to connect using IMAP securely

### What is SMTP and which are the default SMTP ports
	Simple Mail Transfer Protocol (SMTP) is the standard protocol for sending emails across the Internet.

	By default, the SMTP protocol works on three ports:

	Port 25 - this is the default SMTP non-encrypted port
	Port 2525 - this port is opened on all SiteGround servers in case port 25 is filtered (by your ISP for example) 
	and you want to send non-encrypted emails with SMTP
	Port 465 - this is the port used if you want to send messages using SMTP securely
 
	
Reference Table:
----------------
	Prerequisite - DNS Ready, Static IP for Mail Server

Packages:
---------
	মেইল সার্ভার করতে গেলে যে সকল প্যাকেজ গুলা দরকার হবেঃ 

		=> postfix(smtp), 
		=> dovecot(pop3 & IMAP), 
		=> squirrelmail (webmail), 
		=> httpd, 
		=> telnet (testing)
		=> epel (Extra package for Enterprise Linux)

	Daemon - postfix (SMTP), dovecot (POP3 & IMAP), httpd

Configuration files:
--------------------
	=> /var/named/example.com.for  (DNS)
	=> /etc/postfix/main.cf
	=> /etc/dovecot/dovecot.conf 
	=> /etc/dovecot/conf.d/10-mail.conf 
	=> /etc/dovecot/conf.d/10-auth.conf 
	=> /etc/dovecot/conf.d/10-master.conf
	=> /usr/share/squirrelmail/config/conf.pl  - squirrelmail 
	=> /etc/httpd/conf/httpd.conf   - (web mail) 

DNS Part:
----------
	[root@nsX ~]# hostname
	[root@nsX ~]# nslookup nsX.example.com

	[root@nsX ~]# nslookup mail.example.com

	[root@nsX ~]# cd /var/named
	[root@nsX named]# ls
	[root@nsX named]# vim example.com.for 

	8	IN NS   nsX.example.com.         ; no change
	9       IN A   172.25.11.200+X           ; no change

	10      IN MX 10 mail.example.com.     ; new entry
	11      IN MX 20 mail2.example.com.    ; (optional for 2nd Mail server)

	13  nsX     IN A    172.25.11.200+X     ; no change 
	14  mail    IN CNAME nsX.example.com.   ; new entry

	15  mail2   IN A    172.25.11.Y         ;(optional for 2nd Mail server)

	[root@nsX named]# systemctl restart named.service

	Note: CNAME - Canonical Name ( If we want to configure multiple server like DNS, FTP, MAIL, 
	Web in same machine then, we can use CNAME insted of "A" record.

	[root@nsX named]# nslookup mail.example.com
	Server:		172.25.11.200+X
	Address:	172.25.11.200+X#53

	mail.example.com	canonical name = nsX.example.com.
	Name:	nsX.example.com
	Address: 172.25.11.200+X

	[root@nsx ~]# nslookup -query=mx example.com

Step 01:
--------
	[root@nsx ~]# rpm -qa | grep postfix

	[root@nsx ~]# yum install postfix* -y     ; if not found

Step 02:
--------
	[root@nsx ~]# cd /etc/postfix
	[root@ns1 postfix]# ls
	[root@nsx postfix]# vim main.cf
	:set nu

	 75 myhostname = mail.example.com
	 83 mydomain = example.com
	 99 myorigin = $mydomain
	 113 inet_interfaces = all
	 116 #inet_interfaces = localhost 
	 164 #mydestination = $myhostname, localhost.$mydomain, localhost
	 165 mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
	 250 mynetworks_style = subnet
	 264 mynetworks = 172.25.11.0/24, 127.0.0.0/8 
	 419 home_mailbox = Maildir/
	 572 smtpd_banner = $myhostname ESMTP $mail_name

	[root@ns1 postfix]# systemctl restart postfix.service
	[root@ns1 postfix]# systemctl enable postfix.service

Allow port through firewall-cmd:
-------------------------------
	[root@nsx postfix]# systemctl restart firewalld
	[root@nsx postfix]# systemctl enable firewalld
	[root@nsx postfix]# firewall-cmd --permanent --add-service=smtp 
	success
	[root@nsx postfix]# firewall-cmd --reload
	success

	[root@nsx postfix]# yum install telnet -y

Step 03: SMTP Testing
=====================
	[root@nsx postfix]# telnet mail.example.com 25
	Trying 172.25.11.200+X...
	Connected to mail.example.com.
	Escape character is '^]'.
	220 mail.example.com ESMTP Postfix
	quit
	221 2.0.0 Bye
	Connection closed by foreign host.

Step 04: dovecot install
========================
	[root@nsx ~]# yum install dovecot* -y

Step 05: dovecot configure 
========================

	[root@nsx ~]# vim /etc/dovecot/dovecot.conf 
	 24 protocols = imap pop3 lmtp
	 30 listen = *
	 42 login_greeting = Welcome to Example Inc. Mail 

	[root@nsx ~]#  vim /etc/dovecot/conf.d/10-mail.conf 
	 24    mail_location = maildir:~/Maildir

	[root@nsx ~]#  vim /etc/dovecot/conf.d/10-auth.conf 
	 10  disable_plaintext_auth = no
	 100 auth_mechanisms =  plain login

	[root@nsx ~]#  vim /etc/dovecot/conf.d/10-master.conf

	 91     user = postfix
	 92     group = postfix

	[root@nsx ~]# systemctl enable dovecot.service
	[root@nsx ~]# systemctl restart dovecot.service

Allow port through firewall-cmd:
-------------------------------
	[root@nsx ~]# firewall-cmd --permanent --add-port 110/tcp
	success
	[root@nsx ~]# firewall-cmd --reload
	success

Step 06: POP Testing
=====================
	[root@nsx ~]# telnet mail.example.com 110
	Trying 172.25.11.200+X...
	Connected to mail.example.com.
	Escape character is '^]'.
	+OK Welcome to Example Inc. Mail 
	quit
	+OK Logging out
	Connection closed by foreign host.

Mail User Create:
-------------------
	[root@nsx ~]# useradd -s /sbin/nologin sadia.afroz
	[root@nsx ~]# useradd -s /sbin/nologin rose
	[root@nsx ~]# useradd -s /sbin/nologin jack

	[root@nsx ~]# passwd jack
	[root@nsx ~]# passwd rose
	[root@nsx ~]# passwd sadia.afroz

Web Mail Configure with Squirrelmail: 
=====================================

Step 01: EPEL Install 
---------------------
	[root@serverX ~]# yum install epel-release 

	[root@nsx ~]# cd /etc/yum.repos.d
	[root@nsx yum.repos.d]# ls   

Step 02: Install Squirrelmail
------------------------------
	[root@nsx ~]# yum install squirrelmail -y

Step 03: Configure Squirrelmail
-----------------------------
	[root@ns1 ~]# cd /usr/share/squirrelmail/config/
	[root@ns1 config]# ls
	[root@ns1 config]# ./conf.pl 

	 Command >> Press 1  and Enter (Orgnization) 
	 Command >> Press 1  and Enter (Squirrelmail)
	 [SquirrelMail]: Example Ltd.  ;press Enter

	 Command >> 4    (Organizationn Ttile)

	 [SquirrelMail $version]: Training Provider

	 Command >> Press 8     ;and Press Enter 

	 [SquirrelMail]: Example Ltd.

	 Command >> S
	 Command >> R
	 Command >> Press 2
	 Command >> Press 1 (Domain)
	 [localhost]: example.com

	 Command >> Press 3
	Your choice [1/2] [1]: 2 (SMTP)

	Command >> S
	Command >> R

	Command >> Press 4 (General Options)
	Command >> 7 ( Hide SM attributions)
	Hide SM attributions (y/n) [n]: y

	Command >> S

	Command >> Q  

Step 04: Apache HTTP Install 
----------------------------
	[root@nsx ~]# yum install httpd -y

Step 05: Add following lines at the end of configuration files ####
-------------------------------------------------------------------
	[root@nsx ~]# vim /etc/httpd/conf/httpd.conf 

	 [add the following lines end of the file]

	Alias /webmail /usr/share/squirrelmail
	<Directory /usr/share/squirrelmail>
	Options Indexes FollowSymLinks

	RewriteEngine On
	AllowOverride All
	DirectoryIndex index.php
	Order allow,deny
	Allow from all
	 </Directory>

	[root@nsx ~]# systemctl restart httpd.service
	[root@nsx ~]# systemctl enable httpd.service

Allow port through firewall-cmd:
-------------------------------
	[root@nsx ~]# firewall-cmd --permanent --add-service=http 
	success
	[root@nsx ~]# firewall-cmd --permanent --add-service=dns
	success
	[root@nsx ~]# firewall-cmd --reload
	success

	[root@nsx ~]# setenforce 0

Step 06: Test
-------------
	 -> open browser
	 -> http://mail.example.com/webmail   or -> http://172.25.11.200+X/webmail 

	================= Thank you ==============


                           

