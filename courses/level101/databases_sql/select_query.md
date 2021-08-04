### SELECT Query
The most commonly used command while working with MySQL is SELECT. It is used to fetch the result set from one or more tables. 
The general form of a typical select query looks like:-
```
SELECT expr
FROM table1
[WHERE condition]
[GROUP BY column_list HAVING condition]
[ORDER BY column_list ASC|DESC]
[LIMIT #]
```
The above general form contains some commonly used clauses of a SELECT query:-

- **expr** - comma-separated column list or * (for all columns)
- **WHERE** - a condition is provided, if true, directs the query to select only those records.
- **GROUP BY** - groups the entire result set based on the column list provided. An aggregate function is recommended to be present in the select expression of the query. **HAVING** supports grouping by putting a condition on the selected or any other aggregate function.
- **ORDER BY** - sorts the result set based on the column list in ascending or descending order.
- **LIMIT** - commonly used to limit the number of records.

Let’s have a look at some examples for a better understanding of the above. The dataset used for the examples below is available [here](https://dev.mysql.com/doc/employee/en/employees-installation.html) and is free to use.

**Select all records**
```
mysql> select * from employees limit 5;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |
|  10005 | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.00 sec)
```
**Select specific fields for all records**
```
mysql> select first_name, last_name, gender from employees limit 5;
+------------+-----------+--------+
| first_name | last_name | gender |
+------------+-----------+--------+
| Georgi     | Facello   | M      |
| Bezalel    | Simmel    | F      |
| Parto      | Bamford   | M      |
| Chirstian  | Koblick   | M      |
| Kyoichi    | Maliniak  | M      |
+------------+-----------+--------+
5 rows in set (0.00 sec)
```
**Select all records Where hire_date >= January 1, 1990**
```
mysql> select * from employees where hire_date >= '1990-01-01' limit 5;
+--------+------------+------------+-------------+--------+------------+
| emp_no | birth_date | first_name | last_name   | gender | hire_date  |
+--------+------------+------------+-------------+--------+------------+
|  10008 | 1958-02-19 | Saniya     | Kalloufi    | M      | 1994-09-15 |
|  10011 | 1953-11-07 | Mary       | Sluis       | F      | 1990-01-22 |
|  10012 | 1960-10-04 | Patricio   | Bridgland   | M      | 1992-12-18 |
|  10016 | 1961-05-02 | Kazuhito   | Cappelletti | M      | 1995-01-27 |
|  10017 | 1958-07-06 | Cristinel  | Bouloucos   | F      | 1993-08-03 |
+--------+------------+------------+-------------+--------+------------+
5 rows in set (0.01 sec)
```
**Select first_name and last_name from all records Where birth_date >= 1960 AND gender = ‘F’**
```
mysql> select first_name, last_name from employees where year(birth_date) >= 1960 and gender='F' limit 5;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| Bezalel    | Simmel    |
| Duangkaew  | Piveteau  |
| Divier     | Reistad   |
| Jeong      | Reistad   |
| Mingsen    | Casley    |
+------------+-----------+
5 rows in set (0.00 sec)
```
**Display the total number of records**
```
mysql> select count(*) from employees;
+----------+
| count(*) |
+----------+
|   300024 |
+----------+
1 row in set (0.05 sec)
```
**Display gender-wise count of all records**
```
mysql> select gender, count(*) from employees group by gender;
+--------+----------+
| gender | count(*) |
+--------+----------+
| M      |   179973 |
| F      |   120051 |
+--------+----------+
2 rows in set (0.14 sec)
```
**Display the year of hire_date and number of employees hired that year, also only those years where more than 20k employees were hired**
```
mysql> select year(hire_date), count(*) from employees group by year(hire_date) having count(*) > 20000;
+-----------------+----------+
| year(hire_date) | count(*) |
+-----------------+----------+
|            1985 |    35316 |
|            1986 |    36150 |
|            1987 |    33501 |
|            1988 |    31436 |
|            1989 |    28394 |
|            1990 |    25610 |
|            1991 |    22568 |
|            1992 |    20402 |
+-----------------+----------+
8 rows in set (0.14 sec)
```
**Display all records ordered by their hire_date in descending order. If hire_date is the same then in order of their birth_date ascending order**
```
mysql> select * from employees order by hire_date desc, birth_date asc limit 5;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 463807 | 1964-06-12 | Bikash     | Covnot    | M      | 2000-01-28 |
| 428377 | 1957-05-09 | Yucai      | Gerlach   | M      | 2000-01-23 |
| 499553 | 1954-05-06 | Hideyuki   | Delgrande | F      | 2000-01-22 |
| 222965 | 1959-08-07 | Volkmar    | Perko     | F      | 2000-01-13 |
|  47291 | 1960-09-09 | Ulf        | Flexer    | M      | 2000-01-12 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.12 sec)
```
### SELECT - JOINS
JOIN statement is used to produce a combined result set from two or more tables based on certain conditions. It can be also used with Update and Delete statements but we will be focussing on the select query. 
Following is a basic general form for joins
```
SELECT table1.col1, table2.col1, ... (any combination)
FROM
table1 <join_type> table2
ON (or USING depends on join_type) table1.column_for_joining = table2.column_for_joining
WHERE …
```
Any number of columns can be selected, but it is recommended to select only those which are relevant to increase the readability of the resultset. All other clauses like where, group by are not mandatory.
Let’s discuss the types of JOINs supported by MySQL Syntax.

**Inner Join**

This joins table A with table B on a condition. Only the records where the condition is True are selected in the resultset.

Display some details of employees along with their salary
```
mysql> select e.emp_no,e.first_name,e.last_name,s.salary from employees e join salaries s on e.emp_no=s.emp_no limit 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```
Similar result can be achieved by
```
mysql> select e.emp_no,e.first_name,e.last_name,s.salary from employees e join salaries s using (emp_no) limit 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```
And also by
```
mysql> select e.emp_no,e.first_name,e.last_name,s.salary from employees e natural join salaries s limit 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```
**Outer Join**

Majorly of two types:-
- **LEFT** - joining complete table A with table B on a condition. All the records from table A are selected, but from table B, only those records are selected where the condition is True.
- **RIGHT** - Exact opposite of the left join.

Let us assume the below tables for understanding left join better.
```
mysql> select * from dummy1;
+----------+------------+
| same_col | diff_col_1 |
+----------+------------+
|        1 | A          |
|        2 | B          |
|        3 | C          |
+----------+------------+

mysql> select * from dummy2;
+----------+------------+
| same_col | diff_col_2 |
+----------+------------+
|        1 | X          |
|        3 | Y          |
+----------+------------+
```
A simple select join will look like the one below.
```
mysql> select * from dummy1 d1 left join dummy2 d2 on d1.same_col=d2.same_col;
+----------+------------+----------+------------+
| same_col | diff_col_1 | same_col | diff_col_2 |
+----------+------------+----------+------------+
|        1 | A          |        1 | X          |
|        3 | C          |        3 | Y          |
|        2 | B          |     NULL | NULL       |
+----------+------------+----------+------------+
3 rows in set (0.00 sec)
```
Which can also be written as
```
mysql> select * from dummy1 d1 left join dummy2 d2 using(same_col);
+----------+------------+------------+
| same_col | diff_col_1 | diff_col_2 |
+----------+------------+------------+
|        1 | A          | X          |
|        3 | C          | Y          |
|        2 | B          | NULL       |
+----------+------------+------------+
3 rows in set (0.00 sec)
```
And also as
```
mysql> select * from dummy1 d1 natural left join dummy2 d2;
+----------+------------+------------+
| same_col | diff_col_1 | diff_col_2 |
+----------+------------+------------+
|        1 | A          | X          |
|        3 | C          | Y          |
|        2 | B          | NULL       |
+----------+------------+------------+
3 rows in set (0.00 sec)
```
**Cross Join**

This does a cross product of table A and table B without any condition. It doesn’t have a lot of applications in the real world.

A Simple Cross Join looks like this
```
mysql> select * from dummy1 cross join dummy2;
+----------+------------+----------+------------+
| same_col | diff_col_1 | same_col | diff_col_2 |
+----------+------------+----------+------------+
|        1 | A          |        3 | Y          |
|        1 | A          |        1 | X          |
|        2 | B          |        3 | Y          |
|        2 | B          |        1 | X          |
|        3 | C          |        3 | Y          |
|        3 | C          |        1 | X          |
+----------+------------+----------+------------+
6 rows in set (0.01 sec)
```
One use case that can come in handy is when you have to fill in some missing entries. For example, all the entries from dummy1 must be inserted into a similar table dummy3, with each record must have 3 entries with statuses 1, 5 and 7.
```
mysql> desc dummy3;
+----------+----------+------+-----+---------+-------+
| Field    | Type     | Null | Key | Default | Extra |
+----------+----------+------+-----+---------+-------+
| same_col | int      | YES  |     | NULL    |       |
| value    | char(15) | YES  |     | NULL    |       |
| status   | smallint | YES  |     | NULL    |       |
+----------+----------+------+-----+---------+-------+
3 rows in set (0.02 sec)
```
Either you create an insert query script with as many entries as in dummy1 or use cross join to produce the required resultset.
```
mysql> select * from dummy1 
cross join 
(select 1 union select 5 union select 7) T2 
order by same_col;
+----------+------------+---+
| same_col | diff_col_1 | 1 |
+----------+------------+---+
|        1 | A          | 1 |
|        1 | A          | 5 |
|        1 | A          | 7 |
|        2 | B          | 1 |
|        2 | B          | 5 |
|        2 | B          | 7 |
|        3 | C          | 1 |
|        3 | C          | 5 |
|        3 | C          | 7 |
+----------+------------+---+
9 rows in set (0.00 sec)
```
The **T2** section in the above query is called a *sub-query*. We will discuss the same in the next section.

**Natural Join**

This implicitly selects the common column from table A and table B and performs an inner join.
```
mysql> select e.emp_no,e.first_name,e.last_name,s.salary from employees e natural join salaries s limit 5;
+--------+------------+-----------+--------+
| emp_no | first_name | last_name | salary |
+--------+------------+-----------+--------+
|  10001 | Georgi     | Facello   |  60117 |
|  10001 | Georgi     | Facello   |  62102 |
|  10001 | Georgi     | Facello   |  66074 |
|  10001 | Georgi     | Facello   |  66596 |
|  10001 | Georgi     | Facello   |  66961 |
+--------+------------+-----------+--------+
5 rows in set (0.00 sec)
```
Notice how natural join and using takes care that the common column is displayed only once if you are not explicitly selecting columns for the query.

Some More Examples

Display emp_no, salary, title and dept of the employees where salary > 80000
```
mysql> select e.emp_no, s.salary, t.title, d.dept_no 
from  
employees e 
join salaries s using (emp_no) 
join titles t using (emp_no) 
join dept_emp d using (emp_no) 
where s.salary > 80000 
limit 5;
+--------+--------+--------------+---------+
| emp_no | salary | title        | dept_no |
+--------+--------+--------------+---------+
|  10017 |  82163 | Senior Staff | d001    |
|  10017 |  86157 | Senior Staff | d001    |
|  10017 |  89619 | Senior Staff | d001    |
|  10017 |  91985 | Senior Staff | d001    |
|  10017 |  96122 | Senior Staff | d001    |
+--------+--------+--------------+---------+
5 rows in set (0.00 sec)
```
Display title-wise count of employees in each department order by dept_no
```
mysql> select d.dept_no, t.title, count(*) 
from titles t 
left join dept_emp d using (emp_no) 
group by d.dept_no, t.title 
order by d.dept_no 
limit 10;
+---------+--------------------+----------+
| dept_no | title              | count(*) |
+---------+--------------------+----------+
| d001    | Manager            |        2 |
| d001    | Senior Staff       |    13940 |
| d001    | Staff              |    16196 |
| d002    | Manager            |        2 |
| d002    | Senior Staff       |    12139 |
| d002    | Staff              |    13929 |
| d003    | Manager            |        2 |
| d003    | Senior Staff       |    12274 |
| d003    | Staff              |    14342 |
| d004    | Assistant Engineer |     6445 |
+---------+--------------------+----------+
10 rows in set (1.32 sec)
```
#### SELECT - Subquery
A subquery is generally a smaller resultset that can be used to power a select query in many ways. It can be used in a ‘where’ condition, can be used in place of join mostly where a join could be an overkill. 
These subqueries are also termed as derived tables. They must have a table alias in the select query.

Let’s look at some examples of subqueries.

Here we got the department name from the departments table by a subquery which used dept_no from dept_emp table.
```
mysql> select e.emp_no, 
(select dept_name from departments where dept_no=d.dept_no) dept_name from employees e 
join dept_emp d using (emp_no) 
limit 5;
+--------+-----------------+
| emp_no | dept_name       |
+--------+-----------------+
|  10001 | Development     |
|  10002 | Sales           |
|  10003 | Production      |
|  10004 | Production      |
|  10005 | Human Resources |
+--------+-----------------+
5 rows in set (0.01 sec)
```
Here, we used the ‘avg’ query above (which got the avg salary) as a subquery to list the employees whose latest salary is more than the average. 
```
mysql> select avg(salary) from salaries;
+-------------+
| avg(salary) |
+-------------+
|  63810.7448 |
+-------------+
1 row in set (0.80 sec)

mysql> select e.emp_no, max(s.salary) 
from employees e 
natural join salaries s 
group by e.emp_no 
having max(s.salary) > (select avg(salary) from salaries) 
limit 10;
+--------+---------------+
| emp_no | max(s.salary) |
+--------+---------------+
|  10001 |         88958 |
|  10002 |         72527 |
|  10004 |         74057 |
|  10005 |         94692 |
|  10007 |         88070 |
|  10009 |         94443 |
|  10010 |         80324 |
|  10013 |         68901 |
|  10016 |         77935 |
|  10017 |         99651 |
+--------+---------------+
10 rows in set (0.56 sec)
```