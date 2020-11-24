*   Relational DBs are used for data storage. Even a file can be used to store data, but relational DBs are designed with specific goals:
    *   Efficiency
    *   Ease of access and management
    *   Organized
    *   Handle relations between data (represented as tables)
*   Transaction: a unit of work that can comprise multiple statements, executed together
*   ACID properties

    Set of properties that guarantee data integrity of DB transactions

    *   Atomicity: Each transaction is atomic (succeeds or fails completely)
    *   Consistency: Transactions only result in valid state (which includes rules, constraints, triggers etc.)
    *   Isolation: Each transaction is executed independently of others safely within a concurrent system
    *   Durability: Completed transactions will not be lost due to any later failures

	Let’s take some examples to illustrate the above properties.

    *   Account A has a balance of ₹200 & B has ₹400. Account A is transferring ₹100 to Account B. This transaction has a deduction from sender and an addition into the recipient’s balance. If the first operation passes successfully while the second fails, A’s balance would be ₹100 while B would be having ₹400 instead of ₹500. **Atomicity** in a DB ensures this partially failed transaction is rolled back.
    *   If the second operation above fails, it leaves the DB inconsistent (sum of balance of accounts before and after the operation is not the same). **Consistency** ensures that this does not happen.
    *   There are three operations, one to calculate interest for A’s account,  another to add that to A’s account, then transfer ₹100 from B to A. Without **isolation** guarantees, concurrent execution of these 3 operations may lead to a different outcome every time.
    *   What happens if the system crashes before the transactions are written to disk? **Durability** ensures that the changes are applied correctly during recovery.
*   Relational data
    *   Tables represent relations
    *   Columns (fields) represent attributes
    *   Rows are individual records
    *   Schema describes the structure of DB
*   SQL

    A query language to interact with and manage data.

    [CRUD operations](https://stackify.com/what-are-crud-operations/) - create, read, update, delete queries

    Management operations - create DBs/tables/indexes etc, backup, import/export, users, access controls

    Exercise: Classify the below queries into the four types - DDL (definition), DML(manipulation), DCL(control) and TCL(transactions) and explain in detail.

        insert, create, drop, delete, update, commit, rollback, truncate, alter, grant, revoke

    You can practise these in the [lab section](../lab.md).



*   Constraints

    Rules for data that can be stored. Query fails if you violate any of these defined on a table.


	Primary key: one or more columns that contain UNIQUE values, and cannot contain NULL values. A table can have only ONE primary key. An index on it is created by default.

    Foreign key: links two tables together. Its value(s) match a primary key in a different table \
	Not null: Does not allow null values \
	Unique: Value of column must be unique across all rows \
	Default: Provides a default value for a column if none is specified during insert

    Check: Allows only particular values (like Balance >= 0)



*   [Indexes](https://datageek.blog/en/2018/06/05/rdbms-basics-indexes-and-clustered-indexes/)

	Most indexes use B+ tree structure.

	Why use them: Speeds up queries (in large tables that fetch only a few rows, min/max queries, by eliminating rows from consideration etc)

	Types of indexes: unique, primary key, fulltext, secondary

	Write-heavy loads, mostly full table scans or accessing large number of rows etc. do not benefit from indexes



*   [Joins](https://www.sqlservertutorial.net/sql-server-basics/sql-server-joins/)

	Allows you to fetch related data from multiple tables, linking them together with some common field. Powerful but also resource-intensive and makes scaling databases difficult. This is the cause of many slow performing queries when run at scale, and the solution is almost always to find ways to reduce the joins.



*   [Access control](https://dev.mysql.com/doc/refman/8.0/en/access-control.html)

	DBs have privileged accounts for admin tasks, and regular accounts for clients. There are finegrained controls on what actions(DDL, DML etc. discussed earlier )are allowed for these accounts.

	DB first verifies the user credentials (authentication), and then examines whether this user is permitted to perform the request (authorization) by looking up these information in some internal tables.

	Other controls include activity auditing that allows examining the history of actions done by a user, and resource limits which define the number of queries, connections etc. allowed.


### Popular databases

Commercial, closed source - Oracle, Microsoft SQL Server, IBM DB2

Open source with optional paid support - MySQL, MariaDB, PostgreSQL

Individuals and small companies have always preferred open source DBs because of the huge cost associated with commercial software.

In recent times, even large organizations have moved away from commercial software to open source alternatives because of the flexibility and cost savings associated with it.

Lack of support is no longer a concern because of the paid support available from the developer and third parties.

MySQL is the most widely used open source DB, and it is widely supported by hosting providers, making it easy for anyone to use. It is part of the popular Linux-Apache-MySQL-PHP ([LAMP](https://en.wikipedia.org/wiki/LAMP_(software_bundle))) stack that became popular in the 2000s. We have many more choices for a programming language, but the rest of that stack is still widely used.