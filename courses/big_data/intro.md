# Big Data

## Prerequisites

- Basics of Linux File systems.
- Basic understanding of System Design.

## What to expect from this course

This course covers the basics of Big Data and how it has evolved to become what it is today. We will take a look at a few realistic scenarios where Big Data would be a perfect fit. An interesting assignment on designing a Big Data system is followed by understanding the architecture of Hadoop and the tooling around it. 

## What is not covered under this course

Writing programs to draw analytics from data.

## Course Contents

1. [Overview of Big Data](https://linkedin.github.io/school-of-sre/big_data/overview/)
2. [Usage of Big Data techniques](https://linkedin.github.io/school-of-sre/big_data/overview/)
3. [Evolution of Hadoop](https://linkedin.github.io/school-of-sre/big_data/evolution/)
4. [Architecture of hadoop](https://linkedin.github.io/school-of-sre/big_data/architecture/)
    1. HDFS
    2. Yarn
5. [MapReduce framework](https://linkedin.github.io/school-of-sre/big_data/architecture/#mapreduce-framework)
6. [Other tooling around hadoop](https://linkedin.github.io/school-of-sre/big_data/architecture/#other-tooling-around-hadoop)
    1. Hive
    2. Pig
    3. Spark
    4. Presto
7. [Data Serialisation and storage](https://linkedin.github.io/school-of-sre/big_data/architecture/#data-serialisation-and-storage)

# Overview of Big Data

1. Big Data is a collection of large datasets that cannot be processed using traditional computing techniques. It is not a single technique or a tool, rather it has become a complete subject, which involves various tools, techniques and frameworks.
2. Big Data could consist of
    1. Structured data
    2. Unstructured data
    3. Semi-structured data
3. Characteristics of Big Data:
    1. Volume
    2. Variety
    3. Velocity
    4. Variability
4. Examples of Big Data generation include stock exchanges, social media sites, jet engines, etc.


# Usage of Big Data techniques

1. Take the example of the traffic lights problem.
    1. There are more than 300,000 traffic lights in the US as of 2018.
    2. Let us assume that we placed a device on each of them to collect metrics and send it to a central metrics collection system.
    3. If each of the IOT devices sends 10 events per minute, we have 300000x10x60x24 = 432x10^7 events per day.
    4. How would you go about processing that and telling me how many of the signals were “green” at 10:45 am on a particular day?
2. Consider the next example on Unified Payments Interface (UPI) transactions:
    1. We had about 1.15 billion UPI transactions in the month of October, 2019 in India.
    12. If we try to extrapolate this data to about a year and try to find out some common payments that were happening through a particular UPI ID, how do you suggest we go about that?
