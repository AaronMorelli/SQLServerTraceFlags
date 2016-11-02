# Locking and Waiting

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 611 | Aaron: **Info** (Confirmed still in SQL 2014). Records each lock escalation, in the form: "Escalated locks - Reason: LOCK_THRESHOLD, Mode: S, Granularity: TABLE, Table: 222623836, HoBt: 150:256, HoBt Lock Count: 6248, Escalated Lock Count: 6249, Line Number: 1, Start Offset: 0, SQL Statement: select count(*) from dbo.BigTable" | [SSWUG](http://www.sswug.org/articles/viewarticle.aspx?id=undocumented_sql_server_2005_trace_flags-27595) | 
| 1200 | Prints detailed lock info as every request for a lock is made (the process ID and type of lock requested). | [StorEng](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2008/03/30/sql-server-table-variable-vs-local-temporary-table.aspx) (comments); [169960](http://support.microsoft.com/kb/169960/en-us) | 
| 1204 | **Doc2005** **Info** BOL: "Returns the resources and types of locks participating in a deadlock and also the current command affected. Scope: global only". *KB937950 notes a specific type of error where 1204 will not help with deadlocks*  | [937950](http://support.microsoft.com/kb/937950/en-us) | 
| 1205 | Prints information to the SQL error log every time a deadlock search starts, whether or not a deadlock is found. | [832524](http://support.microsoft.com/kb/832524) | 
| 1206 | Used to complement flag 1204 by displaying other locks held by deadlock parties. | [169960](http://support.microsoft.com/kb/169960/en-us) | 
| 1208 | KB: "Prints the host name and program name supplied by the client. This can help identify a client involved in a deadlock, assuming the client specifies a unique value for each connection." | [169960](http://support.microsoft.com/kb/169960/en-us) | 
| 1211 | (more below) Disables lock escalation operations that are due to memory-pressure-related conditions. | | 
| 1222 | **Doc2005** **Info** BOL: "Returns the resources and types of locks that are participating in a deadlock and also the current command affected, in an XML format that does not comply with any XSD schema. Scope: global only" | | 
| 1224 | (more below) BOL: "Disables lock escalation based on number of locks." | | 
| 1229 | Disables lock partitioning (among schedulers) | [CSS](http://blogs.msdn.com/b/psssql/archive/2012/08/31/strange-sch-s-sch-m-deadlock-on-machines-with-16-or-more-schedulers.aspx) | 
| 8001 | (more below) Increases number of wait types shown in sys.dm_os_wait_stats | | 
| 8050 | Causes "optional" wait types (see the CSS article) to be excluded when querying sys.dm_os_wait_stats. | [CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/03/the-sql-server-wait-type-repository.aspx) | 
| 8755 | Disables any locking hints and permits the optimizer/query execution engine to select the appropriate lock behavior. | [SQLMag](http://sqlmag.com/sql-server/investigating-trace-flags) |


**1211** **Doc2005** BOL: "Disables lock escalation based on memory pressure, or based on number of locks. The SQL Server Database Engine will not escalate row or page 
locks to table locks. Using this trace flag can generate excessive numbers of locks. This can slow the performance of the Database Engine, or cause 1204 errors 
(unable to allocate lock resource) because of insufficient memory. If both trace flag 1211 and 1224 are set, 1211 takes precedence over 1224. However, because trace flag 
1211 prevents escalation in every case, even under memory pressure, we recommend that you use 1224. This helps avoid "out-of-locks" errors when many locks are being used. 
Scope: global or session."	[PRand](http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-2330-lock-escalation/)


**1224** **Doc2005** BOL: "Disables lock escalation based on the number of locks. However, memory pressure can still activate lock escalation. 
The Database Engine escalates row or page locks to table (or partition) locks if the amount of memory used by lock objects exceeds one of the following conditions:
-	Forty percent of the memory that is used by Database Engine. This is applicable only when the locks parameter of sp_configure is set to 0.
-	Forty percent of the lock memory that is configured by using the locks parameter of sp_configure. For more information, see [Server Configuration Options (SQL Server)](http://technet.microsoft.com/en-us/library/ms189631.aspx).
If both trace flag 1211 and 1224 are set, 1211 takes precedence over 1224. However, because trace flag 1211 prevents escalation in every case, even under memory pressure, 
we recommend that you use 1224. This helps avoid "out-of- locks" errors when many locks are being used."
Lock escalation to the table- or HoBT-level granularity can also be controlled by using the LOCK_ESCALATION option of the ALTER TABLE statement. Scope: global or session
[PRand](http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-2330-lock-escalation/)


**8001** **Info** Khen2005, p2: "For SQL Server 2005, the SQL Server product team opted not to include [in sys.dm_os_wait_stats] some wait types that fall under one of 
the following three categories:
-	Wait types that are never used in SQL Server 2005; note that some wait types not excluded are also never used.
-	Wait types that can occur only at times when they do not affect user activity, such as during initial server startup and shutdown, and are not visible to users.
-	Wait types that are innocuous but have caused concern among users because of their high occurrence or duration
The complete list of wait types is available by enabling trace flag 8001. The only effect of this trace flag is to force sys.dm_os_wait_stats to display all wait types."



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 1216 | **Info?** KB: mentions 1216 in passing as a flag in SQL 2000 that is associated with deadlock output. The mention occurs only to distance TF1216 on SQL 2000 from TF 1216 on SQL 7.0 and TF 1261 on SQL 2000, which both have a different purpose than 1216 on SQL 2000. Prob needs to be tested on modern versions. | [319892](http://support.microsoft.com/kb/319892) | 
| 1236 | (On by default in SQL 2014 SP1+ and SQL 2012 SP3) Partitions the DATABASE lock type to help reduce contention on internal locking structures symptomized by LOCK_HASH spinlock contention. | [2926217](http://support.microsoft.com/kb/2926217) | 
| 2456 | Relieves RESOURCE_SEMAPHORE_MUTEX contention, which may be primarily due to a bug in SQL 2005. | [956375](http://support.microsoft.com/kb/956375/en-us) | 
