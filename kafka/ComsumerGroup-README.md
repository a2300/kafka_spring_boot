# Getting Started with Apache Kafka from pluralsight
https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents

## ComsumerApp.java
Custom consumer application in Java

Cluster setup:
- Single broker
- One topics
- Three partitions per topic
- Single replication factor
- Three consumers

How to do it in one virtual machine
Terminal 1
```shell
c:\kafka>bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```

Terminal 2
```shell
c:\kafka>bin\windows\kafka-server-start.bat .\config\server-0.properties
```

Terminal 3
```shell

c:\kafka>bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic my-topic
Created topic my-topic.

c:\kafka>.\bin\windows\kafka-topics.bat --describe  --zookeeper localhost:2181
Topic: my-topic TopicId: Izw1ba15SD2qcl8Sh7R1FQ PartitionCount: 3       ReplicationFactor: 1    Configs:
        Topic: my-topic Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: my-topic Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: my-topic Partition: 2    Leader: 0       Replicas: 0     Isr: 0
c:\kafka>
```


Run ConsumerGroupAppXXX in separate terminals
```shell
mvn compile exec:java -Dexec.mainClass="ConsumerGroupApp001"
```
```shell
mvn compile exec:java -Dexec.mainClass="ConsumerGroupApp002"
```
```shell
mvn compile exec:java -Dexec.mainClass="ConsumerGroupApp003"
```

Run ProducerGroupApp
```shell
mvn compile exec:java -Dexec.mainClass="ProducerGroupApp"
```

and see each ConsumerGroupAppXXX assigned to different partitions for my-topic
ConsumerGroupApp001
```text
I am subscribed to the following topics:
my-topic
Topic: my-topic, Partition: 2, Offset: 111, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 112, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 113, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 114, Key: null, Value: ABCDEFG
```

ConsumerGroupApp001
```text
I am subscribed to the following topics:
my-topic
Topic: my-topic, Partition: 0, Offset: 80, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 81, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 82, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 83, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 84, Key: null, Value: ABCDEFG
```

ConsumerGroupApp003
```text
I am subscribed to the following topics:
my-topic
Topic: my-topic, Partition: 2, Offset: 111, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 112, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 113, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 114, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 115, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 116, Key: null, Value: ABCDEFG
```

Re-balancing
As of now,we are having 3 consumers and 3 partitions in the topic each consumer assigned to different partitions.
Kill ConsumerGroupApp003 and wait for some time

Now ConsumerGroupApp002 reads messages from partition 0 and 1
```text
Topic: my-topic, Partition: 0, Offset: 113, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 114, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 115, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 0, Offset: 116, Key: null, Value: ABCDEFG
```

ConsumerGroupApp001 reads messages from partition 2
```text
Topic: my-topic, Partition: 2, Offset: 144, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 145, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 146, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 147, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 148, Key: null, Value: ABCDEFG
Topic: my-topic, Partition: 2, Offset: 149, Key: null, Value: ABCDEFG
```


