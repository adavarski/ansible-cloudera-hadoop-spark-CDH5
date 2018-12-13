### Cloudera  Hadoop + Spark using ansible + vagrant (Spark is not completed)

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

[vagrant@hadoop-00 ~]$ sudo -Hu hdfs spark-submit --master yarn --class org.apache.spark.examples.SparkPi --num-executors 2 --driver-cores 1 --driver-memory 256m --executor-memory 256m --executor-cores 2 --queue default /usr/lib/spark/lib/spark-examples.jar 10
[vagrant@hadoop-00 ~]$ sudo -Hu hdfs spark-submit --master yarn --class org.apache.spark.examples.SparkPi --deploy-mode client /usr/lib/spark/lib/spark-examples.jar 10


```
