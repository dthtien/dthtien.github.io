# What is Kafka?

## Publish/Subscribe
Pub/sub is the messaging pattern where the sender doesn't send data directly to a specific receiver. Instead, the publisher classifies
the messages without knowing if there are any subscribers interested in a particular type of messages. Similarly, the
receiver subscribes to receive a certain class of messages without knowing if there are any sender sending those
messages.

Pub/sub systems usually have a broker, where all messages are published. This decouples publishers from subcribers and
allows for greater flexibility in the type of data that subscribes want to receive. It also reduces the number of
potential connections between publishers and subscribers.
Bulletin board comes handy as a good analogy to a pub/sub messaging pattern where people can publish information in a central place without knowing who the recipients.

## What is Kafka?
Distributed event log where all new records are immutable and appended to the end of the log.
In Kafka, messages are persisted on disk for a certaint period of time known as the retention policy. This is usally the main difference
between Kafka and other messaging systems that makes Kafka some way a hybird between a messaging system and a database.

The main concepts behide Kafka are producers producing messages to different topics and consumer consuming those
messages and maintaining the position in the stream of data. You can think about producer as publishers or senders of
messages. Consumber on the other hand are analogous to receivers or subscribers.
Kafka aims to provide a reliable and high throughput platform for handling real-time data streams and building data
pipelines. It also provides a single place for storing and distributing events. That can be fed into multiple downstream
system which helps to fight the ever-growing problem of integration complexity. Besides all of that Kafka can also be
easily used to build a modern and scalable ETL( exact transform load ), CDC (change data capture) or big data ingest
system.
## Kafka architecture
![](https://finematics.com/wp-content/uploads/2019/09/kafka-architecture-1024x586.png)
On a high level, Kafka architecture consists of a Kafka cluster, producer and consumer.
Kafka very often acts like a central hub for all the events in the system which means it's a perfect plance to connect
to if we are interested in particular type of data.

EXP: a database that can consume and persist message or an elastic search cluster that can consume certain events and
provide full-text search capabilities for other applications
### Brocker
  - A single Kafka server within the cluste.
  - A Kafka cluster usually consists of at least three brokers to provide enough level of redundancy.
  - Responsible for receiving messages from producers, assigning offsets and commiting messages to disk
  - Responsible for responding to consumers fetch requests and serving messages
When messages are sent to a broker, they are sent to a particular topic.
### Topic
  - Provide a way of categorising data that is being sent
  - Further broken down into a number of partitions -> scaling easy as each paritition can be read by a separate
    consumer -> achieving high throughput as both partitions and consumbers can be split across multiple servers
  EXP: a system can have separate topics for processing new users and for processing metrics. Each partition acts as a
  separate commit log and the order of messages is guarenteed only across the same partition
### Producers
  - Applications procude data
    - EXP: our application producing metrics and sending them to our Kafka cluster
### Consumer
  - Applications consume data from Kafka
## Messages, Batches, Schema, Topics and Partitions
  ### Messages
    - A message is a single unit of data that can be sent or received which is a byte array.
    - A message can also have an optional key that can be used to write data in a more controlled way to multiple
   partitions with the same topic

    Example: We want to write our data to multiple partitions as it will be easier to scale the system later. We relised
    that certain messages, let's say for each user have to be written in order. If our topic has multiple partitions,
    there is no guarantee which messages will be written to which partitions, most likely the new message woule be
    written to partitions a round-robin fashion. **To advoid that situation we can define a consistent way for choosing
    the same partition based on a message key. One way of doing that would as simple as using user_id %
    number_of_partitions that would assign always the same partition to the same user. **

  ### Batches
    - Sending single messages over the network creates a lot of overhead, that's why messages are written into Kafka in
  batches.
    - A batch is a collection of messages produced for the same topic and partition.
    - Trade-off between letency and throughput and can be futher controller by adjusting a few Kafka setting
    - Batches can be compressed which provide even more efficient data transfer
  ### Schema
    - JSON
    - XML
    - Avro
    - Protobuf
  ### Ordering guaranteer
    - The only way to achieve the ordering for all messages is to have only one partition which we can be sure that events
    are always ordered by the time they were written into Kafka
    - Each partition can be hosted on a different server which means that a single topic can be scaled horizontally across
      multitple servers to improve the throughput
## Producers, Consumers, Offsets, Consumer Groups and Rebalancing
  ### Producers
      - Create new messages and send them to a specific topic.
      - If a partition is not specified and a topic has multiple partitions, messages would be written into multiple
        partitions evenly -> message key
  ### Consumers
      - Read message
      - Subcribe to one or multiple topics and read messages in the order they were produced
      - Keep track of its position in the stream of data by remembering what offset was already consumed
  ### Offsets
    - Created at a time a message is written to Kafka
    - Correspond to a specific message in a specific partition
    - With the same topic, multiple partition can have different offset and it's up to the consumer to remember what
    offset each partition is at
    - By storing offsets in Zookeeper or Kafka itself a consumer can stop and restart withou losing its position in the
      stream of data
  ### Consumer groups
  ![](https://finematics.com/wp-content/uploads/2019/09/kafka-consumer-groups-1024x597.png)
    - Consumers always belong to a specifc consumer group
    - Consumers within a consumer group work together to consum a topic
    - The group make sure that each partition is only consumed by one member of a consumer group
    ### Rebalancing
    - Consumer can scle horizontally to consume topics which a large number of messages. Additionally, if a single
      consumer failes, the remaining members of the group will rebalace the partitions to make it up for the missing
      member.
    - In case we want to consume the same messages multiple times, we have to make sure the consumers belong to different
      consumer groups. This can be useful if we have multiple applications that have to process the same data separately
## Clusters, Brokers and Retention
  ### Clusters
    - Consist of multiple servers called brokers
  ### Broker
    - Depending on the specfic hardware, a single broker can easily handle thousands of partitions and millions of
      messages per second
    - Designed to be operate as part of a cluster.
    - Within a cluster of brokers, one broker will also act as the cluster controller which is responsible for
      adminitrative operations: assigning partitions to brokers and monitoring for broker failures.
    - A partition is always owned y a single broker in the cluster who is called the leader of the partition.
    - A partition may be assigned to multiple brokers which will result in the partition being replicated. This provides
      redundancy of messages in the partition, such that another broker can take over leadership if the is a broker
      failure.
    - All consumers and producers operating on partition which has been assigned to multiple brokers must connect to the leader
  ### Retention
  - For some period of time, provides durable storage of messages
  - Kafka brokers are configured with a default retention setting for topics either retaining messages for some period
    of time, 7 days by default or until the topic reaches a certain size in bytes EG: 1GB.
  - The oldest messages are expired and delete when the limits are reaches so that retention configuration is minimum
    amount of data available at anytime
  - Individual topics can also configure their own retention setting. EG: a topic for storing metrics might have very
    short retention of a few hours. On the other hand, a topic containing bank transfers might have a retention policy
    of a few months.
## Reliability Guarantees
  - Kafka guarentees the order of messages in partition. If message A was written before message B, using the same
    producer in the same partition, then Kafka guarantees that the offset of message B will be higher then message A.
    This means that consumers will read message A before message B
  - Messages are considered **committed** when they are written to the leader and all in-sync replicas (number of
    in-sync replicas and number of acks can be configured)
  - Committed messages won't be lost as long as at least one replica remains alive and retention policy holds
  - Consumers can only read commited messages
  - Kafka provivides at-least once messae delivery semantics - doesn't prevent duplicated messages being produced. To
    achieve the exactly one semantics, we have to either rely on an external system with some support for unique key or
    use Kafka Stream
## Kafka and other messaging systems
  - The main difference is that Kafka consumers pull messages from brokers which allow or buffering messages for as long
    as the retention holds. In most other JMS system, messages are actually pushed to the consumer instead. Pushing
    messages to consumer makes thinks like backpressure really hard to achieve.
  - Kafka also makes replaying of events easy as messages are stored on disk and can be replayed anytime within the
    limis of retention period.
  - Guarantees the ordering of messages within one partition and it provides an easy way for build scalable and fault
    tolerant system.

## Pros and Cons
  ### Pros
    - Tackles integration complexity
    - Great tool for ETL, CDC, Big data ingestion
    - High throughput
    - Disk-based retention
    - Support multiple producers/consumers
    - Highly scalable
    - Fault-tolerant
    - Fairly low-latency
    - Highly configurable
    - Provides backpressure
  ## Cons
  - Requires a fair amount of time to understand and do not shoot yourself in the foot by accident
  - Might not be the best solution for the real low-latency systems
# References
https://www.youtube.com/watch?v=JalUUBKdcA0
