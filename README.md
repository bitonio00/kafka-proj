# kafka-project 

## I. What is Kafka ?

Apache Kafka is a distributed streaming platform that is used for building real-time data pipelines and streaming applications. It was developed by the Apache Software Foundation and is written in Scala and Java.

Kafka is designed to handle high volumes of data efficiently and with low latency. It can process millions of messages per second and can handle terabytes of data without any issues. Kafka is used by many companies around the world, including LinkedIn, Netflix, and Twitter, to build real-time data pipelines and streaming applications.

Kafka works by using a publish-subscribe model, where producers write data to Kafka topics and consumers read data from those topics. Kafka topics are partitioned and replicated across a cluster, which allows for high availability and fault tolerance. Kafka also has strong durability, meaning that data is persisted on disk and can be recovered in the event of a failure.

Kafka has many features that make it useful for building real-time data pipelines and streaming applications. Some of these features include:

- High throughput: Kafka can handle high volumes of data efficiently, with low latency.
- Scalability: Kafka can scale horizontally by adding more brokers to the cluster.
- Fault tolerance: Kafka replicates data across multiple brokers and can recover from failures.
- Durability: Kafka persists data on disk, which means that data is not lost in the event of a failure.
- Real-time processing: Kafka can process data in real-time, making it useful for streaming applications.
- Integration: Kafka can integrate with other systems and technologies, such as Hadoop, Spark, and Elasticsearch.

Overall, Kafka is a powerful and reliable platform that is used by many companies around the world to build real-time data pipelines and streaming applications.

## II. Using kafka on window 

Kafka was not at first built to run on window but you can do it with a certain configuration. Knowing that we were using window we had to do those steps.

1. Install WSL2
2. Install Java JDK version 11
3. Download Apache Kafka
4. Extract the contents on WSL2
5. Start Zookeeper using the binaries in WSL2
6. Start Kafka using the binaries in another process in WSL2
7. Setup the $PATH environment variables for easy access to the Kafka binaries

## III. Our project using Kafka

We built a kafka producer application.

A Kafka client that publishes records to the Kafka cluster.
The producer is thread safe and sharing a single producer instance across threads will generally be faster than having multiple instances.
The producer consists of a pool of buffer space that holds records that haven’t yet been transmitted to the server as well as a background I/O thread that is responsible for turning these records into requests and transmitting them to the cluster.

How does a Kafka producer work?
Kafka producers write messages into Kafka servers, while Kafka consumers fetch or consume data from Kafka servers. Usually, to produce and consume simple messages or message queues, you can write code or commands using the default CLI tool that comes with Kafka installation

### Cluster Information

First we create a file in configuration/ccloud.properties that allow us to connect to our cluster, we can find there tha api key of our cluster

### Topic

```
confluent kafka topic create output-topic --partitions 1
```
We create a topic for our project. 
Kafka topics are the categories used to organize messages. Each topic has a name that is unique across the entire Kafka cluster. Messages are sent to and read from specific topics. In other words, producers write data to topics, and consumers read data from topics.

### Configure the project 

In build.gradle we make the configuration of our project
Gradle is a build automation tool known for its flexibility to build software. A build automation tool is used to automate the creation of applications. The building process includes compiling, linking, and packaging the code. The process becomes more consistent with the help of build automation tools.

### Producer properties

 Is done in configuration/dev.properties
 where you will find :
 -key.serializer 
 -value.serializer 
 -acks 
 
 ### KafkaProducer application
 
 In src/main/java/io/confluent/developer/KafkaProducerApplication.java we have the 
 -Constructor
 -KafkaProducerApplication.produce method does some processing on a String
 -KafkaProducer.send method is asynchronous and returns as soon as the provided record is placed in the buffer of records to be sent to the broker. Once the broker         acknowledges that the record has been appended to its log, the broker completes the produce request, which the application receives as RecordMetadata—information about the committed message.
 
 ### Data to produce
 
 In input.txt you will find the data that we want to produce
 If you run this command tha application will process the file
 ```
 java -jar build/libs/kafka-producer-application-standalone-0.0.1.jar configuration/dev.properties input.txt
  ```
  You should have this output
  ```
  Offsets and timestamps committed in batch from input.txt
Record written to offset 0 timestamp 1597352120029
Record written to offset 1 timestamp 1597352120037
Record written to offset 2 timestamp 1597352120037
Record written to offset 3 timestamp 1597352120037
Record written to offset 4 timestamp 1597352120037
Record written to offset 5 timestamp 1597352120037
Record written to offset 6 timestamp 1597352120037
Record written to offset 7 timestamp 1597352120037
Record written to offset 8 timestamp 1597352120037
Record written to offset 9 timestamp 1597352120037
Record written to offset 10 timestamp 1597352120038
  ```
 If you want to change the data in input.txt re-run this command

### Consuming from topic

```  
confluent kafka topic consume output-topic -b --print-key --delimiter " : "
```
With this command : console consumer will run that will read topics from the output topic to confirm your application published the expected records.

The output expected is the input.txt
``` 
1 : value
2 : words
3 : All Streams
4 : Lead to
5 : Kafka
6 : Go to
7 : Kafka Summit
8 : How can
9 : a 10 ounce
10 : bird carry a
11 : 5lb coconut
``` 

 So in conclusion we create a KafkaProducer to write records to Kafka.
 
