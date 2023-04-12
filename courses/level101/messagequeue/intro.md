# Messaging services


## What to expect from this course

At the end of training, you will have an understanding of what a Message Queue is, learn about different types of Message Queue implementation and understand some of the underlying concepts & trade offs.

## What is not covered under this course

We will not be deep diving into any specific Message Queue. 


## Course Contents


## Introduction


### Types of Messaging services:

1. **General-purpose message queue:**

General-purpose message queues are versatile and can be used in various non-very-scenarios, from distributing tasks and buffering requests to enabling communication between microservices. These messaging systems are designed to provide reliable message delivery and ensure that messages are processed in the correct order, and handle message volumes typically up to 100,000 messages per second. Message queues often support multiple messaging patterns, such as point-to-point and publish-subscribe, providing flexibility for different use cases. Examples of general-purpose message queues include RabbitMQ, ActiveMQ, and Amazon SQS. By using these message queues, developers can decouple their applications and scale them independently, improving overall system resilience and performance.

2. **Pub/Sub messaging:** 

Publish-Subscribe (Pub/Sub) messaging services allow publishers to send messages to multiple subscribers without direct point-to-point connections. This enables decoupling of producers and consumers, making the system more scalable and fault-tolerant. Pub/Sub systems are particularly useful in scenarios where multiple consumers need to receive and process the same messages, such as sending notifications, logging, or data replication. The Pub/Sub model supports dynamic subscription management, allowing consumers to subscribe and unsubscribe from specific topics or channels at runtime. Examples of Pub/Sub messaging services include Google Cloud Pub/Sub, Apache Pulsar, Azure Event Grid, AWS SNS and NATS. By adopting a Pub/Sub messaging system, developers can create event-driven architectures, reduce system complexity, and streamline the integration of new services.

3. **Streaming processing:** 

Stream processing services are designed to handle large volumes of real-time data (say 1 milion messages per second), allowing continuous processing and analysis of data streams. These services enable complex event processing, time-windowed aggregations, and stateful transformations. They provide a robust platform for building real-time analytics, monitoring, and alerting applications. Stream processing systems often use a combination of in-memory and disk-based storage to balance performance and durability. They also support horizontal scaling, allowing developers to process massive data volumes with low latency. Examples of stream processing services include Apache Kafka, Apache Flink, Amazon Kinesis Data Streams, Azure Event Hubs, RocketMQ, Apache Pulsar and Redis Streams. By leveraging stream processing services, organizations can gain valuable insights from their data in real-time and make data-driven decisions more effectively.

4. **Brokerless:**

Brokerless messaging systems enable direct communication between producers and consumers without relying on a central broker, thereby reducing latency and improving performance. In brokerless systems, nodes communicate with each other using a peer-to-peer architecture, which can simplify deployment and reduce the need for dedicated message broker infrastructure. These systems are particularly suitable for high-performance, low-latency applications or situations where network connectivity is intermittent or unreliable. Examples of brokerless messaging systems include ZeroMQ, nanomsg, Chronicle Queue and the Disruptor. By adopting a brokerless messaging system, developers can build lightweight, fast, and efficient communication channels between components in their distributed systems

5. **Database-as-queue**

In some cases, a traditional relational or NoSQL database can be used as a message queue, allowing for simpler integration with existing systems and providing familiar tools for data management. This approach can be particularly useful for smaller-scale applications or as a transitional step when migrating from monolithic to distributed architectures. Using a database-as-queue can leverage built-in database features like transactions, indexing, and querying to manage messages effectively. Examples of using a database-as-queue include PostgreSQL's LISTEN/NOTIFY feature or leveraging Amazon DynamoDB Streams. While using a database-as-queue might not provide the same performance and scalability as dedicated messaging services and sometimes is considered as an anti-pattern, it can be a suitable option for specific use cases or when the application requirements are less demanding.
