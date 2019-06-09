# Install CDH6 on AWS.
## Follow Udemy

# Cloudera_6_Centos_7


## Instance EC2

connect key in path : `/root/CDH6Test.pem`

connect command line : `ssh -i "CDH6Test.pem" centos@ec2-54-251-138-225.ap-southeast-1.compute.amazonaws.com`

```
root@piboonsak-asus:~# ssh -i "CDH6Test.pem" centos@ec2-54-251-138-225.ap-southeast-1.compute.amazonaws.com
[centos@ip-172-31-16-140 ~]$ pwd
/home/centos

[centos@ip-172-31-16-140 ~]$ cd
[centos@ip-172-31-16-140 ~]$ sudo su
[root@ip-172-31-16-140 centos]# cd
```

Change passwd : `password`

```
[root@ip-172-31-16-140 ~]# passwd
Changing password for user root.

```

```
[root@ip-172-31-16-140 ~]# yum -y update
Loaded plugins: fastestmirror

```

##Run cli below for disable selinux

`sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux` 
`sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config`


echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled" >> /etc/rc.local 
echo "echo never > /sys/kernel/mm/transparent_hugepage/defrag" >> /etc/rc.local 

```
[root@ip-172-31-16-140 ~]# sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux
[root@ip-172-31-16-140 ~]# sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
[root@ip-172-31-16-140 ~]# echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled" >> /etc/rc.local
[root@ip-172-31-16-140 ~]# echo "echo never > /sys/kernel/mm/transparent_hugepage/defrag" >> /etc/rc.local
[root@ip-172-31-16-140 ~]# cat /etc/rc.local
#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.

touch /var/lock/subsys/local
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

### Run cli below

`echo never >/sys/kernel/mm/transparent_hugepage/enabled`
`echo never > /sys/kernel/mm/transparent_hugepage/defrag`

```
[root@ip-172-31-16-140 ~]# echo never >/sys/kernel/mm/transparent_hugepage/enabled
[root@ip-172-31-16-140 ~]# echo never > /sys/kernel/mm/transparent_hugepage/defrag
[root@ip-172-31-16-140 ~]# cat /sys/kernel/mm/transparent_hugepage/defrag 
always madvise [never]

```
---

`sudo vi /etc/systemd/system/disable-thp.service`

```
[Unit]
Description=Disable Transparent Huge Pages (THP)

[Service]
Type=simple
ExecStart=/bin/sh -c "echo 'never' > /sys/kernel/mm/transparent_hugepage/enabled && echo 'never' > /sys/kernel/mm/transparent_hugepage/defrag"

[Install]
WantedBy=multi-user.target
```
___

`systemctl daemon-reload`

`systemctl start disable-thp`
`systemctl enable disable-thp`

`systemctl status disable-thp`

```
[root@ip-172-31-16-140 ~]# systemctl daemon-reload
[root@ip-172-31-16-140 ~]# systemctl start disable-thp
[root@ip-172-31-16-140 ~]# systemctl enable disable-thp
Created symlink from /etc/systemd/system/multi-user.target.wants/disable-thp.service to /etc/systemd/system/disable-thp.service.
[root@ip-172-31-16-140 ~]# systemctl status disable-thp
● disable-thp.service - Disable Transparent Huge Pages (THP)
   Loaded: loaded (/etc/systemd/system/disable-thp.service; enabled; vendor preset: disabled)
   Active: inactive (dead)

Jun 09 06:32:09 ip-172-31-16-140.ap-southeast-1.compute.internal systemd[1]: Started Disable ...
Hint: Some lines were ellipsized, use -l to show in full.

```

`cat /proc/sys/vm/swappiness`

`echo "vm.swappiness = 1" >> /etc/sysctl.conf`

```
[root@ip-172-31-16-140 ~]# echo "vm.swappiness = 1" >> /etc/sysctl.conf
```


/etc/sysctl.conf add line vm.swappiness=1

 

 cat /proc/sys/vm/swappiness
 
___
 
 
  `df -h`
```
[root@ip-172-31-16-140 ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       40G  1.4G   39G   4% /
devtmpfs        3.8G     0  3.8G   0% /dev
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           3.9G   17M  3.9G   1% /run
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
tmpfs           782M     0  782M   0% /run/user/1000

```
  sudo resize2fs /dev/xvde
  
  sudo resize2fs /dev/xvda1

`yum -y update`

`yum -y install ntp`

```
[root@ip-172-31-16-140 ~]# yum -y install ntp
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.vastspace.net
 * extras: mirror.vastspace.net
 * updates: mirror.vastspace.net
Resolving Dependencies

```

`yum -y install wget yum-utils unzip`

```
[root@ip-172-31-16-140 ~]# yum -y install wget yum-utils unzip
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.vastspace.net
```

yum -y --enablerepo=extras install epel-release
yum -y install python-pip
pip install psycopg2==2.7.5 --ignore-installed

```
[root@ip-172-31-16-140 ~]# pip install psycopg2==2.7.5 --ignore-installed
Collecting psycopg2==2.7.5
  Downloading https://files.pythonhosted.org/packages/7c/e6/d5161798a5e8900f24216cb730f2c2be5e4758a80d35c8588306831c0c99/psycopg2-2.7.5-cp27-cp27mu-manylinux1_x86_64.whl (2.7MB)
    100% |████████████████████████████████| 2.7MB 435kB/s 
Installing collected packages: psycopg2
Successfully installed psycopg2-2.7.5
You are using pip version 8.1.2, however version 19.1.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
```
___

# Reboot ec2

___
## Check config

### Selinux

`cat /etc/sysconfig/selinux`
```
[root@ip-172-31-16-140 ~]# cat /etc/sysconfig/selinux

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted

```

`cat /sys/kernel/mm/transparent_hugepage/defrag`

```
[root@ip-172-31-16-140 ~]# cat /sys/kernel/mm/transparent_hugepage/defrag
always madvise [never]
```

`cat /proc/sys/vm/swappiness`

```
[root@ip-172-31-16-140 ~]# cat /proc/sys/vm/swappiness
1

```

Make AMI >> Completed!

# CDH Install

connect to instance :
ssh -i "CDH6Test.pem" centos@ec2-18-139-73-199.ap-southeast-1.compute.amazonaws.com

sudo su
cd


wget https://archive.cloudera.com/cm6/6.0.0/cloudera-manager-installer.bin

wget https://archive.cloudera.com/cm6/6.0.1/cloudera-manager-installer.bin

wget https://archive.cloudera.com/cm6/6.2.0/cloudera-manager-installer.bin

chmod u+x cloudera-manager-installer.bin

./cloudera-manager-installer.bin

to uninstall
sudo /usr/share/cmf/uninstall-cloudera-manager.sh

log file
/var/log/cloudera-manager-installer/

sudo tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log


set hostname

nm-node     ip-172-31-16-140.ap-southeast-1.compute.internal
data-node1  ip-172-31-27-33.ap-southeast-1.compute.internal
data-node2  ip-172-31-28-245.ap-southeast-1.compute.internal
data-node3  ip-172-31-24-21.ap-southeast-1.compute.internal


## postgres not start

Check service status 

systemctl list-units|grep postgresql

systemctl status postgresql.service
