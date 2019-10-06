# PrereQuisites-BASH
REHL only
yum Wget install 
======================================================================================================================================

#!/bin/bash


sudo yum -y install ntp
sudo chkconfig ntpd on
sudo ntpd -q
sudo service ntpd start
sudo mv /etc/localtime /etc/localtime.back
sudo ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime 

sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
sudo setenforce 0
sudo yum install firewalld
sudo systemctl stop firewalld.service
sudo systemctl disable firewalld.service

sudo systemctl start tuned
sudo tuned-adm off
sudo systemctl stop tuned
sudo systemctl disable tuned


echo 0 | sudo tee /proc/sys/vm/swappiness
echo vm.swappiness = 0 | sudo tee -a /etc/sysctl.conf


sed -i '/exit 0/d' /etc/rc.local
cat >>/etc/rc.local <<EOL
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
echo never > /sys/kernel/mm/transparent_hugepage/defrag 
fi
exit 0
EOL

