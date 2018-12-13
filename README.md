### Cloudera  Hadoop + Spark using ansible + vagrant (Spark is not completed, some setup tasks has to be automated : role:common:hosts to setup /etc/hosts , hadoop templates, sparc templates, etc. )

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

```
remove 127.0.0.1 hadoop* from all hosts

[root@hadoopmaster ~]# vi /etc/hosts
[root@hadoopmaster ~]# systemctl restart hadoop-yarn-resourcemanager
[root@hadoopmaster ~]# netstat -lntp|grep LIST|grep 8020
tcp        0      0 127.0.0.1:8020          0.0.0.0:*               LISTEN      25749/java          
[root@hadoopmaster ~]# systemctl restart hadoop-hdfs-namenode
[root@hadoopmaster ~]# netstat -lntp|grep LIST|grep 8020
tcp        0      0 192.168.50.11:8020      0.0.0.0:*               LISTEN      31162/java          
[root@hadoopmaster ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

## vagrant-hostmanager-start
192.168.50.12	hadoopslave1

192.168.50.11	hadoopmaster

## vagrant-hostmanager-end
```
[vagrant@hadoop-00 ~]$ sudo -Hu hdfs spark-submit --master yarn --class org.apache.spark.examples.SparkPi --num-executors 2 --driver-cores 1 --driver-memory 256m --executor-memory 256m --executor-cores 2 --queue default /usr/lib/spark/lib/spark-examples.jar 10
[vagrant@hadoop-00 ~]$ sudo -Hu hdfs spark-submit --master yarn --class org.apache.spark.examples.SparkPi --deploy-mode client /usr/lib/spark/lib/spark-examples.jar 10


```
