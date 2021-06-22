**Prerequisites**

Install Docker


**Setup**

Create a working directory named sos or something similar, and cd into it.

Enter the following into a file named my.cnf under a directory named custom.


```
sos $ cat custom/my.cnf
[mysqld]
# These settings apply to MySQL server
# You can set port, socket path, buffer size etc.
# Below, we are configuring slow query settings
slow_query_log=1
slow_query_log_file=/var/log/mysqlslow.log
long_query_time=1
```


Start a container and enable slow query log with the following:


```
sos $ docker run --name db -v custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=realsecret -d mysql:8
sos $ docker cp custom/my.cnf $(docker ps -qf "name=db"):/etc/mysql/conf.d/custom.cnf
sos $ docker restart $(docker ps -qf "name=db")
```


Import a sample database


```
sos $ git clone git@github.com:datacharmer/test_db.git
sos $ docker cp test_db $(docker ps -qf "name=db"):/home/test_db/
sos $ docker exec -it $(docker ps -qf "name=db") bash
root@3ab5b18b0c7d:/# cd /home/test_db/
root@3ab5b18b0c7d:/# mysql -uroot -prealsecret mysql < employees.sql
root@3ab5b18b0c7d:/etc# touch /var/log/mysqlslow.log
root@3ab5b18b0c7d:/etc# chown mysql:mysql /var/log/mysqlslow.log
```


_Workshop 1: Run some sample queries_
Run the following
```
$ mysql -uroot -prealsecret mysql
mysql>

# inspect DBs and tables
# the last 4 are MySQL internal DBs

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| employees          |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

> use employees;
mysql> show tables;
+----------------------+
| Tables_in_employees  |
+----------------------+
| current_dept_emp     |
| departments          |
| dept_emp             |
| dept_emp_latest_date |
| dept_manager         |
| employees            |
| salaries             |
| titles               |
+----------------------+

# read a few rows
mysql> select * from employees limit 5;

# filter data by conditions
mysql> select count(*) from employees where gender = 'M' limit 5;

# find count of particular data
mysql> select count(*) from employees where first_name = 'Sachin'; 
```

_Workshop 2: Use explain and explain analyze to profile a query, identify and add indexes required for improving performance_
```
# View all indexes on table 
#(\G is to output horizontally, replace it with a ; to get table output)
mysql> show index from employees from employees\G
*************************** 1. row ***************************
        Table: employees
   Non_unique: 0
     Key_name: PRIMARY
 Seq_in_index: 1
  Column_name: emp_no
    Collation: A
  Cardinality: 299113
     Sub_part: NULL
       Packed: NULL
         Null:
   Index_type: BTREE
      Comment:
Index_comment:
      Visible: YES
   Expression: NULL

# This query uses an index, idenitfied by 'key' field
# By prefixing explain keyword to the command, 
# we get query plan (including key used)
mysql> explain select * from employees where emp_no < 10005\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: employees
   partitions: NULL
         type: range
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: NULL
         rows: 4
     filtered: 100.00
        Extra: Using where

# Compare that to the next query which does not utilize any index
mysql> explain select first_name, last_name from employees where first_name = 'Sachin'\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: employees
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 299113
     filtered: 10.00
        Extra: Using where

# Let's see how much time this query takes
mysql> explain analyze select first_name, last_name from employees where first_name = 'Sachin'\G
*************************** 1. row ***************************
EXPLAIN: -> Filter: (employees.first_name = 'Sachin')  (cost=30143.55 rows=29911) (actual time=28.284..3952.428 rows=232 loops=1)
    -> Table scan on employees  (cost=30143.55 rows=299113) (actual time=0.095..1996.092 rows=300024 loops=1)


# Cost(estimated by query planner) is 30143.55
# actual time=28.284ms for first row, 3952.428 for all rows
# Now lets try adding an index and running the query again
mysql> create index idx_firstname on employees(first_name);
Query OK, 0 rows affected (1.25 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain analyze select first_name, last_name from employees where first_name = 'Sachin';
+--------------------------------------------------------------------------------------------------------------------------------------------+
| EXPLAIN                                                                                                                                    |
+--------------------------------------------------------------------------------------------------------------------------------------------+
| -> Index lookup on employees using idx_firstname (first_name='Sachin')  (cost=81.20 rows=232) (actual time=0.551..2.934 rows=232 loops=1)
 |
+--------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)

# Actual time=0.551ms for first row
# 2.934ms for all rows. A huge improvement!
# Also notice that the query involves only an index lookup,
# and no table scan (reading all rows of table)
# ..which vastly reduces load on the DB.
```

_Workshop 3: Identify slow queries on a MySQL server_
```
# Run the command below in two terminal tabs to open two shells into the container.
docker exec -it $(docker ps -qf "name=db") bash

# Open a mysql prompt in one of them and execute this command
# We have configured to log queries that take longer than 1s,
# so this sleep(3) will be logged
mysql -uroot -prealsecret mysql
mysql> select sleep(3);

# Now, in the other terminal, tail the slow log to find details about the query
root@62c92c89234d:/etc# tail -f /var/log/mysqlslow.log
/usr/sbin/mysqld, Version: 8.0.21 (MySQL Community Server - GPL). started with:
Tcp port: 3306  Unix socket: /var/run/mysqld/mysqld.sock
Time                 Id Command    Argument
# Time: 2020-11-26T14:53:44.822348Z
# User@Host: root[root] @ localhost []  Id:     9
# Query_time: 5.404938  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 1
use employees;
# Time: 2020-11-26T14:53:58.015736Z
# User@Host: root[root] @ localhost []  Id:     9
# Query_time: 10.000225  Lock_time: 0.000000 Rows_sent: 1  Rows_examined: 1
SET timestamp=1606402428;
select sleep(3);
```

These were simulated examples with minimal complexity. In real life, the queries would be much more complex and the explain/analyze and slow query logs would have more details.
