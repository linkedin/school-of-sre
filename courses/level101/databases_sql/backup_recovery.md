### Backup and Recovery
Backups are a very crucial part of any database setup. They are generally a copy of the data that can be used to reconstruct the data in case of any major or minor crisis with the database. In general terms, backups can be of two types:

- **Physical Backup** - the data directory as it is on the disk
- **Logical Backup** - the table structure and records in it

Both the above kinds of backups are supported by MySQL with different tools. It is the job of the SRE to identify which should be used when.

#### Mysqldump
This utility is available with MySQL installation. It helps in getting the logical backup of the database. It outputs a set of SQL statements to reconstruct the data. It is not recommended to use `mysqldump` for large tables as it might take a lot of time and the file size will be huge. However, for small tables it is the best and the quickest option.

```shell
mysqldump [options] > dump_output.sql
```

There are certain options that can be used with `mysqldump` to get an appropriate dump of the database.

To dump all the databases:

```shell
mysqldump -u<user> -p<pwd> --all-databases > all_dbs.sql
```

To dump specific databases:

```shell
mysqldump -u<user> -p<pwd> --databases db1 db2 db3 > dbs.sql
```

To dump a single database:

```shell
mysqldump -u<user> -p<pwd> --databases db1 > db1.sql
```
OR
```shell
mysqldump -u<user> -p<pwd> db1 > db1.sql
```

The difference between the above two commands is that the latter one does not contain the `CREATE DATABASE` command in the backup output. 

To dump specific tables in a database:

```shell
mysqldump -u<user> -p<pwd> db1 table1 table2 > db1_tables.sql
```

To dump only table structures and no data:

```shell
mysqldump -u<user> -p<pwd> --no-data db1 > db1_structure.sql
```

To dump only table data and no `CREATE` statements:

```shell
mysqldump -u<user> -p<pwd> --no-create-info db1 > db1_data.sql
```

To dump only specific records from a table:

```shell
mysqldump -u<user> -p<pwd> --no-create-info db1 table1 --where=”salary>80000” > db1_table1_80000.sql
```

`mysqldump` can also provide output in CSV, other delimited text or XML format to support use-cases if any. The backup from `mysqldump` utility is offline, i.e. when the backup finishes it will not have the changes to the database which were made when the backup was going on. For example, if the backup started at 3:00 pm and finished at 4:00 pm, it will not have the changes made to the database between 3:00 and 4:00 pm.

**Restoring** from `mysqldump` can be done in the following two ways:

From shell

```shell
mysql -u<user> -p<pwd> < all_dbs.sql
```
OR

From shell, if the database is already created:

```shell
mysql -u<user> -p<pwd> db1 < db1.sql
```

From within MySQL shell:

```shell
mysql> source all_dbs.sql
```

#### Percona XtraBackup
This utility is installed separately from the MySQL server and is open source, provided by Percona. It helps in getting the full or partial physical backup of the database. It provides online backup of the database, i.e. it will have the changes made to the database when the backup was going on as explained at the end of the previous section.

- **Full Backup** - the complete backup of the database. 
- **Partial Backup** - Incremental 
  - **Cumulative** - After one full backup, the next backups will have changes post the full backup. For example, we took a full backup on Sunday, from Monday onwards every backup will have changes after Sunday; so, Tuesday’s backup will have Monday’s changes as well, Wednesday’s backup will have changes of Monday and Tuesday as well and so on.
  - **Differential** - After one full backup, the next backups will have changes post the previous incremental backup. For example, we took a full backup on Sunday, Monday will have changes done after Sunday, Tuesday will have changes done after Monday, and so on.

![partial backups - differential and cummulative](images/partial_backup.png "Differential and Cumulative Backups")

Percona XtraBackup allows us to get both full and incremental backups as we desire. However, incremental backups take less space than a full backup (if taken per day) but the restore time of incremental backups is more than that of full backups.

**Creating a full backup**

```shell
xtrabackup --defaults-file=<location to my.cnf> --user=<mysql user> --password=<mysql password> --backup --target-dir=<location of target directory>
```

Example:

```shell
xtrabackup --defaults-file=/etc/my.cnf --user=some_user --password=XXXX --backup --target-dir=/mnt/data/backup/
```

Some other options

- `--stream` - can be used to stream the backup files to standard output in a specified format. `xbstream` is the only option for now.
- `--tmp-dir` - set this to a `tmp` directory to be used for temporary files while taking backups.
- `--parallel` - set this to the number of threads that can be used to parallely copy data files to target directory.
- `--compress` - by default - `quicklz` is used. Set this to have the backup in compressed format. Each file is a `.qp` compressed file and can be extracted by `qpress` file archiver.
- `--decompress` - decompresses all the files which were compressed with the `.qp` extension. It will not delete the `.qp` files after decompression. To do that, use `--remove-original` along with this. Please note that the `decompress` option should be run separately from the `xtrabackup` command that used the compress option.

**Preparing a backup**

Once the backup is done with the `--backup` option, we need to prepare it in order to restore it. This is done to make the data files consistent with point-in-time. There might have been some transactions going on while the backup was being executed and those have changed the data files. When we prepare a backup, all those transactions are applied to the data files.

```shell
xtrabackup --prepare --target-dir=<where backup is taken>
```

Example:

```shell
xtrabackup --prepare --target-dir=/mnt/data/backup/
```

It is not recommended to halt a process which is preparing the backup as that might cause data file corruption and backup cannot be used further. The backup will have to be taken again.

**Restoring a Full Backup**

To restore the backup which is created and prepared from above commands, just copy everything from the backup `target-dir` to the `data-dir` of MySQL server, change the ownership of all files to MySQL user (the Linux user used by MySQL server) and start MySQL.

Or the below command can be used as well,

```shell
xtrabackup --defaults-file=/etc/my.cnf --copy-back --target-dir=/mnt/data/backups/
```

**Note** - the backup has to be prepared in order to restore it.

**Creating Incremental backups**

Percona XtraBackup helps create incremental backups, i.e, only the changes can be backed up since the last backup. Every InnoDB page contains a log sequence number or LSN that is also mentioned as one of the last lines of backup and prepare commands.

```shell
xtrabackup: Transaction log of lsn <LSN> to <LSN> was copied.
```
OR
```shell
InnoDB: Shutdown completed; log sequence number <LSN>
<timestamp> completed OK!
```

This indicates that the backup has been taken till the log sequence number mentioned. This is a key information in understanding incremental backups and working towards automating one. Incremental backups do not compare data files for changes, instead, they go through the InnoDB pages and compare their LSN to the last backup’s LSN. So, without one full backup, the incremental backups are useless.

The `xtrabackup` command creates a `xtrabackup_checkpoint` file which has the information about the LSN of the backup. Below are the key contents of the file:

```shell
backup_type = full-backuped | incremental
from_lsn = 0 (full backup) | to_lsn of last backup <LSN>
to_lsn = <LSN>
last_lsn = <LSN>
```
There is a difference between `to_lsn` and `last_lsn`. When the `last_lsn` is more than `to_lsn` that means there are transactions that ran while we took the backup and are yet to be applied. That is what `--prepare` is used for.

To take incremental backups, first, we require one full backup.

```shell
xtrabackup --defaults-file=/etc/my.cnf --user=some_user --password=XXXX --backup --target-dir=/mnt/data/backup/full/
```

Let’s assume the contents of the `xtrabackup_checkpoint` file to be as follows:

```shell
backup_type = full-backuped
from_lsn = 0
to_lsn = 1000
last_lsn = 1000
```
Now that we have one full backup, we can have an incremental backup that takes the changes. We will go with differential incremental backups.

```shell
xtrabackup --defaults-file=/etc/my.cnf --user=some_user --password=XXXX --backup --target-dir=/mnt/data/backup/incr1/ --incremental-basedir=/mnt/data/backup/full/
```

There are delta files created in the `incr1` directory like, `ibdata1.delta`, `db1/tbl1.ibd.delta` with the changes from the full directory. The `xtrabackup_checkpoint` file will thus have the following contents.

```shell
backup_type = incremental
from_lsn = 1000
to_lsn = 1500
last_lsn = 1500
```

Hence, the `from_lsn` here is equal to the `to_lsn` of the last backup or the `basedir` provided for the incremental backups. For the next incremental backup, we can use this incremental backup as the `basedir`.

```shell
xtrabackup --defaults-file=/etc/my.cnf --user=some_user --password=XXXX --backup --target-dir=/mnt/data/backup/incr2/ --incremental-basedir=/mnt/data/backup/incr1/
```

The `xtrabackup_checkpoint` file will thus have the following contents:

```shell
backup_type = incremental
from_lsn = 1500
to_lsn = 2000
last_lsn = 2200
```

**Preparing Incremental backups**

Preparing incremental backups is not the same as preparing a full backup. When prepare runs, two operations are performed - *committed transactions are applied on the data files* and *uncommitted transactions are rolled back*. While preparing incremental backups, we have to skip rollback of uncommitted transactions as it is likely that they might get committed in the next incremental backup. If we rollback uncommitted transactions, the further incremental backups cannot be applied.

We use `--apply-log-only` option along with `--prepare` to avoid the rollback phase. 

From the last section, we had the following directories with complete backup:

```shell
/mnt/data/backup/full
/mnt/data/backup/incr1
/mnt/data/backup/incr2
```

First, we prepare the full backup, but only with the `--apply-log-only` option.

```shell
xtrabackup --prepare --apply-log-only --target-dir=/mnt/data/backup/full
```

The output of the command will contain the following at the end.

```shell
InnoDB: Shutdown complete; log sequence number 1000
<timestamp> Completed OK!
```

Note the LSN mentioned at the end is the same as the `to_lsn` from the `xtrabackup_checkpoint` created for full backup.

Next, we apply the changes from the first incremental backup to the full backup.

```shell
xtrabackup --prepare --apply-log-only --target-dir=/mnt/data/backup/full --incremental-dir=/mnt/data/backup/incr1
```

This applies the delta files in the incremental directory to the full backup directory. It rolls the data files in the full backup directory forward to the time of incremental backup and applies the redo logs as usual.

Lastly, we apply the last incremental backup same as the previous one with just a small change.

```shell
xtrabackup --prepare --target-dir=/mnt/data/backup/full --incremental-dir=/mnt/data/backup/incr1
```

We do not have to use the `--apply-log-only` option with it. It applies the *incr2 delta files* to the full backup data files taking them forward, applies redo logs on them and finally rollbacks the uncommitted transactions to produce the final result. The data now present in the full backup directory can now be used to restore.

**Note**: To create cumulative incremental backups, the `incremental-basedir` should always be the full backup directory for every incremental backup. While preparing, we can start with the full backup with the `--apply-log-only` option and use just the last incremental backup for the final `--prepare` as that has all the changes since the full backup. 

**Restoring Incremental backups**

Once all the above steps are completed, restoring is the same as done for a full backup.

#### Further Reading

- [MySQL Point-In-Time-Recovery](https://dev.mysql.com/doc/refman/8.0/en/point-in-time-recovery.html)
- [Another MySQL backup tool - mysqlpump](https://dev.mysql.com/doc/refman/8.0/en/mysqlpump.html)
- [Another MySQL backup tool - mydumper](https://github.com/maxbube/mydumper/tree/master/docs)
- [A comparison between mysqldump, mysqlpump and mydumper](https://mydbops.wordpress.com/2019/03/26/mysqldump%E2%80%8B-vs-mysqlpump-vs-mydumper/)
- [Backup Best Practices](https://www.percona.com/blog/2020/05/27/best-practices-for-mysql-backups/)
