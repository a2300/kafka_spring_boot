# Getting Started with Apache Kafka from pluralsight
https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents

## ProducerApp.java
Custom producer application in Java

Cluster Setup:
 - Three partitions
 - Three brokers
 - Replication factor 3

How to do it in one virtual machine

see broker_resilience\server-x.properties

Terminal 1. Start up zookeeper
```shell
c:\kafka>bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```

Start up three brokers

Terminal 2.
```shell
c:\kafka>bin\windows\kafka-server-start.bat .\config\server-0.properties
```

Terminal 3.
```shell
c:\kafka>bin\windows\kafka-server-start.bat .\config\server-1.properties
```

Terminal 4.
```shell
c:\kafka>bin\windows\kafka-server-start.bat .\config\server-2.properties
```

Terminal 5.
Create topic with three partitions and replication factor 3
```shell
c:\kafka>bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 3 --partitions 3 --topic my_topic
WARNING: Due to limitations in metric names, topics with a period ('.') or underscore ('_') could collide. To avoid issues it is best to use either, but not both.
Created topic my_topic.
```

Describe topic
```shell
c:\kafka>.\bin\windows\kafka-topics.bat --describe --topic my_topic --zookeeper localhost:2181
Topic: my_topic TopicId: 7H79IXOGQP2sjU0vWLN3iQ PartitionCount: 3       ReplicationFactor: 3    Configs:
        Topic: my_topic Partition: 0    Leader: 2       Replicas: 2,0,1 Isr: 2,0,1
        Topic: my_topic Partition: 1    Leader: 0       Replicas: 0,1,2 Isr: 0,1,2
        Topic: my_topic Partition: 2    Leader: 1       Replicas: 1,2,0 Isr: 1,2,0
```

Terminal 6.
Subscribe to topic
```shell
c:\kafka>bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic my_topic --from-beginning
```

run ProducerApp.java
```shell
mvn compile exec:java -Dexec.mainClass="ProducerApp"
```

In terminal 6 see messages were consumed
```txt
Message: 1  sent at 2024/10/30 13:36:59:346
Message: 4  sent at 2024/10/30 13:36:59:346
Message: 7  sent at 2024/10/30 13:36:59:346
Message: 10  sent at 2024/10/30 13:36:59:347
Message: 13  sent at 2024/10/30 13:36:59:347
Message: 16  sent at 2024/10/30 13:36:59:347
Message: 19  sent at 2024/10/30 13:36:59:348
Message: 22  sent at 2024/10/30 13:36:59:348
Message: 25  sent at 2024/10/30 13:36:59:348
Message: 28  sent at 2024/10/30 13:36:59:348
Message: 31  sent at 2024/10/30 13:36:59:348
Message: 34  sent at 2024/10/30 13:36:59:349
Message: 37  sent at 2024/10/30 13:36:59:349
Message: 40  sent at 2024/10/30 13:36:59:349
Message: 43  sent at 2024/10/30 13:36:59:350
Message: 46  sent at 2024/10/30 13:36:59:350
Message: 49  sent at 2024/10/30 13:36:59:350
Message: 52  sent at 2024/10/30 13:36:59:350
Message: 55  sent at 2024/10/30 13:36:59:351
```
Notice that messages where consumed not in order due to reading from different partitions
