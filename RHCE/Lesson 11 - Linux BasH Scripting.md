What is Kernel?
---------------
Interface Between Hardware and Software

	  User
	   ↓
	Application 
	   ↓
	 Shell 
	   ↓
	 Kernel (Program)
	   ↓
	Hardware (CPU, RAM, HDD)

Application: Firefox, office, games
Operating Sytem: Shell + Kernel
Software: Application + Shell

What is Shell:
--------------
 > Its like a container
 > Interfaces between users and Kernel/OS
 > input from keyboard via users
 > out put to monitor from kernel
 > CLI is a Shell
 > Windows GUI is a Shell
 > Linux KDE/GNOME is a Shell
 > Linux sh, bash etc. is a shell
 
 Types of Linux Shell:
----------------------
 1. shell sh(.sh)
 2. bash shell (commonly used)
 3. zshell 
 4. cshell (c programming based)
 5. kshell
 6. GNOME (GUI)
 7. KDE (GUI)

Find your Shell:
---------------
[root@serverX ~]# cat /etc/shells
[root@serverX ~]# echo $SHELL
[root@serverX ~]# echo $0

Current installed shell in system:
----------------------------------
[root@serverX ~]#  cat /etc/shells 
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin

What is Shell Scripts?
---------------------
Shell Script হলো একটি সাধারণ Plain Text File যার মধ্যে কিছু কমান্ড Sequentially সংরক্ষণ করা হয়। যখন Script টি execute করা হয়, তখন script এ সংরক্ষিত কমান্ডগুলো serially execute করে এবং নিরধারিত কাজ সম্পন্ন হয়। এক কথায় আমরা ম্যানুয়ালি যত কমান্ড রান করি, সেই কমান্ড গুলা কোন ফাইলের মধ্যে রেখে অটোমেটিক ভাবে স্ক্রিপ্টের মাধ্যমে রান করতে পারি। উল্লখ্য যে, আমরা চাইলে  স্ক্রিপ্টের মধ্যে বিভিন্ন কন্ডিশনাল অপারেটর, ফাংশন এবং ভ্যরিয়েবল কল করতে পারি।

উদাহরন হিসবে, আমরা লিনক্সে একটা ইউজার তৈরি এবং পাসওয়ার্ড দিতে চাই, এই জন্য আমাদের অনেক গুলা কমান্ড চালাতে হবে। আবার উক্ত ইউজার আগে থেকেই আছে কিনা সেটাও জানা দরাকার।  তাইলে এই কাজ টি আমরা স্ক্রিপ্টের মাদ্ধ্যেমে করতে চাইলে আমরা প্রথমে ইউজার আছে কিনা চেক করি তারপর, ইউজার তৈরি এবং এবং ইউজারের পাসওয়ার্ড দিই। এখন এই কমান্ড গুলা যদি কোন ফাইলের মধ্যে রেখে সেটাকে স্ক্রিপ্ট হিসেবে রান করলে আমার একই কাজ হবে। 

Shell Extension:
---------------
> test
> test.sh
> test.bash

How to write a script:
======================
[root@server ~]# mkdir /scripting 
[root@server ~]# cd /scripting
[root@server scripting]# ls
[root@server scripting]# vim first.sh
 #!/bin/bash
 # date: 20-10-2017
 # this is shell comments
 # this is a test script

echo "This is our First Script"

date 

ping -c 4 8.8.8.8
ping 4.2.2.2

:x

Setting shell permission:
=========================
[root@server scripting]#  ll
-rw-r--r--. 1 root root 90 Jul 27 10:46 first.sh

[root@server scripting]# chmod +x first.sh

[root@server scripting]# ll
-rwxr-xr-x. 1 root root 90 Jul 27 10:46 first.sh

Script Execute/Run:
==================
[root@server scripting]# ./first.sh

or 

[root@server scripting]# sh first.sh

This is our first Script

Shell Variable Types:
====================
  => System Defined Variable (Block Letter) 
  => User Defined Variabel (Small and Block)

[root@server scripting]# echo $LOGNAME
[root@server scripting]# echo $PWD
[root@server scripting]# echo $LINES
[root@server scripting]# echo $HOSTNAME
[root@server scripting]# echo $OSTYPE

[root@server scripting]# printenv

User Defined Variable:
----------------------
[root@server scripting]# a=10
[root@server scripting]# echo $a
[root@server scripting]# a =10
bash: a: command not found...

[root@server scripting]# a= 20
bash: 20: command not found...

[root@server scripting]# a = 20
bash: a: command not found...

[root@server scripting]# a=10
[root@server scripting]# A=20
[root@server scripting]# echo $a
10
[root@server scripting]# echo $A
20

Read From Terminal:
------------------
[root@server scripting]# read a
10
[root@server scripting]# echo $a
10

Shell Arithmetic:
=================
[root@server scripting]# expr 1 + 2
3

[root@server scripting]# expr 1+2
1+2

[root@server scripting]# expr 2 - 1
1

[root@server scripting]# expr 2 * 9
expr: syntax error

[root@server scripting]# expr 2 \* 9
18 

[root@server scripting]# expr 4 / 2
2

[root@server scripting]# expr 15 % 2   ;mod operatoin 
1

[root@server scripting]# echo expr 1 + 2
expr 1 + 2

[root@server scripting]# echo `expr 1 + 2`

Example: 02
-----------
[root@server scripting]# vim second.sh
 #!/bin/bash
 # this is arithmetic expression test script
 
 a=10
 b=25
 echo `expr $a + $b`
 echo `expr $b - $a`
 echo `expr $b / $a`
 echo `expr $b % $a`
 echo `expr $b \* $a`

:x
[root@server ~]# sh second.sh

Example 03: Command Line Argument:
----------------------------------
[root@server scripting]# vim sum.sh
#!/bin/bash
# this script for command line argument

 echo "The sum is `expr $1 + $2`"

:x

[root@server scripting]# sh sum.sh 10 20
 The sum is 30

Example 04: Value from Terminal/user 
------------------------------------
[root@server scripting]# vim third.sh
#!/bin/bash
# this script testing for user input from terminal

echo -n "Please Enter your name:"
read name
echo "Hello $name !!! "
sleep 2
echo "welcome to Linux Class"
sleep 2
echo -n "Give me value-1: "
read a
echo -n "Give me value-2: "
read b
echo "your output is: `expr $a + $b`"

:x
[root@server scripting]# sh third.sh

Shell built in variable: $0 - $9
 file.sh will store in $0

Working with Exit status (error, true/false:
===========================================
[root@server scripting]# date
Sat Jul 30 09:37:33 BDT 2016

[root@server scripting]# echo $?
 0
[root@server scripting]# abcd
bash: abcd: command not found...
[root@server scripting]# echo $?
 127

Note: The exit status is a numeric value in the range of 0 to 255.
A "0" indicates success; any other value indicates failure.

Working with 'test' statement: (Int. value)
==========================================
[root@server scripting]# test 5 -eq 5
[root@server scripting]# echo $?
0
[root@server scripting]# test 5 -eq 9
[root@server scripting]# echo $?
1
[root@server scripting]# test 5 -gt 9
[root@server scripting]# echo $?
1
[root@server scripting]# test 15 -gt 9
[root@server scripting]# echo $?
0
[root@server scripting]# test 5 -le 9
[root@server scripting]# echo $?
0
[root@server scripting]# test 5 -lt 9
[root@server scripting]# echo $?
0
[root@server scripting]# test 5 -ne 9
[root@server scripting]# echo $?
0
[root@server scripting]# test 5 -ne 5 
[root@server scripting]# echo $?
1

Working with 'test' statement: (string)
======================================
[root@server scripting]# test user_root = user_root
[root@server scripting]# echo $?
0
[root@server scripting]# test user_root = user_student
[root@server scripting]# echo $?
1

[root@samba scripting]# test user_root != user_student
[root@samba scripting]# echo $?
0

[root@samba scripting]# test user_root != user_root
[root@samba scripting]# echo $?
1

Using Conditional Statement:
============================
 if
 exit
 for
 while
 until
 case
 break
 continue

Working with 'if':
------------------
 # First form

 if condition ; then
    commands
 fi

 # Second form

 if condition ; then
    commands
 else
    commands
 fi

 # Third form

 if condition ; then
    commands
 elif condition ; then
    commands
 fi 

Example 05: Working with 'if' statement
--------------------------------------
[root@server scripting]# vim if-then.sh
 #!/bin/bash
 # this script for if then else statement check

 echo -n "Enter the value: "
 read num
  if test $num -ge 0
 then
  echo "The number is positive"
 else
  echo "The number is negative"
 fi

N.B.: here 'test=0/true'. used to perform true/false decisions with 'if'

[root@server scripting]# sh if-then.sh
 Enter the value: 10
 The number is positive

[root@server scripting]# sh if-thenelse.sh
 Enter the value: -10
 The number is negative

Example 06: Use of Expression []:
---------------------------------
[root@server scripting]# vim if-then.sh
#!/bin/bash
# this script for if then else statement check

echo -n "Enter the value: "
read num
if [ $num -ge 0 ]
then
echo "The number is positive"
else
echo "The number is negative"
fi

Example 07: Working with String:
-------------------------------
[root@server scripting]# vim if-else-string.sh
#!/bin/bash
# this script for string comparison

echo -n "Enter the logged in username: "
read user
if test $user = $LOGNAME
then
echo "Hello $LOGNAME, welcome to Linux Systems"
else
echo "Different User Logged in"
fi

[root@server scripting]# sh if-else-string.sh


Example 08: System Information using a shell script:
----------------------------------------------------
[root@server ~]# vim system.sh

#!/bin/bash
#this scripts devloped to get system information
 clear
 echo "######## $HOSTNAME hardware information ######"
  sleep 2
echo "                                      "
echo "**************************************"
echo "**** System Kernel Information ****" 
      uname -r
echo "**************************************"
 sleep 2

echo "**** System Memory Info ****" 
dmidecode --type memory | grep "Installed" | head -1 | cut -d " " -f3-4
echo "**************************************"
 sleep 2

 echo "**** disk Information ****"
fdisk -l | grep GB | cut -d " " -f 1-4
echo "**************************************"
 sleep 2

 echo "**** CPU Information ****"
cat /proc/cpuinfo | grep "model name"
echo "**************************************"
 sleep 2

  echo "**** System uptime information ****"
  uptime
 
[root@server ~]# chmod u+x system.sh
[root@server ~]# sh system.sh
 
yum client configure script:
----------------------------
[root@server ~]# vim yumclient.sh

#!/bin/bash
#this script for yum client configuraiotn
echo "welcome to yum client configuration"
echo "Enter your yum server IP address: "
read server
echo "you type your server ip address is:" $server
echo "pinging your server:" 
echo "`ping -c 2 $server`"
cd /etc/yum.repos.d/
`rm -rf * `
`touch client.repo`
echo [client] > /etc/yum.repos.d/client.repo
echo name=my yum client >> /etc/yum.repos.d/client.repo
echo baseurl=ftp://$server/pub/Packages >> /etc/yum.repos.d/client.repo
echo gpgcheck=0 >> /etc/yum.repos.d/client.repo
echo enabled=1 >> /etc/yum.repos.d/client.repo
echo "your yum client is ready"
sleep 1
echo "thank you"

[root@server ~]# chmod u+x yumclient.sh
[root@server ~]# sh yumclient.sh
[root@server ~]# 

Class Work:
-----------
1. create a username and password with shell scripting

#!/bin/bash
#Find user name if not found create new user

echo -n "Enter the user name:"
read user
if [ `grep $user /etc/passwd` ]
then
        echo "user exists"
        else
             useradd $user
    sleep 2
   
echo "123456" | passwd --stdin $user

 echo "new user added"

fi

===================  x ==================





