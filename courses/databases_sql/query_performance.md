### Query Performance Improvement
Query Performance is a very crucial aspect of relational databases. If not tuned correctly, the select queries can become slow and painful for the application, and for the MySQL server as well. The important task is to identify the slow queries and try to improve their performance by either rewriting them or creating proper indexes on the tables involved in it.

#### The Slow Query Log
The slow query log contains SQL statements that take a longer time to execute then set in the config parameter long_query_time. These queries are the candidates for optimization. There are some good utilities to summarize the slow query logs like, mysqldumpslow (provided by MySQL itself), pt-query-digest (provided by Percona), etc. Following are the config parameters that are used to enable and effectively catch slow queries

| Variable | Explanation | Example value |
| --- | --- | --- |
| slow_query_log | Enables or disables slow query logs | ON |
| slow_query_log_file | The location of the slow query log | /var/lib/mysql/mysql-slow.log |
| long_query_time | Threshold time. The query that takes longer than this time is logged in slow query log | 5 |
| log_queries_not_using_indexes | When enabled with the slow query log, the queries which do not make use of any index are also logged in the slow query log even though they take less time than long_query_time. | ON |

So, for this section, we will be enabling **slow_query_log**, **long_query_time** will be kept to **0.3 (300 ms)**, and **log_queries_not_using** index will be enabled as well.

Below are the queries that we will execute on the employees database.

1. select * from employees where last_name = 'Koblick';
2. select * from salaries where salary >= 100000;
3. select * from titles where title = 'Manager';
4. select * from employees where year(hire_date) = 1995;
5. select year(e.hire_date), max(s.salary) from employees e join salaries s on e.emp_no=s.emp_no group by year(e.hire_date);

Now, queries **1**, **3** and **4** executed under 300 ms but if we check the slow query logs, we will find these queries logged as they are not using any of the index. Queries **2** and **5** are taking longer than 300ms and also not using any index.

Use the following command to get the summary of the slow query log

`mysqldumpslow /var/lib/mysql/mysql-slow.log`

![slow query log analysis](images/mysqldumpslow_out.png "slow query log analysis")

There are some more queries in the snapshot that were along with the queries mentioned. Mysqldumpslow replaces actual values that were used by N (in case of numbers) and S (in case of strings). That can be overridden by `-a` option, however that will increase the output lines if different values are used in similar queries.

#### The EXPLAIN Plan
The **EXPLAIN** command is used with any query that we want to analyze. It describes the query execution plan, how MySQL sees and executes the query. EXPLAIN works with Select, Insert, Update and Delete statements. It tells about different aspects of the query like, how tables are joined, indexes used or not, etc. The important thing here is to understand the basic Explain plan output of a query to determine its performance. 

Let's take the following query as an example,
```
mysql> explain select * from salaries where salary = 100000;
+----+-------------+----------+------------+------+---------------+------+---------+------+---------+----------+-------------+
| id | select_type | table    | partitions | type | possible_keys | key  | key_len | ref  | rows    | filtered | Extra       |
+----+-------------+----------+------------+------+---------------+------+---------+------+---------+----------+-------------+
|  1 | SIMPLE      | salaries | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 2838426 |    10.00 | Using where |
+----+-------------+----------+------------+------+---------------+------+---------+------+---------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
```
The key aspects to understand in the above output are:-

- **Partitions** - the number of partitions considered while executing the query. It is only valid if the table is partitioned.
- **Possible_keys** - the list of indexes that were considered during creation of the execution plan.
- **Key** - the index that will be used while executing the query.
- **Rows** - the number of rows examined during the execution.
- **Filtered** - the percentage of rows that were filtered out of the rows examined. The maximum and most optimized result will have 100 in this field. 
- **Extra** - this tells some extra information on how MySQL evaluates,  whether the query is using only where clause to match target rows, any index or temporary table, etc.

So, for the above query, we can determine that there are no partitions, there are no candidate indexes to be used and so no index is used at all, over 2M rows are examined and only 10% of them are included in the result, and lastly, only a where clause is used to match the target rows.

#### Creating an Index
Indexes are used to speed up selecting relevant rows for a given column value. Without an index, MySQL starts with the first row and goes through the entire table to find matching rows. If the table has too many rows, the operation becomes costly. With indexes, MySQL determines the position to start looking for the data without reading the full table.

A primary key is also an index which is also the fastest and is stored along with the table data. Secondary indexes are stored outside of the table data and are used to further enhance the performance of SQL statements. Indexes are mostly stored as B-Trees, with some exceptions like spatial indexes use R-Trees and memory tables use hash indexes.

There are 2 ways to create indexes:-

- While creating a table - if we know beforehand the columns that will drive the most number of where clauses in select queries, then we can put an index over them while creating a table.
- Altering a Table - To improve the performance of a troubling query, we create an index on a table which already has data in it using ALTER or CREATE INDEX command. This operation does not block the table but might take some time to complete depending on the size of the table.

Let’s look at the query that we discussed in the previous section. It’s clear that scanning over 2M records is not a good idea when only 10% of those records are actually in the resultset. 

Hence, we create an index on the salary column of the salaries table.

`create index idx_salary on salaries(salary)`

OR

`alter table salaries add index idx_salary(salary)`

And the same explain plan now looks like this
```
mysql> explain select * from salaries where salary = 100000;
+----+-------------+----------+------------+------+---------------+------------+---------+-------+------+----------+-------+
| id | select_type | table    | partitions | type | possible_keys | key        | key_len | ref   | rows | filtered | Extra |
+----+-------------+----------+------------+------+---------------+------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | salaries | NULL       | ref  | idx_salary    | idx_salary | 4       | const |   13 |   100.00 | NULL  |
+----+-------------+----------+------------+------+---------------+------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```
Now the index used is idx_salary, the one we recently created. The index actually helped examine only 13 records and all of them are in the resultset. Also, the query execution time is also reduced from over 700ms to almost negligible. 

Let’s look at another example. Here we are searching for a specific combination of first\_name and last\_name. But, we might also search based on last_name only.
```
mysql> explain select * from employees where last_name = 'Dredge' and first_name = 'Yinghua';
+----+-------------+-----------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table     | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+-----------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | employees | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 299468 |     1.00 | Using where |
+----+-------------+-----------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
```
Now only 1% record out of almost 300K is the resultset. Although the query time is particularly quick as we have only 300K records, this will be a pain if the number of records are over millions. In this case, we create an index on last\_name and first\_name, not separately, but a composite index including both the columns. 

`create index idx_last_first on employees(last_name, first_name)`
```
mysql> explain select * from employees where last_name = 'Dredge' and first_name = 'Yinghua';
+----+-------------+-----------+------------+------+----------------+----------------+---------+-------------+------+----------+-------+
| id | select_type | table     | partitions | type | possible_keys  | key            | key_len | ref         | rows | filtered | Extra |
+----+-------------+-----------+------------+------+----------------+----------------+---------+-------------+------+----------+-------+
|  1 | SIMPLE      | employees | NULL       | ref  | idx_last_first | idx_last_first | 124     | const,const |    1 |   100.00 | NULL  |
+----+-------------+-----------+------------+------+----------------+----------------+---------+-------------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```
We chose to put last\_name before first\_name while creating the index as the optimizer starts from the leftmost prefix of the index while evaluating the query. For example, if we have a 3-column index like idx(c1, c2, c3), then the search capability of the index follows - (c1), (c1, c2) or (c1, c2, c3) i.e. if your where clause has only first_name this index won’t work. 
```
mysql> explain select * from employees where first_name = 'Yinghua';
+----+-------------+-----------+------------+------+---------------+------+---------+------+--------+----------+-------------+
| id | select_type | table     | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra       |
+----+-------------+-----------+------------+------+---------------+------+---------+------+--------+----------+-------------+
|  1 | SIMPLE      | employees | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 299468 |    10.00 | Using where |
+----+-------------+-----------+------------+------+---------------+------+---------+------+--------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
```
But, if you have only the last_name in the where clause, it will work as expected.
```
mysql> explain select * from employees where last_name = 'Dredge';
+----+-------------+-----------+------------+------+----------------+----------------+---------+-------+------+----------+-------+
| id | select_type | table     | partitions | type | possible_keys  | key            | key_len | ref   | rows | filtered | Extra |
+----+-------------+-----------+------------+------+----------------+----------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | employees | NULL       | ref  | idx_last_first | idx_last_first | 66      | const |  200 |   100.00 | NULL  |
+----+-------------+-----------+------------+------+----------------+----------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```
For another example, use the following queries:-
```
create table employees_2 like employees;
create table salaries_2 like salaries;
alter table salaries_2 drop primary key;
```
We made copies of employees and salaries tables without the Primary Key of salaries table to understand an example of Select with Join.

When you have queries like the below, it becomes tricky to identify the pain point of the query.
```
mysql> select e.first_name, e.last_name, s.salary, e.hire_date from employees_2 e join salaries_2 s on e.emp_no=s.emp_no where e.last_name='Dredge';
1860 rows in set (4.44 sec)
```
This query is taking about 4.5 seconds to complete with 1860 rows in the resultset. Let’s look at the Explain plan. There will be 2 records in the Explain plan as 2 tables are used in the query.
```
mysql> explain select e.first_name, e.last_name, s.salary, e.hire_date from employees_2 e join salaries_2 s on e.emp_no=s.emp_no where e.last_name='Dredge';
+----+-------------+-------+------------+--------+------------------------+---------+---------+--------------------+---------+----------+-------------+
| id | select_type | table | partitions | type   | possible_keys          | key     | key_len | ref                | rows    | filtered | Extra       |
+----+-------------+-------+------------+--------+------------------------+---------+---------+--------------------+---------+----------+-------------+
|  1 | SIMPLE      | s     | NULL       | ALL    | NULL                   | NULL    | NULL    | NULL               | 2837194 |   100.00 | NULL        |
|  1 | SIMPLE      | e     | NULL       | eq_ref | PRIMARY,idx_last_first | PRIMARY | 4       | employees.s.emp_no |       1 |     5.00 | Using where |
+----+-------------+-------+------------+--------+------------------------+---------+---------+--------------------+---------+----------+-------------+
2 rows in set, 1 warning (0.00 sec)
```
These are in order of evaluation i.e. salaries\_2 will be evaluated first and then employees\_2 will be joined to it. As it looks like, it scans almost all the rows of salaries\_2 table and tries to match the employees\_2 rows as per the join condition. Though where clause is used in fetching the final resultset, but the index corresponding to the where clause is not used for the employees\_2 table. 

If the join is done on two indexes which have the same data-types, it will always be faster. So, let’s create an index on the *emp_no* column of salaries_2 table and analyze the query again.

`create index idx_empno on salaries_2(emp_no);`
```
mysql> explain select e.first_name, e.last_name, s.salary, e.hire_date from employees_2 e join salaries_2 s on e.emp_no=s.emp_no where e.last_name='Dredge';
+----+-------------+-------+------------+------+------------------------+----------------+---------+--------------------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys          | key            | key_len | ref                | rows | filtered | Extra |
+----+-------------+-------+------------+------+------------------------+----------------+---------+--------------------+------+----------+-------+
|  1 | SIMPLE      | e     | NULL       | ref  | PRIMARY,idx_last_first | idx_last_first | 66      | const              |  200 |   100.00 | NULL  |
|  1 | SIMPLE      | s     | NULL       | ref  | idx_empno              | idx_empno      | 4       | employees.e.emp_no |    9 |   100.00 | NULL  |
+----+-------------+-------+------------+------+------------------------+----------------+---------+--------------------+------+----------+-------+
2 rows in set, 1 warning (0.00 sec)
```
Now, not only did the index help the optimizer to examine only a few rows in both tables, it reversed the order of the tables in evaluation. The employees\_2 table is evaluated first and rows are selected as per the index respective to the where clause. Then the records are joined to salaries\_2 table as per the index used due to the join condition. The execution time of the query came down **from 4.5s to 0.02s**.
```
mysql> select e.first_name, e.last_name, s.salary, e.hire_date from employees_2 e join salaries_2 s on e.emp_no=s.emp_no where e.last_name='Dredge'\G
1860 rows in set (0.02 sec)
```