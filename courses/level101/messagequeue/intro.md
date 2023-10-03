# Messaging services


## What to expect from this course

At the end of training, you will have an understanding of what a Message Services is, learn about different types of Message Service implementation and understand some of the underlying concepts & trade offs.

## What is not covered under this course

We will not be deep diving into any specific Message Service. 


## Course Contents

*   [Introduction to Messaging Service](https://linkedin.github.io/school-of-sre/level101/messagequeue/intro/#introduction)
*   [Delivery guarantees](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#delivery-guarantees)
*   [Messages ordering and parallelism](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#messages-ordering-and-parallelism)
*   [Fan Out / In](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#fan-out--in)
*   [Poison Pills and Dead Letters](https://linkedin.github.io/school-of-sre/level101/messagequeue/key_concepts/#poison-pills-and-dead-letters)

## Introduction

In today's distributed systems and microservices architectures, messaging services play a crucial role in ensuring reliable communication and coordination between different components. These services enable the asynchronous exchange of messages, providing a wide range of benefits, such as increased performance, improved fault tolerance, and enhanced scalability.

This article will provide an overview of the various types of messaging services available, including general-purpose message queues, pub/sub messaging, stream processing, brokerless messaging, and database-as-queue systems. We will also explore key concepts like delivery guarantees, message ordering, parallelism, poison pills, and dead letters, which are essential to understanding how messaging services function and how they can be effectively utilized.


### Types of Messaging services:

In this section, we will explore various types of messaging services, each designed to address different requirements and use cases in distributed systems.

1. **General-purpose message queue:**  General-purpose message queues are versatile and can be used in various non-very-scenarios, from distributing tasks and buffering requests to enabling communication between microservices. These messaging systems are designed to provide reliable message delivery and ensure that messages are processed in the correct order, and handle message volumes typically up to 100,000 messages per second. Message queues often support multiple messaging patterns, such as point-to-point and publish-subscribe, providing flexibility for different use cases. Examples of general-purpose message queues include [RabbitMQ](https://www.rabbitmq.com/), [ActiveMQ](https://activemq.apache.org/), and [Amazon SQS](https://aws.amazon.com/sqs/). By using these message queues, developers can decouple their applications and scale them independently, improving overall system resilience and performance.

2. **Pub/Sub messaging:**  Publish-Subscribe (Pub/Sub) messaging services allow publishers to send messages to multiple subscribers without direct point-to-point connections. This enables decoupling of producers and consumers, making the system more scalable and fault-tolerant. Pub/Sub systems are particularly useful in scenarios where multiple consumers need to receive and process the same messages, such as sending notifications, logging, or data replication. The Pub/Sub model supports dynamic subscription management, allowing consumers to subscribe and unsubscribe from specific topics or channels at runtime. Examples of Pub/Sub messaging services include [Google Cloud Pub/Sub](https://cloud.google.com/pubsub), [Apache Pulsar](https://pulsar.apache.org/), [Azure Event Grid](https://azure.microsoft.com/en-us/products/event-grid), [AWS SNS](https://aws.amazon.com/sns/) and [NATS](https://nats.io/). By adopting a Pub/Sub messaging system, developers can create event-driven architectures, reduce system complexity, and streamline the integration of new services.

3. **Streaming processing:**  Stream processing services are designed to handle large volumes of real-time data (say 1 milion messages per second), allowing continuous processing and analysis of data streams. These services enable complex event processing, time-windowed aggregations, and stateful transformations. They provide a robust platform for building real-time analytics, monitoring, and alerting applications. Stream processing systems often use a combination of in-memory and disk-based storage to balance performance and durability. They also support horizontal scaling, allowing developers to process massive data volumes with low latency. Examples of stream processing services include [Apache Kafka](https://kafka.apache.org/), [Amazon Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/), [Azure Event Hubs](https://azure.microsoft.com/en-us/products/event-hubs), [RocketMQ](https://rocketmq.apache.org/), [Apache Pulsar](https://pulsar.apache.org/) and [Redis Streams](https://redis.io/docs/data-types/streams/). By leveraging stream processing services, organizations can gain valuable insights from their data in real-time and make data-driven decisions more effectively.

4. **Brokerless:**  Brokerless messaging systems enable direct communication between producers and consumers without relying on a central broker, thereby reducing latency and improving performance. In brokerless systems, nodes communicate with each other using a peer-to-peer architecture, which can simplify deployment and reduce the need for dedicated message broker infrastructure. These systems are particularly suitable for high-performance, low-latency applications or situations where network connectivity is intermittent or unreliable. Examples of brokerless messaging systems include [ZeroMQ](https://zeromq.org/), [nanomsg](https://nanomsg.org/), [Chronicle Queue](https://github.com/OpenHFT/Chronicle-Queue) and [the Disruptor](https://lmax-exchange.github.io/disruptor/). By adopting a brokerless messaging system, developers can build lightweight, fast, and efficient communication channels between components in their distributed systems

5. **Database-as-queue**  In some cases, a traditional relational or NoSQL database can be used as a message queue, allowing for simpler integration with existing systems and providing familiar tools for data management. This approach can be particularly useful for smaller-scale applications or as a transitional step when migrating from monolithic to distributed architectures. Using a database-as-queue can leverage built-in database features like transactions, indexing, and querying to manage messages effectively. Examples of using a database-as-queue include PostgreSQL's LISTEN/NOTIFY feature or leveraging Amazon DynamoDB Streams. While using a database-as-queue might not provide the same performance and scalability as dedicated messaging services and sometimes is considered as an anti-pattern, it can be a suitable option for specific use cases or when the application requirements are less demanding.

### Comparsion

|                        | Performance        | Scalability        | Flexibility          | Complexity          | Functionality        | Ease of Use          |
|------------------------|--------------------|--------------------|----------------------|---------------------|----------------------|----------------------|
| General-purpose MQ     | Moderate           | Moderate           | High                 | Moderate            | High                 | Moderate             |
| Pub/Sub                | Moderate to High   | High               | High                 | Moderate            | Moderate to High     | Moderate to High     |
| Stream processing      | High               | High               | Moderate to High     | High                | High                 | Moderate             |
| Brokerless             | High               | Moderate           | Moderate             | Low to Moderate     | Moderate             | High                 |
| Database-as-queue      | Low to Moderate    | Low to Moderate    | Moderate             | Low                 | Low to Moderate      | High                 |


