# NoSQL Concepts


## What to expect from this course

At the end of training, you will have an understanding of what a NoSQL database is, what kind of advantages or disadvantages it has over traditional RDBMS, learn about different types of NoSQL databases and understand some of the underlying concepts & trade offs w.r.t to NoSQL.


## What is not covered under this course

We will not be deep diving into any specific NoSQL Database. 


## Table of Contents



*   Introduction to NoSQL
*   CAP Theorem
*   Data versioning
*   Partitioning
*   Hashing
*   Quorum


## Introduction

When people use the term “NoSQL database”, they typically use it to refer to any non-relational database. Some say the term “NoSQL” stands for “non SQL” while others say it stands for “not only SQL.” Either way, most agree that NoSQL databases are databases that store data in a format other than relational tables.

A common misconception is that NoSQL databases or non-relational databases don’t store relationship data well. NoSQL databases can store relationship data—they just store it differently than relational databases do. In fact, when compared with SQL databases, many find modeling relationship data in NoSQL databases to be _easier_, because related data doesn’t have to be split between tables.

Such databases have existed since the late 1960s, but the name "NoSQL" was only coined in the early 21st century. NASA used a NoSQL database to track inventory for the Apollo mission. NoSQL databases emerged in the late 2000s as the cost of storage dramatically decreased. Gone were the days of needing to create a complex, difficult-to-manage data model simply for the purposes of reducing data duplication. Developers (rather than storage) were becoming the primary cost of software development, so NoSQL databases optimized for developer productivity. With the rise of Agile development methodology, NoSQL databases were developed with a focus on scaling, fast performance and at the same time allowed for frequent application changes and made programming easier.


### Types of NoSQL databases:

Over time due to the way these NoSQL databases were developed to suit requirements at different companies, we ended up with quite a few types of them. However, they can be broadly classified into 4 types. Some of the databases can overlap between different types. They are



1. **Document databases: **They store data in documents similar to [JSON](https://www.json.org/json-en.html) (JavaScript Object Notation) objects. Each document contains pairs of fields and values. The values can typically be a variety of types including things like strings, numbers, booleans, arrays, or objects, and their structures typically align with objects developers are working with in code. The advantages include intuitive data model & flexible schemas. Because of their variety of field value types and powerful query languages, document databases are great for a wide variety of use cases and can be used as a general purpose database. They can horizontally scale-out to accomodate large data volumes. Ex: MongoDB, Couchbase
2. **Key-Value databases:** These are a simpler type of databases where each item contains keys and values. A value can typically only be retrieved by referencing its value, so learning how to query for a specific key-value pair is typically simple. Key-value databases are great for use cases where you need to store large amounts of data but you don’t need to perform complex queries to retrieve it. Common use cases include storing user preferences or caching. Ex: [Redis](https://redis.io/), [DynamoDB](https://aws.amazon.com/dynamodb/), [Voldemort](https://www.project-voldemort.com/voldemort/)/[Venice](https://engineering.linkedin.com/blog/2017/04/building-venice--a-production-software-case-study) (Linkedin), 
3. **Wide-Column stores:** They store data in tables, rows, and dynamic columns. Wide-column stores provide a lot of flexibility over relational databases because each row is not required to have the same columns. Many consider wide-column stores to be two-dimensional key-value databases. Wide-column stores are great for when you need to store large amounts of data and you can predict what your query patterns will be. Wide-column stores are commonly used for storing Internet of Things data and user profile data. [Cassandra](https://cassandra.apache.org/) and [HBase](https://hbase.apache.org/) are two of the most popular wide-column stores.
4. Graph Databases: These databases store data in nodes and edges. Nodes typically store information about people, places, and things while edges store information about the relationships between the nodes. The underlying storage mechanism of graph databases can vary. Some depend on a relational engine and “store” the graph data in a table (although a table is a logical element, therefore this approach imposes another level of abstraction between the graph database, the graph database management system and the physical devices where the data is actually stored). Others use a key-value store or document-oriented database for storage, making them inherently NoSQL structures. Graph databases excel in use cases where you need to traverse relationships to look for patterns such as social networks, fraud detection, and recommendation engines. Ex: [Neo4j](https://neo4j.com/) 


### **Comparison** 


<table>
  <tr>
   <td>
   </td>
   <td>Performance
   </td>
   <td>Scalability
   </td>
   <td>Flexibility
   </td>
   <td>Complexity
   </td>
   <td>Functionality
   </td>
  </tr>
  <tr>
   <td>Key Value
   </td>
   <td>high
   </td>
   <td>high
   </td>
   <td>high
   </td>
   <td>none
   </td>
   <td>Variable
   </td>
  </tr>
  <tr>
   <td>Document stores
   </td>
   <td>high
   </td>
   <td>Variable (high)
   </td>
   <td>high
   </td>
   <td>low
   </td>
   <td>Variable (low)
   </td>
  </tr>
  <tr>
   <td>Column DB
   </td>
   <td>high
   </td>
   <td>high
   </td>
   <td>moderate
   </td>
   <td>low
   </td>
   <td>minimal
   </td>
  </tr>
  <tr>
   <td>Graph
   </td>
   <td>Variable
   </td>
   <td>Variable
   </td>
   <td>high
   </td>
   <td>high
   </td>
   <td>Graph theory
   </td>
  </tr>
</table>



### Differences between SQL and NoSQL

The table below summarizes the main differences between SQL and NoSQL databases.


<table>
  <tr>
   <td>
   </td>
   <td>SQL Databases
   </td>
   <td>NoSQL Databases
   </td>
  </tr>
  <tr>
   <td>Data Storage Model
   </td>
   <td>Tables with fixed rows and columns
   </td>
   <td>Document: JSON documents, Key-value: key-value pairs, Wide-column: tables with rows and dynamic columns, Graph: nodes and edges
   </td>
  </tr>
  <tr>
   <td>Primary Purpose
   </td>
   <td>General purpose
   </td>
   <td>Document: general purpose, Key-value: large amounts of data with simple lookup queries, Wide-column: large amounts of data with predictable query patterns, Graph: analyzing and traversing relationships between connected data
   </td>
  </tr>
  <tr>
   <td>Schemas
   </td>
   <td>Rigid
   </td>
   <td>Flexible
   </td>
  </tr>
  <tr>
   <td>Scaling
   </td>
   <td>Vertical (scale-up with a larger server)
   </td>
   <td>Horizontal (scale-out across commodity servers)
   </td>
  </tr>
  <tr>
   <td>Multi-Record <a href="https://en.wikipedia.org/wiki/ACID">ACID </a>Transactions
   </td>
   <td>Supported
   </td>
   <td>Most do not support multi-record ACID transactions. However, some—like MongoDB—do.
   </td>
  </tr>
  <tr>
   <td>Joins
   </td>
   <td>Typically required
   </td>
   <td>Typically not required
   </td>
  </tr>
  <tr>
   <td>Data to Object Mapping
   </td>
   <td>Requires ORM (object-relational mapping)
   </td>
   <td>Many do not require ORMs. Document DB documents map directly to data structures in most popular programming languages.
   </td>
  </tr>
</table>



### Advantages



*   Flexible Data Models

    Most NoSQL systems feature flexible schemas. A flexible schema means you can easily modify your database schema to add or remove fields to support for evolving application requirements. This facilitates with continuous application development of new features without database operation overhead.

*   Horizontal Scaling

    Most NoSQL systems allow you to scale horizontally, which means you can add in cheaper & commodity hardware, whenever you want to scale a system. On the other hand SQL systems generally scale Vertically (a more powerful server). NoSQL systems can also host huge data sets when compared to traditional SQL systems.

*   Fast Queries

    NoSQL can generally be a lot faster than traditional SQL systems due to data denormalization and horizontal scaling. Most NoSQL systems also tend to store similar data together facilitating faster query responses. 

*   Developer productivity

    NoSQL systems tend to map data based on the programming data structures. As a result developers need to perform fewer data transformations leading to increased productivity & fewer bugs.
