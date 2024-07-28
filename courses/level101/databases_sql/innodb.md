### Why should you use this?

General purpose, row level locking, ACID support, transactions, crash recovery and multi-version concurrency control, etc.


### Architecture

![alt_text](images/innodb_architecture.png "InnoDB components")


### Key components:

*   Memory:
    *   Buffer pool: LRU cache of frequently used data (table and index) to be processed directly from memory, which speeds up processing. Important for tuning performance.
    *   Change buffer: Caches changes to secondary index pages when those pages are not in the buffer pool and merges it when they are fetched. Merging may take a long time and impact live queries. It also takes up part of the buffer pool. Avoids the extra I/O to read secondary indexes in.
    *   Adaptive hash index: Supplements InnoDB’s B-Tree indexes with fast hash lookup tables like a cache. Slight performance penalty for misses, also adds maintenance overhead of updating it. Hash collisions cause AHI rebuilding for large DBs.
    *   Log buffer: Holds log data before flush to disk.

        Size of each above memory is configurable, and impacts performance a lot. Requires careful analysis of workload, available resources, benchmarking and tuning for optimal performance.

*   Disk:
    *   Tables: Stores data within rows and columns.
    *   Indexes: Helps find rows with specific column values quickly, avoids full table scans.
    *   Redo Logs: all transactions are written to them, and after a crash, the recovery process corrects data written by incomplete transactions and replays any pending ones.
    *   Undo Logs: Records associated with a single transaction that contains information about how to undo the latest change by a transaction.

