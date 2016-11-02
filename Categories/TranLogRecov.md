# Transactions, Logging, and Recovery

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 9592 and 9567 [High Availability/Distributed Technologies](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/HADRDist.md)


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 610 | Enables minimal-logging under certain scenarios. Connect item indicate TF 610 soon may be a query hint. | [StorEng](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2008/03/23/minimal-logging-changes-in-sql-server-2008-part-2.aspx); [Data Loading Perf Guide](http://technet.microsoft.com/en-us/library/dd425070(v=sql.100).aspx); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/557515/trace-flag-610-functionality-should-be-implemented-as-a-hint)| 
| 2537 | Enables fn_dblog() to read all of the t-log, not just the active portion of the log. Argenis Tweet: fn_dblog + 2537 is a way to check your t-log file for corruption. | [PRand](http://www.sqlskills.com/blogs/paul/finding-out-who-dropped-a-table-using-the-transaction-log/); [PRand](https://www.sqlskills.com/blogs/paul/using-fn_dblog-fn_dump_dblog-and-restoring-with-stopbeforemark-to-an-lsn/) | 
| 3412 | (Needs verification for whether still current) Reports when each transaction is rolled forward or back, except for large transactions | [170115](http://support.microsoft.com/kb/170115/en-us) | 
| 8599 | Allows the use of a save-point within a distributed transaction. | [295027](http://support.microsoft.com/kb/295027); [903742](http://support.microsoft.com/kb/903742/en-us) | 
| 9038 | CAdkin Tweet: "If you are using low-latency storage it appears that you can also cause high spins on LOGFLUSHQ in SQL 2016 with the in-memory engine and full durability. The multithreaded log writer is the cause of this and can be turned off via TF 9038." | | 
| 9909 | Turns off a logging optimization when an ALTER TABLE operation on a memory-optimized table would normally be done in parallel. This avoids the possibility of data loss b/c of a bug in parallel ALTER TABLE. | [3174963](https://support.microsoft.com/en-us/kb/3174963) | 




## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3924 | Enables a fix where "XA" transactions started within a JDBC-connected Java app are not closed properly and stay open indefinitely. | [3145492](https://support.microsoft.com/en-us/kb/3145492) | 
| 9024 | (On by default as of SQL 2014 SP1+) Enables a fix in SQL 2012/2014 that partitions (by memory node) a "pointer to a memory object" (PMO) to the log pool. The KB references this flag specifically in the context of Availability Groups, though the flag appears to deal specifically with the design of the "logpool". Note that 8048 is also potentially applicable here, as it will further partition the PMO to a by-CPU partitioning scheme. (See the KB). | [2809338](http://support.microsoft.com/kb/2809338/en-us) | 
