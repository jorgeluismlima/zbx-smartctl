#Description
This is the template for Zabbix providing SMART monitoring for HDD using smartctl utility.  
This branch has the template for Zabbix 2.08. 
LLD is used for disks discovery.  Only devices with SMART enabled will be discovered.


##MAC OSX:
zabbix_agentd_osx_installer
https://github.com/mipmip/zabbix_agentd_osx_installer/releases
```
smartmontools_osx_installer  
http://builds.smartmontools.org/ 
```
  
- Make sure that smartmontools utils are installed:
- install the script smartctl-disks-discovery.pl in /usr/local/bin/
- test the script by running it. You should receive JSON object in the script output
```

Add the following lines in zabbix_agentd.conf file:  
```
##########SMART############
UserParameter=uHDD[*],/usr/local/sbin/smartctl -A /dev/$1 | grep $2 | awk '{print $$10}' | tail -1
UserParameter=uHDD.I[*],/usr/local/sbin/smartctl -i /dev/$1 |grep "$2" | cut -f2 -d: |tr -d " "
UserParameter=uHDD.H[*],/usr/local/sbin/smartctl -H /dev/$1 |grep "test"| cut -f2 -d: |tr -d " "
UserParameter=uHDD.L[*],/usr/local/sbin/smartctl -l error /dev/$1 |grep "ATA Error Count"| cut -f2 -d: |tr -d " "
UserParameter=uHDD.discovery,/usr/local/bin/smartctl-disks-discovery.pl

