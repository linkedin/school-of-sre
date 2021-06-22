*   Explain and explain+analyze

	EXPLAIN &lt;query> analyzes query plans from the optimizer, including how tables are joined, which tables/rows are scanned etc.

	Explain analyze shows the above and additional info like execution cost, number of rows returned, time taken etc.

	This knowledge is useful to tweak queries and add indexes.

	Watch this performance tuning [tutorial video](https://www.youtube.com/watch?v=pjRTLPeUOug).

	Checkout the [lab section](https://linkedin.github.io/school-of-sre/databases_sql/lab/) for a hands-on about indexes.

*   [Slow query logs](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)

	Used to identify slow queries (configurable threshold), enabled in config or dynamically with a query

	Checkout the [lab section](https://linkedin.github.io/school-of-sre/databases_sql/lab/) about identifying slow queries.

*   User management

	This includes creation and changes to users, like managing privileges, changing password etc.



*   Backup and restore strategies, pros and cons

	Logical backup using mysqldump - slower but can be done online

	Physical backup (copy data directory or use xtrabackup) -  quick backup/recovery. Copying data directory requires locking or shut down. xtrabackup is an improvement because it supports backups without shutting down (hot backup).

	Others - PITR, snapshots etc.


*   Crash recovery process using redo logs

	After a crash, when you restart server it reads redo logs and replays modifications to recover


*   Monitoring MySQL

	Key MySQL metrics: reads, writes, query runtime, errors, slow queries, connections, running threads, InnoDB metrics

	Key OS metrics: CPU, load, memory, disk I/O, network


*   Replication

    Copies data from one instance to one or more instances. Helps in horizontal scaling, data protection, analytics and performance. Binlog dump thread on primary, replication I/O and SQL threads on secondary. Strategies include the standard async, semi async or group replication.

*   High Availability

    Ability to cope with failure at software, hardware and network level. Essential for anyone who needs 99.9%+ uptime. Can be implemented with replication or clustering solutions from MySQL, Percona, Oracle etc. Requires expertise to setup and maintain. Failover can be manual, scripted or using tools like Orchestrator.

*   [Data directory](https://dev.mysql.com/doc/refman/8.0/en/data-directory.html)

    Data is stored in a particular directory, with nested directories for the data contained in each database. There are also MySQL log files, InnoDB log files, server process ID file and some other configs. The data directory is configurable.

*   [MySQL configuration](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html)

    This can be done by passing [parameters during startup](https://dev.mysql.com/doc/refman/5.7/en/server-options.html), or in a [file](https://dev.mysql.com/doc/refman/8.0/en/option-files.html). There are a few [standard paths](https://dev.mysql.com/doc/refman/8.0/en/option-files.html#option-file-order) where MySQL looks for config files, `/etc/my.cnf` is one of the commonly used paths. These options are organized under headers (mysqld for server and mysql for client), you can explore them more in the lab that follows.

*   [Logs](https://dev.mysql.com/doc/refman/5.7/en/server-logs.html)

    MySQL has logs for various purposes - general query log, errors, binary logs (for replication), slow query log. Only error log is enabled by default (to reduce I/O and storage requirement), the others can be enabled when required - by specifying config parameters at startup or running commands at runtime. [Log destination](https://dev.mysql.com/doc/refman/5.7/en/log-destinations.html) can also be tweaked with config parameters.
