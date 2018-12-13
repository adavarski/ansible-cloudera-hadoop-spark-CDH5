# Cloudera  Hadoop using ansible + vagrant

Launch Cloudera Hadoop 
(Latest release: CDH 5.16.1 Release Date: November 28, 2018 Status: Production)

Edit Vagrantfile regarding your setup

$ vagrant up

Create inventory hosts file: 
     
     [launched]
     <All IP ADDRESSES>
     
     [master]
     <Master IP ADDRESSES>
     
     [slaves]
     <Slaves IP ADDRESSES>
     <Slaves IP ADDRESSES>
     <Slaves IP ADDRESSES>
     
Then Run
      
$ ansible-playbook  -i hosts site.yaml 

### Config yarn  | resourcemanager:   | http://192.168.102.100:8088  |

```
[root@hadoop-00 conf]# yum install net-tools -y
[root@hadoop-00 conf]# diff yarn-site.xml yarn-site.xml.ORIG
6,9d5
<   <property> 
<     <name>yarn.resourcemanager.webapp.address</name>
<     <value>0.0.0.0:8088</value>
<   </property>
[root@hadoop-00 conf]# systemctl restart hadoop-yarn-resourcemanager
[root@hadoop-00 conf]# netstat -nlpt|grep 8088
tcp6       0      0 :::8088                 :::*                    LISTEN      16599/java          
```
