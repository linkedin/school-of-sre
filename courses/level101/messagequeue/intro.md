# Messaging services


## What to expect from this course

At the end of training, you will have an understanding of what a Message Queue is, learn about different types of Message Queue implementation and understand some of the underlying concepts & trade offs.

## What is not covered under this course

We will not be deep diving into any specific Message Queue. 


## Course Contents


## Introduction


### Types of Messaging services:

1. **General-purpose message queue:** . 
Typcially it supports various of message queue protocol like AMQP, MQTT, .  
Thoughput are under 100K messages per second. Examples are Rabbitmq, ActiveMQ.

2. **Pub/Sub messaging:** 
Examples are Azure Event Grid, AWS SNS

3. **Streaming processing:** 
processes 1 milion messages per second
 To scaling out, those solution will pick a sharding (partitioning). Only the message in the same partition can be processed in order. 
Message comsumption is not transtional.
Kafka, AWS Kinesis stream, Azure Event Hubs, RocketMQ, Apache Pulsar, Redis Streams, 

4. **Brokerless:** ex zeromq, Chronicle Queue

5. **Database-as-queue** Normal people will consider it is an anti-pattern to 
