# Cloudera  Hadoop + Spark using ansible + vagrant (Spark is not completed, some setup tasks has to be automated)

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

### Config RM  | resourcemanager:   | http://192.168.102.100:8088  |

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
### Setup hostnames hadoop-00 and hadoop-01 and restart services
```
[root@hadoop-01 hadoop-hdfs]# tail -f hadoop-hdfs-datanode-hadoop-01.out
2018-12-13 01:43:50,931 WARN  [Thread-16] datanode.DataNode (BPServiceActor.java:retrieveNamespaceInfo(174)) - Problem connecting to server: hadoop-00:8020

[root@hadoop-00 ~]# netstat -antpl|grep 8020
tcp        0      0 127.0.0.1:8020          0.0.0.0:*               LISTEN      1268/java  

# cat /etc/hosts
#127.0.0.1	hadoop-00	hadoop-00
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.102.100 hadoop-00
192.168.102.101 hadoop-01

[root@hadoop-00 conf]# systemctl restart hadoop-hdfs-namenode
[root@hadoop-00 conf]# systemctl restart hadoop-yarn-resourcemanager
[root@hadoop-00 conf]# netstat -antpl|grep 8020
tcp        0      0 192.168.102.100:8020    0.0.0.0:*               LISTEN      3134/java  

[root@hadoop-01 hadoop-hdfs]# systemctl restart hadoop-hdfs-datanode

[vagrant@hadoop-00 ~]$ sudo -Hu hdfs spark-submit --master yarn --class org.apache.spark.examples.SparkPi --num-executors 2 --driver-cores 1 --driver-memory 256m --executor-memory 256m --executor-cores 2 --queue default /usr/lib/spark/lib/spark-examples.jar 10
[vagrant@hadoop-00 ~]$ sudo -Hu hdfs spark-submit --master yarn --class org.apache.spark.examples.SparkPi --deploy-mode client /usr/lib/spark/lib/spark-examples.jar 10


```
