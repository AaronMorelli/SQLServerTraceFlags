# Connectivity

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 

TODO: move 4112 to a section for old fix flags for the Query Processor
TODO: make sure query syntax/rules has a link to 7311 in its see also (or move this flag there)
TODO: Ensure the double-duty of 9394 is handled properly in linking between both docs

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3689 | See 4029 below | [SQLProtocols](http://blogs.msdn.com/b/sql_protocols/archive/2005/12/19/505372.aspx) (comment) | 
| 4010 | Allows only shared memory connections to the SQL Server, even if TCP/IP and Named Pipes are enabled protocols. | [MSBlog](http://blogs.msdn.com/b/sqlserverfaq/archive/2011/05/11/inf-hey-my-sql-server-service-is-not-starting-what-do-i-do.aspx); [CSS](http://blogs.msdn.com/b/psssql/archive/2008/09/05/sql-server-2005-setup-fails-in-wow-x86-on-computer-with-more-than-32-cpus.aspx) | 
| 4013 | Writes an entry to error log when a new connection is established, e.g.: Login: sa saSQL Query Analyzer(local)ODBCmaster, server process ID (SPID): 57, kernel process ID (KPID): 57. | maybe [sqlkbs](http://sqlkbs.blogspot.se/2008/01/trace-flag.html); [PRand SDB407](http://www.scribd.com/doc/109431789/Randal-SQL-SDB407-Undocumented) | 
| 4029 | "These [TF 3689 and 4029] will push more verbose errors to the errorlog when read+write errors occur in the network layer on the server side. [The flags] will dump out the socket error code." KB has an example. | [SQLProtocols](http://blogs.msdn.com/b/sql_protocols/archive/2005/12/19/505372.aspx) (comment); [949298](https://support.microsoft.com/en-us/kb/949298) | 
| 7300 | **Info** Can provide more info when encountering errors during the execution of a query across a linked server. Note the contradictory info between first KB and Connect item re: interaction between TF 3604 and this flag/linked servers.  | [314530](http://support.microsoft.com/kb/314530); [280106](http://support.microsoft.com/kb/280106/en-us); [280102](http://support.microsoft.com/kb/280102/en-us); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/306380/trace-flag-issue-7300-3604) | 
| 7311 | Offers a new alternative to handling the tricky problem of converting Oracle NUMBER types (across OLEDB linked server queries) with unknown precision/scale to a valid SQL Server data type, by treating all such types as NUMERIC(38,10). | [3051993](https://support.microsoft.com/en-us/kb/3051993) | 
| 7806 | **Doc2005** BOL: BOL: "Enables a dedicated administrator connection (DAC) on SQL Server Express. By default, no DAC resources are reserved on SQL Server Express. Scope: global only". *Bertrand shows its use in some strange connectivity scenarios with the DAC.* | [Bertrand](http://www.sqlperformance.com/2012/08/sql-memory/test-your-dac-connection) | 
| 7826 | "To completely disable the Connectivity Ring Buffer, globally enable trace flag 7826" | [SQLProtocols](http://blogs.msdn.com/b/sql_protocols/archive/2008/05/20/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer.aspx) | 
| 7827 | **Info** "Client-initiated connection closures are not traced [in the connectivity ring buffer] by default because they are a normal usage pattern... However, the option to trace them, in addition to the above events, is available by globally enabling trace flag 7827." | [SQLProtocols](http://blogs.msdn.com/b/sql_protocols/archive/2008/05/20/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer.aspx); [CSS](http://blogs.msdn.com/b/psssql/archive/2009/12/28/sql-2008-new-functionality-to-the-dm-os-ring-buffers-for-connectivity-troubleshooting.aspx); [Connect](https://connect.microsoft.com/SQLServer/feedback/details/518158/-packet-error-a-fatal-error-occurred-while-reading-the-input-stream-from-the-network) | 
| 7833 | Allows reversion to pre SQL 2012 SP2 CU8 behavior for a "silent error" condition in sqlcmd. | [3082877](https://support.microsoft.com/en-us/kb/3082877) | 
| 8765 | [Technet Wiki TF collection](http://social.technet.microsoft.com/wiki/contents/articles/13105.trace-flags-in-sql-server.aspx): "Allows use of variable length data, from ODBC driver; fixes the issue of a field returning the wrong data length." | [MySQL to MSSQL ODBC](http://bugs.mysql.com/bug.php?id=46857); [OLEDB connectivity issue](http://jacob.steelsmith.org/content/sql-server-and-ole-db) | 
| 9394 | (9394 may be doing double-duty, or there’s a typo. See other entry for 9394) Apparently enables a performance fix when querying Oracle through a linked server and a NUMBER data type is involved. | [3138659](https://support.microsoft.com/en-us/kb/3138659) | 
	



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3448 | KB: fixes a race condition leading to a hung database in mirroring failover situations. "This trace flag forces new connections to keep checking for database state every two seconds instead of waiting for a lock for infinite time. It helps ending the connection tasks faster as the mirroring reaches the start of the recovery phase and releasing more worker threads to be used by database mirroring." | [2970421](http://support2.microsoft.com/kb/2970421) | 
| 4112 | Fixes a problem with remote queries in 2005/2008 that target linked servers to SQL 2000 instances. | [936223](http://support.microsoft.com/kb/936223/en-us) | 
