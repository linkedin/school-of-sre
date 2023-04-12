# Key Concepts

Lets looks at some of the key concepts when we talk about messaging queue system


### Delivery guarantees			

One of the essential aspects of messaging services is ensuring that messages are delivered to their intended recipients. Different systems offer varying levels of delivery guarantees, and it is crucial to understand these guarantees to choose the right messaging service for your needs.

*   **at-most-once-delivery**

This guarantee ensures that a message is delivered at most once to its intended recipient. In other words, messages may be lost, but they will never be delivered more than once. This approach is suitable for scenarios where message loss is tolerable and duplication is not desired.

*   **at-lesat-once-delivery**

Under this guarantee, a message will be delivered to its intended recipient at least once, but it may be delivered multiple times in case of failures. This approach is appropriate for situations where message loss is unacceptable, but duplication can be managed by the recipient.

*   **exactly-once-delivery**

This guarantee ensures that a message is delivered exactly once to its intended recipient, with no loss or duplication. This is the most stringent level of delivery guarantee and is suitable for applications where both message loss and duplication are unacceptable.

Selecting the right delivery guarantee depends on the specific requirements of your application. For example, in financial transactions, an exactly-once delivery guarantee is essential to avoid double-processing of payments or missed transactions. In contrast, a log monitoring system may only require at-most-once delivery to reduce system overhead.


### Messages ordering and parallelism

Ensuring the correct order of messages and processing them in parallel can be a challenge in distributed messaging systems. The following strategies help maintain order and ensure parallelism:

* Strict ordering:
In some cases, maintaining strict order is essential, such as when processing financial transactions. This may require additional overhead, such as sequencing numbers, buffering, and reordering messages.

* Partial ordering:
Partial ordering can be used when only a subset of messages must be ordered. For example, messages within a specific group or partition must be processed in order, but messages between groups or partitions can be processed independently.

* Unordered processing:
In some scenarios, processing messages in any order is acceptable. This approach reduces complexity and enables higher parallelism, improving overall system performance.

Strategies to maintain order and ensure parallelism:
Partitioning messages by a key, using sequencing numbers, and buffering can help maintain order while still allowing parallel processing. It is crucial to strike the right balance between ordering requirements and parallelism to optimize system performance.



### Fan Out / In
Fan out and fan in are two crucial concepts in messaging systems that deal with the distribution of messages among multiple consumers and the aggregation of messages from multiple producers, respectively.

#### Fan Out
Fan out is a pattern where a single message is sent to multiple consumers, ensuring that each consumer receives a copy of the message. This can be achieved using the Publish-Subscribe (Pub/Sub) messaging pattern or by creating multiple bindings with unique routing keys in a message broker like RabbitMQ. Fan out is useful in scenarios where multiple services or applications need to process the same messages independently, such as sending notifications to multiple subscribers or replicating data across multiple databases.

#### Fan In
Fan in is a pattern where messages from multiple producers are aggregated and processed by a single consumer or a group of consumers. This can be achieved by using a message broker with multiple producers sending messages to a shared queue, which is then consumed by one or more consumers. Fan in is beneficial when you need to centralize processing or consolidate data from multiple sources, such as aggregating logs from various services or combining sensor data from multiple IoT devices.


### Poison Pills and Dead Letters
In messaging systems, poison pills are problematic messages that can cause failures or crashes in the message processing pipeline. To handle these messages, messaging services often employ Dead Letter Queues (DLQs).

A poison pill is a message that cannot be processed due to various reasons, such as invalid format, missing information, or incorrect data. When a consumer encounters a poison pill, it must handle it gracefully to avoid crashing or getting stuck in an infinite processing loop.

A Dead Letter Queues (DLQ) is a separate queue used to store poison pills or messages that could not be processed successfully. Instead of discarding problematic messages, they are redirected to a DLQ, allowing engineers to analyze and resolve the issues.

To handle poison pills effectively, you can implement error handling and monitoring, set up retries with backoff policies, and use DLQs for further analysis and resolution. Regularly monitor the DLQ, identify patterns causing poison pills, and implement fixes in the message processing pipeline to prevent future issues.


## Use case

### Work queues

### Publish-subscribe pattern


