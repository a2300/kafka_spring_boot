# Getting Started with Apache Kafka from pluralsight
https://app.pluralsight.com/library/courses/apache-kafka-getting-started/table-of-contents

## ComsumerApp.java
Custom consumer application in Java

Cluster setup:
- Single broker
- Two topics
- Three partitions per topic
- Single replication factor

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
c:\kafka>bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic my-topic-other
Created topic my-topic-other.

c:\kafka>bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic my-topic
Created topic my-topic.

c:\kafka>.\bin\windows\kafka-topics.bat --describe  --zookeeper localhost:2181
Topic: my-topic TopicId: Izw1ba15SD2qcl8Sh7R1FQ PartitionCount: 3       ReplicationFactor: 1    Configs:
        Topic: my-topic Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: my-topic Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: my-topic Partition: 2    Leader: 0       Replicas: 0     Isr: 0
Topic: my-topic-other   TopicId: XYi8h94lS6ii0Us2ZspTFw PartitionCount: 3       ReplicationFactor: 1    Configs:
        Topic: my-topic-other   Partition: 0    Leader: 0       Replicas: 0     Isr: 0
        Topic: my-topic-other   Partition: 1    Leader: 0       Replicas: 0     Isr: 0
        Topic: my-topic-other   Partition: 2    Leader: 0       Replicas: 0     Isr: 0

c:\kafka>
```

```shell
c:\kafka>bin\windows\kafka-producer-perf-test.bat --topic my-topic --num-records 10 --record-size 1 --throughput 10 --producer-props bootstrap.servers=localhost:9092 key.deserializer=org.apache.kafka.common.serialization.StringDeserializer value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

Run ConsumerAssignApp
```shell
mvn compile exec:java -Dexec.mainClass="ConsumerAssignApp"
```
and see that messages are consume only my-topic partition 0
```text
I am assigned to following partitions:
Partition: 2 in Topic: my-topic-other
Partition: 0 in Topic: my-topic
Topic: my-topic, Partition: 0, Offset: 64, Key: null, Value: S
Topic: my-topic, Partition: 0, Offset: 65, Key: null, Value: S
Topic: my-topic, Partition: 0, Offset: 66, Key: null, Value: S
```

```shell
c:\kafka>bin\windows\kafka-producer-perf-test.bat --topic my-topic --num-records 50 --record-size 1 --throughput 10 --producer-props bootstrap.servers=localhost:9092 key.deserializer=org.apache.kafka.common.serialization.StringDeserializer value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
c:\kafka>bin\windows\kafka-producer-perf-test.bat --topic my-topic-other --num-records 50 --record-size 1 --throughput 10 --producer-props bootstrap.servers=localhost:9092 key.deserializer=org.apache.kafka.common.serialization.StringDeserializer value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```
Run ConsumerSubscribeApp
```shell
mvn compile exec:java -Dexec.mainClass="ConsumerSubscribeApp"
```
and see that messages are consumed for all partitions my-topic and my-topic-other
