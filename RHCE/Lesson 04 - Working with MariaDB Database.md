Mariadb Database Server Documentation
-------------------------------------
Database Server :
------------------
	Database server is the term used to refer to the back-end system of a database application 
	using client/server architecture. The back-end, sometimes called a database server, performs 
	tasks such as data analysis, storage, data manipulation, archiving, and other non-user specific tasks.

Database Sever Example
----------------------
	Some examples of proprietary database servers are Oracle, DB2, Informix, and Microsoft SQL Server. 
	Examples of GNU General Public Licence database servers are Ingres and MySQL. Every server uses its 
	own query logic and structure. The SQL query language is more or less the same in all relational 
	database servers.

What is mariadb
----------------
	MariaDB is a community-developed fork of the MySQL relational database management system intended to 
	remain free under the GNU GPL. It is notable for being led by the original developers of MySQL, 
	who forked it due to concerns over its acquisition by Oracle.

Why use mariadb
---------------
	First and foremost, MariaDB offers more and better storage engines. NoSQL support, provided by Cassandra, 
	allows you to run SQL and NoSQL in a single database system. MariaDB also supports TokuDB, which can handle 
	big data for large organizations and corporate users.

	MySQL's usual (and slow) database engines MyISAM and InnoDB are replaced in MariaDB by Aria and XtraDB 
	respectively. Aria offers better caching, which makes a difference when it comes to disk-intensive operations.
	Temporary tables also use Aria, which speeds up complex queries, such as those involving GROUP BY and 
	DISTINCT. Percona's XtraDB gets rid of all of the InnoDB problems with slow performance and stability, 
	especially in high load environments.

	Additional, unmatched features in MariaDB provide better monitoring through the introduction of microsecond 
	precision and extended user statistics. MariaDB also enhances the KILL command to allow you to kill all 
	queries for a user (KILL USER username) or to kill a query ID (KILL QUERY ID query_id). MariaDB also 
	switched to Perl-compatible regular expressions (PCRE), which offer more powerful and precise queries 
	than standard MySQL regex support.

	In addition to more features, MariaDB has also applied a number of query optimizations for queries connected 
	with disk access, join operations, subqueries, derived tables and views, execution control, and even explain 
	statements. To see what these mean for database performance, visit the MariaDB optimizer benchmark page.

How to install mariadb all service
-----------------------------------
	[root@localhost Desktop]#yum install mariadb* -y		
		
How to start mariadb all service
---------------------------------
	[root@localhost Desktop]#systemctl start mariadb	
		
How to enable mariadb all service
----------------------------------
	[root@localhost Desktop]#systemctl enable mariadb		
		
How to restart mariadb all service
----------------------------------
	[root@localhost Desktop]systemctl restart mariadb		
		
How to login mariadb
--------------------
	[root@localhost Desktop]#mysql -u root		
		
How to create database in mariadb
----------------------------------
	MariaDB [(none)]> create database mydb;		
		
	// mydb is database name
How to show database in mariadb
--------------------------------
	MariaDB [(none)]> show databases;  		
		
How to Delete database in mariadb
----------------------------------
	MariaDB [(none)]> drop database mydb;   		
		
	// mydb is database name
How to select database in mariadb
-----------------------------------
	MariaDB [(none)]> use mydb;  		
		
	// mydb is database name
How to create table in mariadb
--------------------------------
	MariaDB [mydb]> create table student(id int auto_increment 
	primary key,name varchar(200));		
		
	// student is table name
	// id is column name and int is data type
	// auto_increment ...id column value auto increment
	// primary key for operation
	// name is column name
	// varchar(200) datatype and value length
how to add column in table in mariadb
--------------------------------------
	MariaDB [mydb]> alter table student add column roll 
	varchar(200);		
		
	//alter table is sql command
	//student table name
	//column is sql command
	// roll varchar(200) ...roll is column name and varchar datatype and length
how to delete column in table in mariadb
----------------------------------------
	MariaDB [mydb]> alter table student drop column roll;		
		
	// alter table is sql command
	//student is table name
	//drop is sql command and it use for delete column.
	//roll is column name.
How to show all tables in database in mariadb
----------------------------------------------
	MariaDB [mydb]> show tables; 		
		
how to show creating table command in mariadb
---------------------------------------------
	MariaDB [mydb]> show create table student; 		
		
	//student is table name
how to show table structure in mariadb
---------------------------------------
	MariaDB [mydb]> describe student;  		
		
	//student is table name
how to data insert in table in mariadb
----------------------------------------
	MariaDB [mydb]> insert into student(name) values('Rahman');  		
		
	//student is table name
	//student is table name
how to show all data in table in mariadb
----------------------------------------
	MariaDB [mydb]> select * from student;  		
		
	// * for all column selected
	// student is table name
how to show data in table by column in mariadb
----------------------------------------------
	MariaDB [mydb]> select name student;  		
		
	// name is column name
	// student is table name
how to show data in table by row in mariadb
--------------------------------------------
	MariaDB [mydb]> select * from student where id = 2;		
		
	//* for all column selected
	// student is table name
	// where is clause that imposes a condition on the command execution.
	// id is column name
how to update data in table in mariadb
--------------------------------------
	MariaDB [mydb]> update student set name='raju' where id=4;		
		
	// update sql command and use for data update in table.
	// student is table name.
	//name and id is column name.
how to data delete in table in mariadb
--------------------------------------
	MariaDB [mydb]> delete * from student where id=2;	
		
	//delete is sql command and it use for data delete in table;
how to make relational table in mariadb
----------------------------------------
	MariaDB [mydb]> create table country(id int auto_increment 
	primary key,name varchar(200));
	
	MariaDB [mydb]> create table city(id int auto_increment 
	primary key,name varchar(200),countryid int,
	foreign key(countryid) references country(id));	
		
	//relation country table id column between city table countryid column
how to show data from two table in mariadb
------------------------------------------
Using INNER JOIN


	MariaDB [mydb]> select city.id,city.name,country.name from city inner
	join country on city.id = country.id;	
		
INNER JOIN: Returns all rows when there is at least one match in BOTH tables

LEFT JOIN


	MariaDB [mydb]> select city.id,city.name,country.name from city left
	join country on city.id = country.id;	
		
LEFT JOIN: Return all rows from the left table, and the matched rows from the right table

Using RIGHT JOIN


	MariaDB [mydb]> select city.id,city.name,country.name from city right
	join country on city.id = country.id;
		
RIGHT JOIN: Return all rows from the right table, and the matched rows from the left table

how to delete table in mariadb
-------------------------------
	MariaDB [mydb]> drop table student;	
		
	//drop is sql command and it use for delete table.
creating user account in mariadb
--------------------------------
	MariaDB [(none)]> create user mobius@localhost identified by 
    	'redhat';	
		
	// create user sql command for user create
	// mobius is username.
	// @localhost- user mobius can connect just form localhost
Rename User account in mariadb
------------------------------
	MariaDB [(none)]>RENAME USER 'mobius@localhost' TO 
   	'duck'@'localhost';
			
		
How to delete user in mariadb
-----------------------------
	MariaDB [(none)]>drop user mobius@localhost;	
		
	Grant Insert,Update,Delete,and select privileges to user mobius
		MariaDB [(none)]> grant insert,update,delete,select on 
		mydb.* to mobius@localhost;	
		
	// grant command use for user permission
Flush the privileges
--------------------
	MariaDB [(none)]> flush privileges;	
		
how to show grants in mariadb
------------------------------
	MariaDB [(none)]>show grants for mobius@localhost;	
		
how to remove grants in mariadb
-------------------------------	
	MariaDB [(none)]> revoke select,insert,update,delect on 
	mydb.student from mobius@localhost;	
		
how to login users in mariad
------------------------------
	[root@localhost Desktop]# mysql -u mobius -p;	
		
how to backup a database in mariadb
-----------------------------------
	[root@localhost Desktop]# mysqldump -u root -p mydb > 
	backup/mydb.dump;	
		
how to backup all database in mariadb
--------------------------------------	
	[root@localhost Desktop]# mysqldump -u root -p --all-
	databases > backup/mariadb.dump;	
		
restoring a database form backup in mariadb
-------------------------------------------	
	[root@localhost Desktop]# mysql -u root databasename < 
	backup/mydb.dump;	
		
restoring databases form backup
-------------------------------	
	[root@localhost Desktop]# mysql -u root < 
	backup/mariadb.dump;	
		
how to making Serial number in mariadb
----------------------------------------	
	MariaDB [mydb]>select name,@id:=@id+1 serial_number
	from (SELECT @id:= 0) AS id,country
	order by name asc	
		
how to start apache Server in linux
-----------------------------------		
	[root@localhost Desktop]#sudo service httpd start	
		
	Apache,phpMyAdmin, Mariadb Configuration
	your All service Download from Internet
		
	[root@localhost Desktop]# yum install epel-release	
		
Install your important Service
-------------------------------		
	[root@localhost Desktop]# yum install httpd -y
	[root@localhost Desktop]# yum install php php-mysql
	[root@localhost Desktop]# yum install phpmyadmin -y
	[root@localhost Desktop]# yum install mariadb* -y
		
Add port your firewall
----------------------		
	[root@localhost Desktop]# systemctl status firewalld
	[root@localhost Desktop]# systemctl restart firewalld	
	[root@localhost Desktop]# systemctl enable firewalld	
	[root@localhost Desktop]# firewall-cmd --permanent --add-port =80/tcp	
	[root@localhost Desktop]# firewall-cmd --reload	
	[root@localhost Desktop]# firewall-cmd --permanent --list-all	
		
	//check firewall status //check permanent running service
Run all service
---------------		
	[root@localhost Desktop]# systemctl restart httpd
	[root@localhost Desktop]# systemctl restart mariadb
		
	if you need change phpmyadmin config.d
		
	etc/httpd/config.d/phpmyadmin.conf	
		
	Configuring MariaDB for Remote Client Access
	Finding the Defaults File
	To enable MariaDB to listen to remote connections, you need to edit your defaults file. 
	See Configuring MariaDB with my.cnf for more detail.

		
	  * /etc/my.cnf                      (*nix/BSD)
	  * $MYSQL_HOME/my.cnf               (*nix/BSD) *Most Notably /etc/mysql/my.cnf
	  * SYSCONFDIR/my.cnf                (*nix/BSD)
	  * DATADIR\my.ini                   (Windows)
		
Editing the Defaults File
--------------------------
	Once you have located the defaults file, use a text editor to open the file and try to find 
	lines like this under the [mysqld] section:

		
	 [mysqld]
    ...
    skip-networking
    ...
    bind-address = 
    ...	
		
	(The lines may not be in this order, and the order doesn't matter.) If you are able to locate 
	these lines, make sure they are both commented out (prefaced with hash (#) characters), so that 
	they look like this:

    
    [mysqld]
    ...
    #skip-networking
    ...
    #bind-address = 
    ...
	(Again, the order of these lines don't matter) Save the file and restart the mysqld daemon or 
	service (see Starting and Stopping MariaDB).

Granting User Connections From Remote Hosts
--------------------------------------------		
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.100.%' IDENTIFIED BY 'my-new-password' WITH GRANT OPTION;	
		
	(% is a wildcard) For more information about how to use GRANT, please see the GRANT page. At this point 
	we have accomplished our goal and we have a user 'root' that can connect from anywhere on 
	the 192.168.100.0/24 LAN.

Port 3306 is Configured in Firewall
-------------------------------------
	One more point to consider whether the firwall is configured to allow incoming request from remote clients:

	On RHEL and CentOS 7, it may be necessary to configure the firewall to allow TCP access to MySQL from remote 
	hosts. To do so, execute both of these commands:

		

	[root@localhost Desktop]# firewall-cmd --add-port=3306/tcp 
	[root@localhost Desktop]# firewall-cmd --permanent --add-port=3306/tcp
		
Login Remotely
----------------		
	[root@localhost Desktop]# mysql -u mahabub -h 192.168.0.134 -p	
		
your All service Download from Internet
----------------------------------------		
	[root@localhost Desktop]# yum install epel-release	
		
