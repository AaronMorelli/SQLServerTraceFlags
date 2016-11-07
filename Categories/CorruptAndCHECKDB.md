# CHECKDB and Corruptions

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 

TODO: Strongly consider combining this and the BackupRestore section somehow

TODO: Reconsider where 2509 and 2514 go. (Have to do with file/page internals, but activated by DBCC CHECKDB)

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 806 | (more below) Whenever a data page is read into the buffer pool, a more intensive DBCC-style check is done. Can be used to catch memory-related corruption problems. | | 
| 815 | (more below) Helps detect memory scribblers by setting the VirtualProtect PAGE_READWRITE on a page only when it is about to be written by the SQL engine. Otherwise, the page is always read-only. | | 
| 818 | (more below) Tracks info re: writes in a ring buffer to catch problems with IO subsystem  | | 
| 831 | Randal-SQL-SDB407: "Protect unchanged pages in the buffer pool to catch memory corruptions." | [Randal-SQL-SDB407](http://www.scribd.com/doc/109431789/Randal-SQL-SDB407-Undocumented) | 
| 2508 | TODO: experiment to see if this has any kernel of truth, may just be a typo of 2528:  Social.Technet: Disables parallel non-clustered index checking for DBCC CHECKTABLE" | [Social.Technet](http://social.technet.microsoft.com/wiki/contents/articles/13105.trace-flags-in-sql-server.aspx) | 
| 2514 | **Info** Used with DBCC CHECKTABLE to see the total count of ghost records in a table. | [Argenis](http://sqlblog.com/blogs/argenis_fernandez/archive/2012/05/29/ghost-records-backups-and-database-compression-with-a-pinch-of-security-considerations.aspx) | 
| 2528 | (more below) Disables parallel processing for CHECKDB | | 
| 2529 | **Info?** Full functionality unknown, but appears to print memory usage for CHECKDB (memory that appears to be allocated via an “IMemObj” interface) at the very beginning and very end of CHECKDB output. | [SQLService.se](http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx) | 
| 2549 | (more below) Causes CHECKDB to consider each separate DB file as being on a unique drive | | 
| 2558 | Disables integration between CHECKDB and Watson | [CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx) | 
| 2562 | (more below) CHECKDB runs all indexes in a single batch rather than many batches | | 
| 2566 | Disables DATA_PURITY checks and was released as a workaround for several problems in x64 instances (as described in the KB articles). | [945770](http://support.microsoft.com/kb/945770); [Bertrand](http://www.sqlperformance.com/2012/11/io-subsystem/minimize-impact-of-checkdb); [2888996](http://support.microsoft.com/kb/2888996/en-us) | 


**806** PRand: "Causes 'DBCC-style' page auditing to be performed whenever a database page is read into the buffer pool. This is useful to catch cases where pages are 
being corrupted in memory and then written out to disk with a new page checksum. When they're read back in the checksum will look correct, but the page is corrupt 
(because of the previous memory corruption). This page auditing goes someway to catching this - especially on non-Enterprise Edition systems that don't have the 
'checksum sniffer'." [Paul notes that there will be a CPU hit if you turn this on].	
[PRand](http://www.sqlskills.com/blogs/paul/how-to-tell-if-the-io-subsystem-is-causing-corruptions/) | 
[841776](http://support.microsoft.com/kb/841776/en-us) | 
[IOBasics Chapter 2](http://technet.microsoft.com/en-us/library/cc917726.aspx)


**815** IO Basics: "To help detect unwanted changes to in-memory SQL Server data pages, latch enforcement is enhanced with the –T815 trace flag. When a page is latched for 
modification, the VirtualProtect on the page is set to PAGE_READWRITE. At all other times the protection is PAGE_READONLY. This can help catch actions such as memory 
overwrites (scribblers). Starting with...build 8.00.0922, you can dynamically turn on or turn off trace flag -T815 by using the DBCC TRACEON...TRACEOFF. 
Important Latch enforcement is only valid for non-AWE (Address Windowing Extensions) environments."	
[IOBasics](http://technet.microsoft.com/en-us/library/cc966500.aspx) | 
[CSS](http://blogs.msdn.com/b/psssql/archive/2012/11/12/how-can-reference-counting-be-a-leading-memory-scribbler-cause.aspx) 


**818**	IO Basics: "tracks the last 2,048 page write operations. During a successful write I/O completion (proper page ID, bytes transferred successfully, and the proper 
OS error codes), the DBID, Page ID, and LSN are recorded in a ring buffer. If a failure occurs, error 823 is raised. When an 823 or 605 error is detected, SQL Server 
looks in the ring buffer for the LSN value that was on the page during the last write. If not correct, extra information is added to the SQL Server error log. The 
information indicates the type of error along with both the expected and the retrieved LSN"	
[IOBasics](http://technet.microsoft.com/en-us/library/cc966500.aspx) | 
[826433](http://support.microsoft.com/kb/826433) | 
[828339](http://support.microsoft.com/kb/828339/en-us)


**2528** **Doc2005** BOL: "Disables parallel checking of objects by DBCC CHECKDB, DBCC CHECKFILEGROUP, and DBCC CHECKTABLE. By default, the degree of parallelism is 
automatically determined by the query processor. The maximum degree of parallelism is configured just like that of parallel queries. For more information, see 
[Configure the max degree of parallelism Server Configuration Option](http://technet.microsoft.com/en-us/library/ms189094.aspx).
Parallel DBCC should typically be left enabled. For DBCC CHECKDB, the query processor reevaluates and automatically adjusts parallelism with each table or batch of 
tables checked. Sometimes, checking may start when the server is almost idle. An administrator who knows that the load will increase before checking is complete may want 
to manually decrease or disable parallelism. Disabling parallel checking of DBCC can cause DBCC to take much longer to complete and if DBCC is run with the TABLOCK feature 
enabled and parallelism set off, tables may be locked for longer periods of time. Scope: global or session."
[PRand](http://www.sqlskills.com/blogs/paul/checkdb-from-every-angle-how-long-will-checkdb-take-to-run/)


**2549** DBCC CHECKDB builds an internal list of pages to read per unique disk drive across all database files. This logic determines unique disk 
drives based on the drive letter of the physical file name of each file. If the underlying disks are actually unique when the drive letters or not (e.g. mount points), 
the DBCC CHECKDB command would treat these as one disk. When this trace flag is enabled, each database file is assumed to be on a unique disk drive. 
Do not use this trace flag unless you know that each file is based on a unique physical disk. Scope – Any	
[2634571](http://support.microsoft.com/kb/2634571/en-us) | 
[PRand](http://www.sqlskills.com/blogs/paul/dbcc-checkdb-scalability-and-performance-benchmarking-on-ssds/) | 
[Bertrand](http://www.sqlperformance.com/2012/11/io-subsystem/minimize-impact-of-checkdb) | 
[CSS](http://blogs.msdn.com/b/psssql/archive/2012/02/23/a-faster-checkdb-part-ii.aspx) | 
[Connect](https://connect.microsoft.com/SQLServer/feedback/details/982532/sql-server-2014-rtm-ignores-trace-flag-2549) (problems w/SQL 2014)
[BWard](https://blogs.msdn.microsoft.com/bobsql/2016/07/12/dbcc-trace-flags-2562-and-2549/)


**2562**	When enabled, DBCC CHECKDB runs in a single "batch" regardless of the number of indexes in the database. By default, DBCC CHECKDB tries to minimize 
tempdb resources by limiting the number of indexes or "facts" that it generates by using a "batches" concept. This trace flag forces all processing into one batch, 
and can reduce contention on the DBCC_MULTIOBJECT_SCANNER latch. Using the flag can increase the amount of TempDB used during CHECKDB processing.	
[2634571](http://support.microsoft.com/kb/2634571/en-us) | 
[PRand](http://www.sqlskills.com/blogs/paul/how-does-dbcc-checkdb-with-estimateonly-work/) | 
[PRand](http://www.sqlskills.com/blogs/paul/dbcc-checkdb-scalability-and-performance-benchmarking-on-ssds/) | 
[PSSSQL](http://blogs.msdn.com/b/psssql/archive/2012/02/23/a-faster-checkdb-part-ii.aspx) | 
[Bertrand](http://www.sqlperformance.com/2012/11/io-subsystem/minimize-impact-of-checkdb) | 
[BWard](https://blogs.msdn.microsoft.com/bobsql/2016/07/12/dbcc-trace-flags-2562-and-2549/)




## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 2509 | The only reference I can find is found in Ken Henderson’s Guru’s Guide to T-SQL, on page 503 (found via books.google.com): "Used in conjunction with DBCC CHECKTABLE to see the total count of ghost records in a table." Maybe the SQL 2000 corollary to 2514 in modern builds?  | | 
