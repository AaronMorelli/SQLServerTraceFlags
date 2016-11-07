# Special Features

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 3101 (bypasses a CDC upgrade operation that can cause slow RESTORES) and 9958 (enables a fix for a Hekaton log restore bug) [Backup and Restore](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/BackupRestore.md)


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 1851 | (from Amanda Ford @ SQL Relay Reading 2014 via JustDave): "…disables the automerge functionality for in-memory oltp" | [JustDave](http://justdaveinfo.wordpress.com/2014/10/16/october-13-microsoft-sql-relay-reading/); [Amanda Ford Slides](http://www.sqlrelay.co.uk/resources/sql-relay-2014-slides.html) |
| 7601 | Turns on full text tracing (though perhaps just for the indexing process). Writes (apparently) to the SQL error log. | [Connect](http://connect.microsoft.com/SQLServer/feedback/details/526343/looking-for-documentation-on-trace-flags-7601-7603-7604-and-7605) | 
| 7603 | Ditto | | 
| 7604 | Ditto | | 
| 7605 | Ditto | | 
| 7608 | Appears to enable the full-text crawling process to use non-clustered unique indexes, improving performance. The KB implies that enabling this flag is *still* required even in modern versions. | [938672](https://support.microsoft.com/en-us/kb/938672) | 
| 7613 | Appears to enable a regression to SQL 2000 behavior regarding how words inside double-quoted phrases are handled by the full-text search FREETEXT predicate. | [927643](https://support.microsoft.com/en-us/kb/927643) | 
| 8011 | Appears to disable the Resource Monitor ring buffer. Scope global. | [920093](http://support.microsoft.com/kb/920093) | 
| 8040 | Kalen2008, p52: Disables Resource Governor; Similar behavior occurs if you use the –m or  –f startup options | | 
| 8295 | (more below) Modifies index definition for index added to internal/hidden Change Tracking tables. | | 
| 9837 | BWard PASS 2014 IO: enables "extra tracing but massive output" for Hekaton checkpoint files. | | 


	


## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 7614 | Appears to modify full-text index population behavior (in SQL 2005 SP2) so that indexed view matching can actually occur rather than the view ref being resolved to the underlying tables, improving performance. (Perhaps via the NOEXPAND hint)  | [928537](https://support.microsoft.com/en-us/kb/928537) | 
| 7646 | Appears to relieve blocking issues in SQL 2008/2008 R2 full text search when a high frequency of updates are occurring and being pushed to an internal FTS table.  | [FTS Whitepaper](https://technet.microsoft.com/en-us/library/cc721269(SQL.100).aspx#_Toc202506243); [DynamicsBlog](https://blogs.msdn.microsoft.com/axinthefield/sql-server-trace-flags-for-dynamics-ax/); [Blog](https://sqldev.wordpress.com/2008/09/16/sql-server-2008-full-text-slowness/); [Ozar](https://stackoverflow.blog/2008/11/sql-2008-full-text-search-problems/) |
| 9989 | In 2014 CTP2, enabled functionality for reading in-memory tables on a readable secondary | [Connect](https://connect.microsoft.com/SQLServer/feedback/details/795360/secondary-db-gets-suspect-when-i-add-in-memory-table-to-db-which-is-part-of-alwayson-availability-group) | 
| 9929 | Works around a condition where DML (e.g. index rebuild) on disk-based tables causes an excessive number of checkpoint files to be generated for in-memory tables. | [3147012](https://support.microsoft.com/en-us/kb/3147012) | 

**8295** SMartinez: "Creates a secondary index on the identifying columns on the change tracking side table at enable time"
Apparently turned on by default for SCCM 2012. [2276330](http://support.microsoft.com/kb/2276330) notes that one performance fix for CHANGETABLE is enabled by TF 4199. 
[2476322](http://support.microsoft.com/kb/2476322) | 
[Connect](http://connect.microsoft.com/SQLServer/feedback/details/783327/trace-flag-8295) | 
[KLittle](http://www.brentozar.com/archive/2014/06/performance-tuning-sql-server-change-tracking/) | 
[SMartinez](http://blogs.technet.com/b/smartinez/archive/2013/03/06/sql-for-configmgr-2012.aspx)
