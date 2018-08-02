# Zabbix template for EMC VNX Monitoring&Reporting 
 
 Use this template to monitor EMC VNX Monitoring&reporting aka VNX M&R vApp/linux instance via passive checks. This readme contains step-by-step instructions for zabbix agent insrtallation and template configuration.
# Prerequisities
  - Install and configure Zabbix-agent inside vApp
  - Tweak firewall settings to allow zabbix passive checks
 
#### Zabbix-agent installation
  - login to your VNX M&R instance via ssh - use root/Changeme1! credentials and run:
```sh
zypper addrepo http://download.opensuse.org/repositories/server:/monitoring/SLE_11_SP3/ server_monitoring
zypper update
zypper install zabbix-agent
```
  - configure Zabbix-agent according your needs (server ip for passive checks)
  - enable Zabbix-agent service
```sh
chkconfig zabbix-agentd on
```
  - check if your changes were applied
```sh
chkconfig zabbix-agentd -l
```
should print smth like:
```sh
zabbix-agentd             0:off  1:off  2:off  3:on   4:off  5:on   6:off
```
#### Tweak VNX M&R firewall settings
  - edit file:
```sh
/etc/sysconfig/scripts/SuSEfirewall2-custom
```
add following line into fw_custom_before_port_handling section before "true":
```sh
iptables -I INPUT 2 -p tcp -s 217.73.28.46 --dport 10050 -m comment --comment "zabbix server" -j ACCEPT	
```
   - apply changes by running
```sh
/sbin/SuSEfirewall2
```
# Template installation and Zabbix configuration
Now, the easy part :)
  - Import template VNX Monitoring & Reporting from this repo into Zabbix
  - Add VNX Monitoring & Reporting vApp as New host into Zabbix
  - Assign newly imported template to your new host

You can also:
  - Assign Template OS Linux to your new host
  
