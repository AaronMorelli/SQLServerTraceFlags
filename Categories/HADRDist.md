# High Availability/Distributed Technologies

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 3448 [Connectivity](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Connectivity.md), 
9024 [Transactions, Logging, and Recovery](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/TranLogRecov.md) (KB references in context of AGs)


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 101 | **Info** Helps troubleshooting Merge replication slowness via increased logging | [2892633](http://support.microsoft.com/kb/2892633/en-us) | 
| 102 | **Info** Seems to write the output from TF 101 to a table named msmerge_history | [2892633](http://support.microsoft.com/kb/2892633/en-us) | 
| 106 | NOTE: This is NOT a SQL DB Engine trace flag, but rather a parameter passed to Replmerg.exe. It has been included only due to the frequent confusion. DB Engine TF 106 is actually an older TF that appears to no longer be relevant. | [BOL](http://msdn.microsoft.com/en-us/library/ms151872(v=sql.100).aspx) | 
| 1439 | **Info** Banerjee: "Trace database restart and failover messages to SQL Errorlog for mirrored databases" | [983500](http://support.microsoft.com/kb/983500/en-us); [Banerjee 2012](http://troubleshootingsql.com/2014/01/20/sql-server-2012-trace-flags/) | 
| 1448 | **Doc2012** BOL: "Enables the replication log reader to move forward even if the async secondaries have not acknowledged the reception of a change. Even with this trace flag enabled the log reader always waits for the sync secondaries. The log reader will not go beyond the min ack of the sync secondaries. This trace flag applies to the instance of SQL Server, not just an availability group, an availability database, or a log reader instance. Takes effect immediately without a restart. This trace flag can be activated ahead of time or when an async secondary fails." *SSC: Available since SQL 9* | [937041](http://support.microsoft.com/kb/937041); [983480](http://support.microsoft.com/kb/983480) | 
| 1462 | **Doc2014** (more below) Disables log stream compression for mirroring and AGs | | 
| 3923 | Causes SQL Server to raise an exception when the warning message #3303 is raised. This enables certain client libraries to trigger exceptions when a transaction is in-doubt because of an Availability Group failover. | [3014867](http://support.microsoft.com/kb/3014867) | 
| 8202 | Causes every replicated UPDATE statement to be replicated as a DELETE/INSERT pair. (Normally certain updates can be replicated as an UPDATE) | [160181](http://support.microsoft.com/kb/160181/EN-US); [238254](http://support.microsoft.com/kb/238254/en-us);  [197176](http://support.microsoft.com/kb/197176/en-us) | 
| 8206 | Allows the replication of stored procedure execution for heterogenous subscribers | [284228](http://support.microsoft.com/kb/284228/en-us) | 
| 8207 | (more below) **Doc2012** Causes updates on a unique column that affect only one row to be replicated as an update and not as a delete/insert pair. | | 
| 8209 | **Info** Apparently enables a "ReplSchemaTrace" functionality in the replication code base and logs a variety of error messages to the SQL Error log. | [916706](http://support.microsoft.com/kb/916706/en-us) | 
| 9559 | For AGs, "when enabled on the secondary ignores the redo target provided from the primary progress message and always set the redo target at the Max LSN value." | [CSS](http://blogs.msdn.com/b/alwaysonpro/archive/2013/12/04/recovery-on-secondary-lagging-shared-redo-target.aspx) | 
| 9567 | **Doc2014** "Enables compression of the data stream for availability groups during automatic seeding. Compression can significantly reduce the transfer time during automatic seeding and will increase the load on the processor.” | | 
| 9592 | **Doc2014** "Enables log stream compression for synchronous availability groups. This feature is disabled by default on synchronous availability groups because compression adds latency." | | 




**1462** **Doc2014** Disables the DB mirroring log stream compression functionality of SQL 2008 (effectively reverting that particular behavior back to SQL 2005).
Similarly, in AGs, disables log stream compression (on by default in 2012 & 2014, and on for asynchronous in 2016). 
[PRand](http://www.sqlskills.com/blogs/paul/sql-server-2008-performance-boost-for-database-mirroring/) |
[SQLCAT](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0CCYQFjAB&url=http%3A%2F%2Fdownload.microsoft.com%2Fdownload%2F0%2FF%2FB%2F0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D%2FSQLCAT%27s%2520Guide%2520to%2520High%2520Availability%2520Disaster%2520Recovery.pdf&ei=4eGJVfzVHI35oASj25ygBg&usg=AFQjCNHWYH0t3bF8HqBuNuutrielPX3hJg&sig2=xno-YS7agP9DRG-CzNe0Ug&bvm=bv.96339352,bs.1,d.b2w) |
[JChang](http://sqlblog.com/blogs/joe_chang/archive/2014/03/13/hekaton-and-benchmarks.aspx) |
[SimGreci](https://blogs.technet.microsoft.com/simgreci/2016/06/17/sqlserver-2016-alwayson-and-new-log-transport-behavior/) 

**8207** **Doc2012** BOL: "Enables singleton updates for Transactional Replication. Updates to subscribers can be replicated as a DELETE and INSERT pair. 
This might not meet business rules, such as firing an UPDATE trigger. With trace flag 8207 an update to a unique column that affects only one row (a singleton update) 
is replicated as an UPDATE and not as a DELETE or INSERT pair. If the update affects a column on which has a unique constraint or if the update affects multiple rows, 
the update is still replicated as a DELETE or INSERT pair."	
[302341](http://support.microsoft.com/kb/302341) | 
[889553](http://support.microsoft.com/kb/889553/en-us) | 
[889552](http://support.microsoft.com/kb/889552/en-us)


## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 120 | "FIX: Error message when you schedule a Replication Merge Agent job to run after you install SQL Server 2000 Service Pack 4: "The process could not enumerate changes at the 'Subscriber'" " | [925678](http://support.microsoft.com/kb/925678/en-us) | 
| 1400 | Enables the use of DB mirroring in SQL 2005 RTM. This flag is unneeded as of SP1. | [KB916940](http://support.microsoft.com/kb/916940/en-us); [KB907741](http://support.microsoft.com/kb/907741/en-us) | 
| 1449 | Enables a hotfix that resolves connectivity error behavior when connecting to a SQL mirrored DB | [936179](http://support.microsoft.com/kb/936179/en-us) | 
| 3499 | Enables a workaround for doing a rolling upgrade from SQL 2005 to 2008 with a DB that has a full-text index. | [9560170](http://support.microsoft.com/kb/956017/en-us) | 
| 8446 | Necessary on certain SQL 2005 builds to set up DB mirroring on a DB that has been restored/upgraded from SQL 2000. | [959008](http://support.microsoft.com/kb/959008/en-us) | 
| 9532 | Required in CTP versions of Denali (SQL 2012) for some areas of AlwaysOn Availability Group functionality. | [Connect](http://connect.microsoft.com/SQLServer/feedback/details/669176/remove-replica-from-the-availability-group); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/682581/denali-hadron-read-only-routing-url-is-not-yet-implemented) | 
