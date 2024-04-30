### SELECT Query
The most commonly used command while working with MySQL is `SELECT`. It is used to fetch the resultset from one or more tables. 
The general form of a typical select query looks like:

```
SELECT expr
FROM table1
[WHERE condition]
[GROUP BY column_list HAVING condition]
[ORDER BY column_list ASC|DESC]
[LIMIT #]
```

The above general form contains some commonly used clauses of a `SELECT` query:

- **expr** - comma-separated column list or * (for all columns)
- **WHERE** - a condition is provided, if true, directs the query to select only those records.
- **GROUP BY** - groups the entire resultset based on the column list provided. An aggregate function is recommended to be present in the select expression of the query. **HAVING** supports grouping by putting a condition on the selected or any other aggregate function.
- **ORDER BY** - sorts the resultset based on the column list in ascending or descending order.
- **LIMIT** - commonly used to limit the number of records.

Let’s have a look at some examples for a better understanding of the above. The dataset used for the examples below is available [here](https://dev.mysql.com/doc/employee/en/employees-installation.html) and is free to use.

**Select all records**

```shell
mysql> SELECT * FROM employees LIMIT 5;
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

```shell
mysql> SELECT first_name, last_name, gender FROM employees LIMIT 5;
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

```shell
mysql> SELECT * FROM employees WHERE hire_date >= '1990-01-01' LIMIT 5;
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

```shell
mysql> SELECT first_name, last_name FROM employees WHERE year(birth_date) >= 1960 AND gender='F' LIMIT 5;
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

```shell
mysql> SELECT COUNT(*) FROM employees;
+----------+
| COUNT(*) |
+----------+
|   300024 |
+----------+
1 row in set (0.05 sec)
```

**Display gender-wise count of all records**

```shell
mysql> SELECT gender, COUNT(*) FROM employees GROUP BY gender;
+--------+----------+
| gender | COUNT(*) |
+--------+----------+
| M      |   179973 |
| F      |   120051 |
+--------+----------+
2 rows in set (0.14 sec)
```

**Display the year of hire_date and number of employees hired that year, also only those years where more than 20k employees were hired**

```shell
mysql> SELECT year(hire_date), COUNT(*) FROM employees GROUP BY year(hire_date) HAVING COUNT(*) > 20000;
+-----------------+----------+
| year(hire_date) | COUNT(*) |
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

**Display all records ordered by their hire_date in descending order. If hire_date is the same, then in order of their birth_date ascending order**

```shell
mysql> SELECT * FROM employees ORDER BY hire_date DESC, birth_date ASC LIMIT 5;
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
`JOIN` statement is used to produce a combined resultset from two or more tables based on certain conditions. It can be also used with `UPDATE` and `DELETE` statements, but we will be focussing on the select query.

Following is a basic general form for joins:

```
SELECT table1.col1, table2.col1, ... (any combination)
FROM
table1 <join_type> table2
ON (or USING depends on join_type) table1.column_for_joining = table2.column_for_joining
WHERE …
```

Any number of columns can be selected, but it is recommended to select only those which are relevant to increase the readability of the resultset. All other clauses like `WHERE`, `GROUP BY` are not mandatory.
Let’s discuss the types of JOINs supported by MySQL Syntax.

**Inner Join**

This joins table A with table B on a condition. Only the records where the condition is True are selected in the resultset.

Display some details of employees along with their salary:

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e JOIN salaries s ON e.emp_no=s.emp_no LIMIT 5;
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

Similar result can be achieved by:

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e JOIN salaries s USING (emp_no) LIMIT 5;
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

And also by:

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e NATURAL JOIN salaries s LIMIT 5;
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

Majorly of two types:

- **LEFT** - joining complete table A with table B on a condition. All the records from table A are selected, but from table B, only those records are selected where the condition is True.
- **RIGHT** - Exact opposite of the `LEFT JOIN`.

Let us assume the below tables for understanding `LEFT JOIN` better.

```shell
mysql> SELECT * FROM dummy1;
+----------+------------+
| same_col | diff_col_1 |
+----------+------------+
|        1 | A          |
|        2 | B          |
|        3 | C          |
+----------+------------+

mysql> SELECT * FROM dummy2;
+----------+------------+
| same_col | diff_col_2 |
+----------+------------+
|        1 | X          |
|        3 | Y          |
+----------+------------+
```

A simple `SELECT JOIN` will look like the one below:

```shell
mysql> SELECT * FROM dummy1 d1 LEFT JOIN dummy2 d2 ON d1.same_col=d2.same_col;
+----------+------------+----------+------------+
| same_col | diff_col_1 | same_col | diff_col_2 |
+----------+------------+----------+------------+
|        1 | A          |        1 | X          |
|        3 | C          |        3 | Y          |
|        2 | B          |     NULL | NULL       |
+----------+------------+----------+------------+
3 rows in set (0.00 sec)
```

Which can also be written as:

```shell
mysql> SELECT * FROM dummy1 d1 LEFT JOIN dummy2 d2 USING(same_col);
+----------+------------+------------+
| same_col | diff_col_1 | diff_col_2 |
+----------+------------+------------+
|        1 | A          | X          |
|        3 | C          | Y          |
|        2 | B          | NULL       |
+----------+------------+------------+
3 rows in set (0.00 sec)
```

And also as:

```shell
mysql> SELECT * FROM dummy1 d1 NATURAL LEFT JOIN dummy2 d2;
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

A Simple `CROSS JOIN` looks like this:

```shell
mysql> SELECT * FROM dummy1 CROSS JOIN dummy2;
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

One use case that can come in handy is when you have to fill in some missing entries. For example, all the entries from `dummy1` must be inserted into a similar table `dummy3`, with each record must have 3 entries with statuses 1, 5 and 7.

```shell
mysql> DESC dummy3;
+----------+----------+------+-----+---------+-------+
| Field    | Type     | Null | Key | Default | Extra |
+----------+----------+------+-----+---------+-------+
| same_col | int      | YES  |     | NULL    |       |
| value    | char(15) | YES  |     | NULL    |       |
| status   | smallint | YES  |     | NULL    |       |
+----------+----------+------+-----+---------+-------+
3 rows in set (0.02 sec)
```

Either you create an `INSERT` query script with as many entries as in `dummy1` or use `CROSS JOIN` to produce the required resultset.

```shell
mysql> SELECT * FROM dummy1 
CROSS JOIN 
(SELECT 1 UNION SELECT 5 UNION SELECT 7) T2 
ORDER BY same_col;
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

```shell
mysql> SELECT e.emp_no,e.first_name,e.last_name,s.salary FROM employees e NATURAL JOIN salaries s LIMIT 5;
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

Notice how `NATURAL JOIN` and using takes care that the common column is displayed only once if you are not explicitly selecting columns for the query.

**Some More Examples**

Display `emp_no`, `salary`, `title` and `dept` of the employees where salary > 80000.

```shell
mysql> SELECT e.emp_no, s.salary, t.title, d.dept_no 
FROM  
employees e 
JOIN salaries s USING (emp_no) 
JOIN titles t USING (emp_no) 
JOIN dept_emp d USING (emp_no) 
WHERE s.salary > 80000 
LIMIT 5;
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

Display title-wise count of employees in each department ordered by `dept_no`:

```shell
mysql> SELECT d.dept_no, t.title, COUNT(*) 
FROM titles t 
LEFT JOIN dept_emp d USING (emp_no) 
GROUP BY d.dept_no, t.title 
ORDER BY d.dept_no 
LIMIT 10;
+---------+--------------------+----------+
| dept_no | title              | COUNT(*) |
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
A subquery is generally a smaller resultset that can be used to power a `SELECT` query in many ways. It can be used in a `WHERE` condition, can be used in place of `JOIN` mostly where a `JOIN` could be an overkill. 
These subqueries are also termed as derived tables. They must have a table alias in the `SELECT` query.

Let’s look at some examples of subqueries.

Here, we got the department name from the `departments` table by a subquery which used `dept_no` from `dept_emp` table.

```shell
mysql> SELECT e.emp_no, 
(SELECT dept_name FROM departments WHERE dept_no=d.dept_no) dept_name FROM employees e 
JOIN dept_emp d USING (emp_no) 
LIMIT 5;
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

Here, we used the `AVG` query above (which got the avg salary) as a subquery to list the employees whose latest salary is more than the average. 

```shell
mysql> SELECT AVG(salary) FROM salaries;
+-------------+
| AVG(salary) |
+-------------+
|  63810.7448 |
+-------------+
1 row in set (0.80 sec)

mysql> SELECT e.emp_no, MAX(s.salary) 
FROM employees e 
NATURAL JOIN salaries s 
GROUP BY e.emp_no 
HAVING MAX(s.salary) > (SELECT AVG(salary) FROM salaries) 
LIMIT 10;
+--------+---------------+
| emp_no | MAX(s.salary) |
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