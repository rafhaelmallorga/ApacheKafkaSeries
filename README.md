
### What is Apache Kafka?

#### How companies start?

To understand what Apache Kafka solves, let's understand how companies start.

So companies will have a source system, for example, a database. And at some point another part of the company will want to take that to put it into another system. For example, a target system.

So the data has to move from a source system to a target system. And at first, it's very simple. Someone who writes some code and then take the data, exact it, transform it, and then load it.

![[Pasted image 20241012161737.png | 200]]

#### After a while...

Now after a while, your company evolves and has many source systems and also has many target systems. And now your data integration challenges just got a lot more complicated, because all your source systems must send data to all your target systems to share information.

And as we can see, we have a lot of integration. 

![[Pasted image 20241012165127.png]]

#### Types of problems organizations are facing with the previous architecture.

- So the previous architecture is that, for example, if we have **4 source system** and **6 target systems**, you're going to have to write **24 integrations** to make it work;
- Each integration comes with difficulties around:
	- Protocol - how the data is transported (TCP, HTTP, REST, FTP, JDBC...), the protocol may differ;
	- Data format - how the data is parsed (Binary, CSV, JSON, Avro, Protobuf...);
	- Data schema & evolution - how the data is shaped and may change;
-  Each source system will have an **increased load** from the connections to extract the data.

#### Why Apache Kafka: Decoupling of data streams & system

How do we solve this problem? Well, we bring some decoupling using **Apache Kafka**.

So we still have our source systems and our target system, but in the middle, will sit Apache Kafka. 

![[Pasted image 20241012174503.png]]

What happens?

Now the source system are responsible for sending data. **It's called producing for producing data into Apache Kafka.** 

So now **Apache Kafka** is going to have a data stream of all your data, of all our source systems within it.

And our target systems, if they ever need to receive data from our source systems, they will actually tap into the data of **Apache Kafka**. Because Kafka is meant to actually receive and send data.

So our target systems are now consuming from Apache Kafka, and everything looks a little bit better and a little bit more scalable.

So if we go back to the same example, what can be our source systems for example?

Well, it could be Website Events, Pricing Data, Financial Transactions and User Interactions and all these things create data streams. That means data created in real time, and it is sent to Apache Kafka.

Now our target system could be Databases, Analytics System, Email Systems and Audit System. So this is the kind of architecture we we'll implement.

#### Why is Apache Kafka is so good?

Well, Kafka was created by LinkedIn, and it was created as an open source project. Now it's mainly maintained by big corporations such as Confluent, IBM, LinkedIn and so on. 

It's **Distributed, Resilient Architecture, Fault tolerant.**

That means that we can upgrade Kafka without put all the system down.

Kafka is also very good, because we have horizontal scalability. That means that you can add brokers over time into your Kafka cluster, and we can scale to hundreds of broker.

It also has huge scale for messages throughputs. So we can have millions of messages per second (This is the case of Twitter).

It's really high performance. So we have really low latency and sometimes it's measured in less than 10 milliseconds, which is why we call Apache Kafka a real time system.

#### Resume
- Distributed, resilient architecture, fault tolerant;
- Horizontal scalability;
	- Can scale to 100 hundreds of brokers;
	- Can scale to millions of messages in real time.
- High performance (latency of less than 10 ms) - real time;
- Used by the 2000+ firms, **80% of the Fortune 100**.

#### Use cases

- Messaging system;
- Activity Tracking;
- Gather metrics from many different locations;
- Application Logs gathering;
- Stream processing (with the Kafka Streams API for example);
- De-coupling of system dependencies;
- Integration with Spark, Flink, Storm, Hadoop, and many other Big Data technologies;
- Micro-services pub/sub.

#### Real world examples...

- **Netflix**: Uses Kafka to apply recommendations in real-time while you're watching TV shows
- **Uber**: Uses Kafka to gather user, taxi and trip data in real-time to compute forecast demand, and compute surge pricing in real-time;
- **LinkedIn**: Uses Kafka to prevent spam, collect user interactions to make better connection recommendations in real time.

Remember that Kafka is only used as a transportation mechanism!

### Kafka Topics

So, **Kafka Topics** are a particular stream of data within our Kafka Cluster. A Kafka Cluster can have many topics, and it could be named, for example logs, purchases, twitter_tweets, trucks_gps, and so on. 

![[Pasted image 20241012232744.png | 150]]

A topic in Kafka is a stream of data, and we can make a parallel to databases, a topic is similar to what a table is in data base, but without all the constraints. Because we send whatever we want to a Kafka topic, there is no data verification.

We can have many topics as we want in our Kafka Cluster and the way to identify a topic in a Kafka cluster is by its name. That's why I have logs, purchases, twitter_tweets and truck_gps, those are all names for my Kafka topics.

The Kafka Topics supports any kind of messages formats. And then, we can send, for example, JSON, Avro, text file, binary whatever you want.

The sequence of the messages in a topic, is called a data stream. And this is why Kafka is called a data streaming platform, because we make data stream through topics.

We cannot query topics. So topics are similar to table database, but we cannot query them, instead to add data into a Kafka topic, we're going to use Kafka Producers. And to read data from a topic, we're going to use Kafka Consumers. But there is no querying capability within Kafka.

##### Resume
- **Topics**: a particular stream of data;
- Like a table in a database (without all the constraints);
- You can have as many topics as we want;
- A topic is identified by its **name**;
- Any kind of message format;
- The sequence of messages is called a __data stream___;
- We cannot query topics, instead, use Kafka Producers to send data and Kafka Consumers to read the data.

#### Partitions and offsets

Topics are general, but we can divide them into partitions. So a topic can be made up of for example, 100 partitions.  

![[Kafka_Topics.png]]

The messages sent to a Kafka topic are going to end up in these partitions, and messages within each partition are going to be ordered. So, my first messages into partition zero will have the id zero, one and then two, etc... These ids are always incrementing from zero to whatever. This id is called a Kafka partition **OFFSET**. Each partition has different offsets.

Kafka topics are also immutable, that means that once the data is written into a partition, it cannot be changed. We cannot delete data in Kafka, we cannot update data in Kafka, we have to keep on written to the partition.

So, let's note some important things about topics, partitions and offsets. Once a data is written to a partition it will not be changed, it cannot be changed that that's called immutability. 

Data in Kafka is only kept for a limited time and the default is one week, although that is configurable. That means that after one week, your data will disappear.

The offsets only have a meaning for a specific partition. As we can see in the image above, the offsets are repeated across partitions. And the offsets are not going to be reused even if previous messages have been deleted. It keeps on increasing incrementally one by one, as we send messages into our Kafka topic. That means also that the order of messages is guaranteed only within a partition but not across partitions.

And then the data when sent to a Kafka topic, is going to be assigned to a random partition, unless we provide a key.
##### Resume
- Topics are split in __partitions__ (example: 100 partitions)
	- Messages within each partition are ordered;
	- Each message within a partition gets an incremental id, called __offset__.
- Kafka topics are __immutable__: once data is written to a partition, it cannot be changed;
- Once the data is written to a partition, __it cannot be changed__ (immutability);
- Data is kept only for a limited time (default is one week - configurable);
- Offset only have a meaning for a specific partition;
	- E.g. offset 3 in partition 0 doesn't represent the same data as offset 3 in partition 1;
	- Offsets are not re-used eve if previous messages have been deleted.
- Order is guaranteed only within a partition (not across partitions);
- Data is assigned randomly to a partition unless a key is provided (more on this later);
- You can have as many partitions per topic as you want.

### Producers

Producers are going to write to the topic, partitions. The producers is right before that is going to send data into your ==Kafka Topic, partitions==.

![[Producers.png]]

Producers know in advance to which partition they write to, and then which ==Kafka Broker==, which is a Kafka server has it. So that means that the producers know in advance in which partition the message is going to be written. Some people think that Kafka decides at the end, the server which partition data get written to, this is wrong.

The producer decides in advance which partition to and we'll see how. And then in case A, Kafka server that has a partition as a failure, the producers know how to automatically recover. So there is a lot of behind the scenes magic.

We have load balancing in this case because the producers, they're going to send data across all partitions based on some mechanism, and this is why Kafka scales, it's because we have many partitions within a topic and each partition is going to receive messages from one or more producers.

So now producers have message keys in the message. So the message itself contain data, but then we can add a key and it's optional, and the key can be anything you want, could be a string, a number, a binary, etc...

If the key is null, that means that the data is going to be sent round robin. If the key is not null that means that has some value, and the Kafka producers have a very important property, that is that all the messages that share the same key will always end up being written to the same partition, thanks to a hashing strategy.

##### Resume
- Producers write data to topics (which are made o partitions);
- Producers know to which partition to write to (and which Kafka broker has it);
- In case of Kafka broker failures, Producers will automatically recover;
- Producers can choose to send a ==key== with the message (string, number, binary, etc...);
- If key=null, data is sent round robin (partition 0, then 1, then 2...);
- if key!=null, then all messages for that key will always go to the same partition (hashing);
- A key are typically sent if you need message ordering for a specific field (ex: truck_id).

#### Kafka Messages anatomy...

Here is what a Kafka message looks like when created by a producer. So we have the key and it can be null as we've seen, and it's in a binary format. Then we have the value, which is your message content. It can be null as well, but usually is not, and it contains the value of our message.

![[kafka_message.png]]

Then we can add compression onto our messages, if we desire it to be smaller. So we can specify a compression mechanism, for example, gzip, snappy, lz4 or zstd. 

We can also add headers to our message, which are optional list of key value pairs.

Then we have the partition  that the message is going to be sent to as well as its Offsets.

And the finally, a timestamp. That is either set by the system or by the user.

This is what a Kafka message is, and then it gets sent into Apache Kafka for storage.

### Kafka Message Serializer

How do these messages get created? So we have what's called a Kafka Message Serializer. Because Kafka is a very good technology, and what makes it good is that it only accepts series of bytes as an input from producers, and it will send bytes as an output to consumers.

![[Serializers.png]]

But when we construct Messages, they're not bytes. So we are going to perform message serialization. ==And doing serialization means that we are going to transform our data and our objects into bytes==.

And then ==these Serializers are going to be used only on the value and the key.==

But then we're going to specify the key Serializer to be an integer Serializer. And what's going to happen is that Kafka producer is smart enough to transform that key objects __"123"__ through the Serializer into a series of bytes, which is going to give us a binary representation of that key.

And then for the Value Object we're going to specify a string Serializer, as you see the value and the key Serializer in this instance are different. And so that means it's good to be smart enough to transform the string, ==hello world== into a series of bytes for our value. 

And now that we have the key and the value as binary representations, that message is now ready to be sent into Apache Kafka.

Kafka producers come with common Serializers that help you do this transformation. So we have string, including the JSON representation of the String, Integer, Floats, Avro, Protobuf and so on. We can find a lot of message serializers out there.

#### Kafka Message Key Hashing

There is something called a Kafka partitioner, which is a code logic that will take a record, a message and determine to which partitioner to send it to. So when we do a send, the producer partitioner logic is going to look at the record and then assign it to a partition, for example, partition one, and then it gets sent by the producer into Apache Kafka. 

And the process of key hashing is used to determine the mapping of a key to a partition, and in the default Kafka partitioner, then the keys are going to be hashed using the __murmur2__ algorithm, and there is a formula right here that we're going to know.

```Javascript
targetPartition = Math.abs(Utils.murmur2(keyBytes)) % (numPartitions - 1)
```

That means that it's going to look at the bytes of the key, apply the __murmur2__ algorithm, and then figure out, thanks to it, what is going to be the target partition.

This is just to stress the fact that producers are the one who choose where the message is going to end up, thanks to the key bytes.

##### Resume
- A Kafka partitioner is a code logic that takes a record and determines to which partition to send it into;
- __Key Hashing__ is the process of determining the mapping of a key to a partition;
- In the default Kafka partitioner, the keys are hashed using the __murmur2 algorithm__, with the formula above.

### Consumers

To read data from a topic we use the consumers. Consumers implement the pool model, that means that consumers are going to request data from the ==Kafka Brokers==, the servers, and then they will get a response back. It's not the Kafka brokers pushing data to the consumers.

![[Consumers.png]]

So in the example above we have three topic partitions that contain data, and then one consumer reading data from topic-A Partition zero and other consumer reading data from topic-A Partitions one and two. 

So the consumers the consumers when they need to read data from a partition, they will automatically know which broker, which Kafka server to read from. And in case of a broker has a failure the consumer know how to recover for this.

Now the data of the read data being read for all these partitions is going to be read in order, from low to high offset, so zero, one, two... and so on within each partition.

In our example, the first consumer is going to read data from the Partition 0, from the offset 0 all the way to offset 11. Same for consumer 2, that is going to reading data in order for partition 1, and in order for partition 2 but remembering that there is no ordering guarantees across partition 1 and partition 2 because they are different partitions.

#### Consumer Deserializer

So now that consumers read messages, they need to transform these bytes that they receive from Kafka into objects or data. 

![[consumer_deserializer.png]]

So we have the key that is a binary format and value that is a binary format, which corresponds to the data in your Kafka message. And then we need to transform them to read them and put them into an object that our programming language can use.

So the consumers has to know in advance what is the format of your messages? And this consumers knows that my key is an integer therefore is going to use an integer Deserializer to transform my key that is by into an integer.

And then the key object is going to be back to one, two, three. Same for the value, we know that we need a Deserializer of type string because this is that we expect to be in this Kafka topic, and therefore, this Deserializer is going to take bytes as an input and then create a string  out of it. So we're going to get back our value object, ==hello world==.

So obviously, Deserializers are being bundled with Apache Kafka and they can be used by your consumers. So it could be for string including JSON, Integer, Floats, Avro, Protobuf and so on.

And as we see, we have a process of Serializer at the producer side and Deserializer at the consumer side, and the consumer needs to know in advance what is the expected format for you key and your value. That means that within your topic life cycle, so as long as your topic is created, ==you must absolutely no change the type of data that is being tent by the producers==, because otherwise you're going to break our consumers because they're going to expect for a example, integer and string, but we're going to change them into Floats and Avro who knows... So if we want to change the data type of a topic, what we have to do is to create a new topic instead and in this new topic we can have whatever format we want, and then our consumers will have to be reprogrammed a little bit to read from these new topics with this new format.
##### Resume
- Consumers read data from a topic (identified by name) - pull model;
- Consumers automatically know which broker to read from;
- In case of broker failures, consumers know how to recover;
- Data is read in order from low to high offset __within each partitions__.
- Deserialize indicates how to transform bytes into objects / data;
- They are used  on the value and the key of the message;
- Common Deserializers:
	- String (incl. JSON);
	- Int, Float;
	- Avro;
	- Protobuf.
- __The serialization / deserialization type must not change during a topic lifecycle (create a new topic instead)__.

### Consumer Groups

When we have Kafka and we want to scale we're going to have many consumers in an application and they're going to read data as a group and it's called a consumer group. 

In the example below  of a Kafka topic with five partitions, and then we have a consumer group that is called Consumer Group Application, and that consumer group has three consumers, one, two and three. Then each consumer within the group if they belong to the same group are going to be reading from exclusive partitions. That means that my Consumer 1 is going to read from partition zero and maybe partition one. My consumer 2 is going to read from partition two and partition three, and finally my Consumer 3 is going to read from Partition 4. 

![[consumer_group.png]]

So as we can see consumer one, two and three are sharing the reads from all the partitions and they read all from distinct partition, this way a group is reading the Kafka as a whole. 

##### Resume
- All the consumers in an application read data as a consumer groups;
- Each consumer within a group reads from exclusive partitions; 

#### What if too many consumers?

So what if we have too many consumers in our consumer group, more than partitions? So let's see the example below of Topic A, with partition zero, one and two, and then my consumer group application has consumer one, two and three. So in this case, we know that the mapping is simple we have each consumer reading from one partition and then if we add another consumer into this consumer group and __we can, it's possible__. 

Then that Consumer 4 is going to be inactive. And that means that it's just is going to be stand by consumer, and it's not going to read from any topic partitions and that's okay, but you need to know that it's normal. That Consumer 4 is not going to help Consumer 1 to read from Partition 0, it's going to stay inactive.

![[many_consumers.png]]

And also we can have multiple consumer groups on one topic so it is completely acceptable to have multiple consumer groups on the same topic, and let's take an example. So we go back to our topic with three partitions and then we have our first consumer group that I've named Consumer Group Application 1, and it has two consumers. Now they're going to share their reads from our topic partitions, so Consumer 1 is going to read from two partitions, and Consumer 2, just from one and that's fine. 

And then we have a second consumer group application and this one will have three consumers, and each of them are going to be reading from a distinct partition. 

And finally, if we have a consumer group three with just one consumer, that consumer is going to be reading from all topic partitions. So as we can see, it's fine to have multiple consumer groups on the topic, then each partition will have multiple readers.

![[multiple_consumers_groups.png]]


> [!INFO] Info
> In Apache Kafka it is acceptable to have multiple consumer groups on the same topic.

Consumers groups are useful when we have different services consuming data from the same topic, for example, a consumer group for location service, and another to  notification service.

To create distinct consumer groups, we use the consumer property name group id to give a name to a consumer group, and then consumer will know in which group they belong.

#### Consumer Offsets

And these groups they're even more powerful than what we think. So in this group we can define consumer offsets.

Well, Kafka is going to store the offsets at which a consumer group has been reading. And these offsets are going to be in a Kafka topic named consumers offsets with underscores in the beginning because it's an internal Kafka topic.

So, let's check an example and we'll understand why consumer offsets are so important. So we have this topic, and what I represented right here vertically is an offset, so we've been writing a lot in this topic and now we have number 4258, all the way up. So we have a consumer from within the consumer group and is going to commit once in a while. And when the offsets are committed, this is going to allow the consumer to keep on reading from that offsets onwards. And the idea is that when a consumer is done processing the data that is received from Apache Kafka, it should once in a while commit the offsets and tell the Kafka brokers to write to the consumer offset topic, and by committing the offsets we're going to be able to tell the Kafka broker how far we've been successfully reading into the Kafka topic and so this is why we do it once in a while.

![[consumer_group_commit.png]]

Why we do this? Because if our consumer dies then comes back and then is going to be able to read back from where it left it off, thanks to the committed consumer offsets, because Kafka is going to say, hey in this partition two, it seems you have been reading up to these offsets for 4262 then when you restart, please will only send you data from this offsets onwards. And this is thanks to consumer groups offsets that we're going to be able to have some mechanism to replay data form where we have crashed or failed.

##### Resume
- __Kafka__ stores the offsets at which a consumer group has been reading;
- The offsets committed are in Kafka __topic__ named **__consumer_offsets**;
- When a consumer in a group has processed data received from Kafka, it should be __periodically__ committing the offsets (the Kafka broker will write to **__consumer_offsets**, not the group itself);
- If the consumer dies, it will be able to read back from where it left off thanks to the committed consumer offsets!

#### Delivery semantics for consumers

So that means that we have different delivery semantics for consumer, and we'll explore the those in detail later. But by default, the Java consumers will automatically commit offsets in an at least once mode. But if we choose to commit manually, we have three delivery semantics,

__At least once (usually preferred)__ which means that the offsets are going to be committed right after the message is processed and in case the processing goes wrong, then there's a chance we are going to read that message again. So that means that we can have duplicate processing of messages in this setting, and so we need to make sure that our processing is it idempotent, that means that when you process again the messages it will not impact our system.

__At most once__, and the effect of this is that we commit offsets as soon as the consumers receive messages but if the processing goes wrong then some messages are going to be lost because there won't be read again because we have committed offsets sooner than actually processing the message, so that means that we see messages at most once.

__Exactly once__ where we want to process messages just once. So when we do Kafka to Kafka workflow that means when we read from topic and then we write back to topic as result we can use the transaction API, which is very easy to use if we use the Kafka streams API as well, for example, or if go from Kafka to an external system then we need to use an idempotent consumer.

##### Resume
- By default, Java Consumers will automatically commit offsets (at least once);
- There are 3 delivery semantics if you choose to commit manually;
- __At least once (usually preferred)__;
	- Offsets are committed after the message is processed;
	- If the processing goes wrong, the message will be read again;
	- This can result in duplicate processing of messages. Make sure your processing is <u>idempotent</u> (i.e. processing again the messages won't impact your system);
- __At most once__;
	- Offset are committed as soon as messages are received;
	- If the processing goes wrong, some messages will be lost (they won't be read again);
- __Exactly once__;
	- For Kafka: Kafka workflow -> use the Transactional API (easy with Kafka Streams API);
	- For Kafka: External System workflows: use an <u>idempotent</u> consumer.

### Kafka Brokers

So, a Kafka cluster is an ensemble of multiple Kafka brokers, and a broker is just a server, in Kafka they are called brokers because they receive and send data.

A broker will be identified with an ID, which is an integer, and so for example, we're going to have Broker 101, Broker 102 and Broker 103 in our cluster.

Now, each broker is going to contain only certain topic partitions. That means that our data is going to be distributed across all brokers, ans we will see how in below.

>[!INFO] Bootstrap Broker
>When we connect to any Kafka broker, also called a bootstrap broker, then the clients or the producers or the consumers will be connected and know how to connect to the entire Kafka cluster, because the Kafka clients have a smart mechanics for this.

So that means that we don't need to know in advance all the brokers in our cluster, just to know how to connect to one broker, and then our clients will automatically connect to the rest. 

That means that our Kafka cluster can be made of as many brokers as we want. And a good number to get started is going to be 3 brokers, but some big clusters are going to have over 100 brokers in them.

#### Brokers and topics

Example of __Topic-A__ with ==3 partitions== and __Topic-B__ with ==2 partitions==.

And then we have three Kafka brokers (101, 102 and 103).

![[cluester_of_brokers.png]]

So as we can see, the topic partitions are going to be spread out across all brokers in whatever order. With the example above we can see that the data is distributed and it's normal that Broker 103 does not have any Topic-B partition. Because the two partitions have already been placed on our Kafka broker.

#### Kafka Broker Discovery

Each Kafka broker in your cluster is called a bootstrap server. So let's take an example of five brokers in our Kafka cluster, only the __Broker 101__ is represented as bootstrap but all of them are actually bootstrap servers.

![[broker_discovery 1.png]]

So that means that in this cluster, we only need to connect to one broker and then the clients will know how to be connected to the entire cluster. So our Kafka client is going to initiate a connection into broker 101, as well as a metadata request. And then the broker 101, if successful, is going to return the list of all brokers in the cluster, and actually more data as well, such which broker has which partition.

##### Resume
- Every Kafka broker is also called a "bootstrap server";
- That means that __we only need to connect to one broker__, and the Kafka clients will know how to be connected to the entire cluster (smart clients);
- Each broker knows about all brokers, topics and partitions (metadata).

### Topic Replication Factor

Topics in Kafka, when we're doing stuff on our own machine, they can have a replication factor of one, but usually when we are in production, real life cluster, we need to set a replication factor greater than one, usually between two and three and most commonly at three. 

So that way, if a broker is down, that means a Kafka server is stopped for maintenance or for a technical issue. Then another Kafka broker still has a copy of the data to serve and receive.

For example, if we have a Topic-A, it has two partitions and a replication factor of two. So we have three Kafka brokers, and we're going to place partition zero of Topic-A onto broker 101, partition one of a Topic-A onto broker 102. 

![[replication.png]]

And then because we have a replication factor of two then we're going to have a copy of partition zero onto broker 102 with a replication mechanism, and a copy of partition one onto broker 103, with again, a replication mechanism.

If we loose the broker 102, we'll still having both partitions.
##### Resume
- Topics should have a replication factor > 1 (usually between 2 and 3);
- This way if a broker is down, another broker can serve the data;
- Example: Topi-A with 2 partitions and replication factor of 2.

#### Concept of Leader for a Partition

And so therefore we have a leader for a partition, and a any time one broker can be a leader for a given partition. And the rule is that producers can only send data to the broker that is the leader of a partition.

So if we go back to our example, we can check the leader of each partition with the star. 

![[leader_partition.png]]

And we can see that broker 101 is the leader of partition 0, and broker 102 is the leader of partition one, but broker 102 is a replica of partition zero, and broker 103 is a replica of partition one.

So the others brokers replicate the data. And if the data is replicated fast enough then each replica is going to be called as ISR (in-sync replica).

#### Default producer & consumer behavior with leaders

So by default, and this is default behavior with leaders, your producers are going to only write into leader broker for a partition. So if the producer knows it wants to send data into partition zero, and we have a leader and a ISR then the producer knows that it should only send the data into the broker that is the leader of that partition.

And the Kafka consumers, they're going to read that default only from the leader of a partition so that means that the consumer will only request data from the leader broker 101.

That means that broker 102 in the previous example is a replica just for a sake of replicating data, and in case  the broker 101 goes down then it can become the new leader, and serve the data for the producer and the consumer.

### Producer Acknowledgements (acks)

So we have the producers sending data into our brokers, and so the producers can choose to receive acknowledgements of data rights. That means to have the confirmation from the Kafka broker that the write did successfully happen.

![[acks.png]]

So we have three settings:
- __acks=0__: Producer won't wait for acknowledgment (possible data loss), if a broker goes down, we won't know about it;
- __acks=1__: Producer will wait for leader acknowledgment (limited data loss);
- __acks=all__: Leader + replicas acknowledgment (no data loss).

#### Kafka Topic Durability

- For a topic replication factor of 3, topic data durability can withstand 2 brokers loss;

### Zookeeper

[[Zookeeper]] has been how Kafka was able to function all the way up until today, but it's slowly disappearing and it's going to be replaced. 

So, zookeeper, are managing Kafka brokers and Zookeepers is a software. And it's going to keep a list of our Kafka brokers. It's also going to be very helpful for Kafka because whenever we have a broker going down, we need to perform a leader election to choose new leader for partitions, and Zookeeper is going to help with this process.

Also, Zookeeper is going to send notifications to Kafka brokers in case of changes. For example, when a new topic is created, when a Kafka broker goes down, or comes up, deletion of topics and son on.

So, it has a lot of the Kafka metadata. And Kafka all the way up until version 2.X, it cannot work without Zookeeper.

Zookeeper has been since the beginning of Kafka, a companion to Kafka brokers, and we cannot launch Kafka without launching Zookeeper.

But now stating with Kafka 3.X, we can have Kafka work on its own without Zookeeper, it's called the Kafka Raft mechanism instead. So, it's Kraft or Kafka Raft. We can learn more about it searching for [[KIP-500]].

And then, in version Kafka 4.X we will not have Zookeeper anymore. So, right now the community is transitioning to making Kafka work with Zookeeper correctly, and then, at some point, migrate and move without a, to a Zookeeper less Kafka.

Zookeeper by design is going to operate with an odd version of servers. So, either we have one Zookeeper, or three Zookeeper, or five Zookeeper, or seven Zookeeper. Never more than seven usually. It also have a concept o leaders and the rest are followers. 

>[!WARNING] Atention
>Zookeeper  does not hold any consumer data nowadays. In old versions it used to keep the offset data.

##### Resume
- Zookeeper manages brokers (keeps a list of them);
- Zookeeper helps in performing leader election for partitions;
- Zookeeper sends notifications to Kafka in case of changes (e.g. new topic, broker dies, broker comes up, delete topic, etc...);
- __Kafka 2.x can't work without Zookeeper__;
- __Kafka 3.x can work without Zookeeper (KIP-500) - using Kafka Raft instead__;
- __Kafka 4.x will not have Zookeeper;
- Zookeeper by design operates with an odd number of servers (1, 3, 5, 7);
- Zookeeper has a leader (writes) the rest of the servers are followers (reads);
- (Zookeeper does NOT store consumer offsets with Kafka > v 0.10).

#### Zookeeper Cluster (ensemble)

![[zookeeper.png]]

So if we look at Zookeeper, we have three servers. The second one is the leader, and then the brokers are connected to Zookeeper, and that's how they get their metadata. 

#### About Kafka KRaft

- In 2020, the Apache Kafka project started to work __to remove the Zookeeper dependency__ from it (KIP-500);
- Zookeeper shows scaling issues when Kafka clusters have > 100.000 partitions;
- By removing Zookeeper, Apache Kafka can:
	- Scale to millions of partitions, and becomes easier to maintain and set-up;
	- Improve stability, makes it easier to monitor, support and administer;
	- Single security model for the whole system;
	- Single process to start with Kafka;
	- Faster controller shutdown and recovery time.
- Kafka 3.X now implements the Raft protocol (KRaft) in order to replace Zookeeper:
	- Producing ready since Kafka 3.3.1 (KIP-833);
	- Kafka 4.0 will be released only with KRaft (no Zookeeper).

With Quorum Controller (without Zookeeper, only Kafka controllers) and one of hem is the Quorum leader. So we can see the simplified architecture. 

Also, KRaft gives us performance improvements. In the image below we can see the control shutdown time as well as the recovery time after uncontrolled shutdown is substantially better. So, overall KRaft is a great improvement.

![[KRaft.png]]

### Conduktor

- __Easiest way to get started with Apache Kafka__;
- Free - start Kafka online without a credit card;
- Comers with a UI to speed development;
- Only use it for personal usage / learning purposes;
- https://conduktor.io/get-started.

### Setup Kafka Binaries

- Install Java JDK (Java Development Kit) version 11 [Corretto 11](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/generic-linux-install.html);
- Download Apache Kafka from https://kafka.apache.org/quickstart;
- Extract the contents on Linux;
- Setup the $PATH environment variables for easy access to the Kafka binaries.

### Running Kafka with Zookeeper

To run the Zookeeper .sh file we need to pass some configurations to that, that in our case are in the ==config/zookeeper.properties==. Without this configuration it'll crash.

```bash
# Start the ZooKeeper service
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```

If everything is __ok__ we will see the follow log:

![[zookeeper_log.png]]

```bash
# Start the Kafka broker service
$ bin/kafka-server-start.sh config/server.properties
```

Now in a new tab we're going to start Kafka:

![[kafka_logs.png]]

As we can see, now Kafka is running and the log show us ==[KafkaServer id=0]== .

### Running Kafka with KRaft mode

Generate a Cluster UUID

```bash
$ KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```

Format Log Directories

```bash
$ bin/kafka-storage.sh format -t $KAFKA_CLUSTER_ID -c config/kraft/server.properties
```

Start the Kafka Server

```bash
$ bin/kafka-server-start.sh config/kraft/server.properties
```

![[Pasted image 20241016003857.png]]

### Kafka CLI

#### kafka-topics.sh

So now that we have Kafka Cluster and we know that it's made out of topics, and so therefore we're going to do topic management using the CLI.

- Create Kafka Topics;
- List Kafka Topics;
- Describe Kafka Topics;
- Increase Partitions in a Kafka Topic;
- Delete a Kafka Topic.

##### Create a new topic

To create a new topic we use the follow command:

```bash
bin/kafka-topics.sh --create --topic first_topic --bootstrap-server localhost:9092
```

![[Pasted image 20241016010810.png]]

To check our topic info we can use the command passing the __--describe__ param:

```bash
bin/kafka-topics.sh --describe --topic first_topic --bootstrap-server localhost:9092
```

![[Pasted image 20241016010837.png]]

In our first topic we just have 1 partition and the leader of the topic is the Broker 1, as we just have one broker.

Let's create another topic, but this time we will create it with 5 partitions now using the param __--partitions 5__:

```bash
bin/kafka-topics.sh --create --topic second_topic --partitions 5 --bootstrap-server localhost:9092
```

 Now, let's list our topics using the command below:

```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

![[Pasted image 20241016011527.png]]

Let's describe our __second_topic__ to check if it really has 5 partitions:

```bash
bin/kafka-topics.sh --describe --topic second_topic --bootstrap-server localhost:9092
```

![[Pasted image 20241016011707.png]]

As we can see, we have the second topic with 5 partitions, but the replication factor is __1__. So, if we desire to create replicas of our topics we must use the follow command, passing the param __--replication_factor 2__, for example:

```bash
bin/kafka-topics.sh --create --topic third_topic --partitions 3 --replication-factor 2 --bootstrap-server localhost:9092
```

But now we have a problem, Kafka throws an __Error__, why? Because running locally we just have one cluster and as we saw on replication section, we can't have a number of replication bigger than our number of clusters.

![[Pasted image 20241016012443.png]]

>[!WARNING]
>Error while executing topic command : Unable to replicate the partition 2 time(s): The target replication factor of 2 cannot be reached because only 1 broker(s) are registered.
>
[2024-10-16 01:23:00,185] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Unable to replicate the partition 2 time(s): The target replication factor of 2 cannot be reached because only 1 broker(s) are registered.

So, let's just pass --replication-factor 1 to make it work:

![[Pasted image 20241016020233.png]]

And finally, to delete a topic we use the param __--delete__:

```bash
bin/kafka-topics.sh --topic first_topic --delete --bootstrap-server localhost:9092
```

![[Pasted image 20241016020752.png]]

#### kafka-console-producer.sh

This script allow us to produce data into our Kafka topics. So we're going to practice two use cases.

1. Without sending keys;
	- The data will be distributed across all partitions.
2. Sending keys.
	- The same key always going to the same partition.

The command to write on a topics is below, this command will start on our terminal the wright mode:

```bash
bin/kafka-console-producer.sh --topic first_topic --bootstrap-server localhost:9092
```

So, every message that we type and press enter will write on our topic, for example:

```bash
> Hello world
> My name is Rafhael Mallorga
> I love Kafka
```

To read our topic we use the follow command:

```bash
bin/kafka-console-consumer.sh --topic first_topic --from-beginning --bootstrap-server localhost:9092
```

We can write in a topic using __--producer-property__, for example, the property __acks__ (acknowledge) with __all__ value to right in all topic partitions.

```bash
bin/kafka-console-producer.sh --topic first_topic --producer-property acks=all --bootstrap-server localhost:9092
```

By default, if we try to right on a topic that don't exist, Kafka will create the topic for us.

#### Producing with key

When we use keys, the same keys will go to the same partition, and that's the behavior we will verify.

The property that we use for that is __parse.key=true__,  and the property is __key.separator=:__ ,
So that means that when we produce a message, what is left of the colon is the key, and what is right of the colon is value.

```bash
bin/kafka-console-producer.sh --topic first_topic --property parse.key=true --property key.separator=: --bootstrap-server localhost:9092
```

#### kafka-console-consumer.sh

So we know that consumers can read, from partitions, the data in order, and as well in the group, we'll see this later on. And so we're going to practice an example to read a Kafka topic. 

So we'll consume from the tale of the topic, from beginning and show both key and values in the output.

Let's create another producer passing a producer property called the =="partitioner"== class, and this is called a __"RoundRobinPartitioner"__. The reason to use that is because we want to produce to one partition at a time, and change every partition. 

>[!INFO]
>If we don't use this round robin partitioner, there have been so many optimizations built in into Kafka right now that we will keep on the same partition up until we send about 16 kilobytes of data,  and then we will switch partition.

```bash
bin/kafka-console-producer.sh --topic second_topic --producer-property partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner --bootstrap-server localhost:9092
```

To log the keys and values from our topics we must to use the follow consumer settings:
```bash
bin/kafka-console-consumer.sh --topic second_topic --formatter kafka.tools.DefaultMessageFormatter --property print.timestamp=true --property print.key=true --property print.value=true --property print.partition=true --from-beginning --bootstrap-server localhost:9092
```

##### Consumer in Groups

Now that we now how Kafka consumers works, we're going to be able to start them in a consumer group, learn about the group parameter. And it's going to show us how partitions are divided amongst multiple CLI consumers.

![[Pasted image 20241016221615.png]]

Sow we have, for example, five partitions and then three consumers, and each consumer is consuming from a different partition, they're all distinct. So that's the behavior we're going to try to explore by creating multiple consumers within the same group.

Now what we're going to do is to consume, from this topic, but now we'll use a new param, the __--group__ follow by a group id. 

```bash
bin/kafka-console-consumer.sh --topic third_topic --group my-first-application --from-beginning --bootstrap-server localhost:9092
```

We can create 3 consumers using this group because we have 3 partition, and the producer will wright onto the topic, but the consumers will receive the messages separately and distributed by Round Robin. 

If we create more consumer than partitions one of consumer will be standing by.

>[!INFO]
>The re-balance is made automatically by Kafka when we create our delete a consumer.

>[!IMPORTANT]
>When we create a new consumer from a existing group, it doesn't read our messages from beginning.
>But, if we create a new consumer with a new group id, it'll read our topic since the beginning.

For example:

```
bin/kafka-console-consumer.sh --topic third_topic --group my-second-application --from-beginning --bootstrap-server localhost:9092
```


#### kafka-consumer-groups.sh

So now we're going to see how to list the existing consumer groups. We're going to describe one consumer group and then we're going to delete a consumer group.

To list all our consumer groups:

```bash
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```

To describe a group:

```bash
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-first-application
```

![[Pasted image 20241016225023.png]]

If we produce messages without any consumer consumption, we'll generate a lag on our broker. And it'll updated when we start consuming again.

![[Pasted image 20241016225622.png]]

##### Reset Offsets

To reset the offsets of our groups we can use __--dry-run__ param, dry run reset the offsets to the beginning of each partition.

```bash
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to--earliest --topic third_topic --dry-run
```

After that we need to execute the reset, using the follow command:

```bash
bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --topic third_topic --execute
```

Now the lag of our topic increased and the current off-set is 0.

![[Pasted image 20241016230943.png]]

Now if we run a consumer again it'll read all the messages from the beginning.

>[!IMPORTANT]
>To reset offsets we must stop all the consumers.



### Set Kafka Producer Serializer

We can specify how the producer is going to behave about serialization. In Kafka when we use a producer, we pass in some information and at first there will be a string and that will be serialized into bytes by this __key.serializer__ and the __value.serializer__, before being sent to Apache Kafka. 

```java
properties.setProperty("key.serializer", StringSerializer.class.getName());  
properties.setProperty("value.serializer", StringSerializer.class.getName());
```

That means that our producer is expecting strings and these strings will be serialized using the StringSerializer which is a class provided with the Kafka clients.
