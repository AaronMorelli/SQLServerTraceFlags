# Background Sessions and Tasks

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 670 and 671 ([DBs and Files](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DBsFiles.md)), 
9837 ([Special Features](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/SpecialFeatures.md))

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 634 | **Doc2014** BOL: "Disables the background columnstore compression task. SQL Server periodically runs a background task that compresses columnstore index rowgroups with uncompressed data, one such rowgroup at a time. Columnstore compression improves query performance but also consumes system resources. You can control the timing of columnstore compression manually, by disabling the background compression task with trace flag 634, and then explicitly invoking ALTER INDEX REORGANIZE or ALTER INDEX REBUILD at the time of your choice. Scope: global only" | |
| 661 | **Doc2014** BOL: "Disables the ghost record removal process. Scope: global only" | [920093](https://support.microsoft.com/en-us/kb/920093); [PRand](http://www.sqlskills.com/blogs/paul/turning-off-the-ghost-cleanup-task-for-a-performance-gain/) and [here](http://www.sqlskills.com/blogs/paul/the-pros-and-cons-of-trace-flags/); [2590839](https://support.microsoft.com/en-us/kb/2590839) | 
| 662 | When used with 3605, logs detailed info about the ghost cleanup process to the SQL error log | [SQLJourney](https://blogs.msdn.microsoft.com/sqljourney/2012/07/27/an-in-depth-look-at-ghost-records-in-sql-server/) | 
| 669 | "...prevents user queries from queuing requests to the ghost cleanup process". This is a workaround for dumps occurring right after instance startup, where user queries (that queue pages for ghost cleanup) were running so quickly after startup that they were queuing pages before the ghost cleanup initialization | [3027860](https://support.microsoft.com/en-us/kb/3027860) | 
| 809 | Causes the Lazy Writer to be less aggressive. | [315447](https://support.microsoft.com/en-us/kb/315447) | 
| 828 | KB: "In SQL 2000 SP3 and later versions, checkpoint tries to use the recovery interval setting as a target for the length of time that checkpoint will take. Checkpoint adjusts to the I/O bandwidth to limit the time that checkpoint will take to the period of time that is reflected in the recovery interval setting. When you enable trace flag 828, you enable checkpoint to ignore the recovery interval target and to keep a steady I/O pace when checkpoint is executed." | [906121](https://support.microsoft.com/en-us/kb/906121); [CSS](https://blogs.msdn.microsoft.com/psssql/2008/04/11/how-it-works-sql-server-checkpoint-flushcache-outstanding-io-target/) | 
| 3502 | PRand: "writes to the error log when a checkpoint starts and finishes." Must be enabled globally. | [PRand](http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-1530-checkpoint-only-writes-pages-from-committed-transactions/); [815436](https://support.microsoft.com/en-us/kb/815436); [MSBlog](https://blogs.msdn.microsoft.com/joaol/2008/11/20/sql-server-checkpoint-problems/) | 
| 3504 | PRand: "writes to the error log information about what is written to disk” (during a checkpoint). These "FlushCache” messages can appear w/o the trace flag starting in SQL 2012. Referenced in BWard PASS 2014. Must be enabled globally. | [PRand](http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-1530-checkpoint-only-writes-pages-from-committed-transactions/); [MSBlog](https://blogs.msdn.microsoft.com/joaol/2008/11/20/sql-server-checkpoint-problems/); [CSS](https://blogs.msdn.microsoft.com/psssql/2012/06/01/how-it-works-when-is-the-flushcache-message-added-to-sql-server-error-log/) | 
| 3505 | KB: "Setting 3505 disables automatic checkpoints across the server for all DBs. After you set 3505, you must issue checkpoint commands [manually]. 3505 does not prevent the internal checkpoints that are issued by certain commands, such as BACKUP." | [815436](https://support.microsoft.com/en-us/kb/815436); [PRand](http://www.sqlskills.com/blogs/paul/benchmarking-1-tb-table-population-part-2-optimizing-log-block-io-size-and-how-log-io-works/) (comments) | 



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.
