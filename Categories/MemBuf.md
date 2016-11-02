# SQLOS Memory and Buffer Pool

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 2456 [Locking and Waiting](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/LockWait.md), 
3449 [Disk and Network IO](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DiskNetworkIO.md),
6527 [Exceptions and Memory Dump Behavior](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/ExceptMemDump.md), 
8004 [Exceptions and Memory Dump Behavior](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/ExceptMemDump.md),
8032 [Plan Caching](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/PlanCache.md)


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 834 | (more below) **Doc2014**  SQL allocates memory at startup using Windows Large Pages | | 
| 835 | BWard PASS 2013 talk: causes SQL to not use the Locked Memory model even when the SKU and the LPIM privilege would normally allow SQL to use the Locked Memory model | | 
| 836 | KB: SQL sizes BPool at startup based on max server mem instead of on total physical mem. Use to reduce number of buf descriptors that are allocated at startup in 32-bit AWE mode. Startup only. *Slava: way to get SQL 2005 to behavior like SQL 2000 in terms of allocating AWE memory at startup instead of on demand.* | [920093](http://support.microsoft.com/kb/920093); [Slava](http://blogs.msdn.com/b/slavao/archive/2006/08/03/687573.aspx); [CSS](http://blogs.msdn.com/b/psssql/archive/2012/12/11/how-it-works-sql-server-32-bit-pae-awe-on-sql-2005-2008-and-2008-r2-not-using-as-much-ram-as-expected.aspx) | 
| 840 | Whenever a single page read is done and the BP hasn’t come close to its target size, the read is turned into a read of the extent that contains the page. Only needed for non-Ent/Dev/Eval editions. | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2011/12/09/the-read-ahead-that-doesn-t-count-as-read-ahead.aspx); [912322](http://support.microsoft.com/kb/912322) | 
| 842 | **Info** BWard PASS2013: "tracks any buffer gets" and therefore 1) the numbers in dm_os_memory_node_access_stats will be non-zero, 2) the page_reference_tracker XEvent will start firing. Very significant performance impact | | 
| 845 | Enables support for Locked Pages on Standard Editions of SQL 2005 and 2008 64-bit. Startup only. | [970070](http://support.microsoft.com/kb/970070); [2708594](http://support.microsoft.com/kb/2708594) | 
| 851 | BWard PASS 2014 IO talk: "disable[s] BPE even if enabled via ALTER SERVER" | | 
| 3654 | (more below) enables tracing for all "Memory Object" allocations to sys.dm_os_memory_allocations | | 
| 4610 | (more below) **Doc2014** "Increases the size of the hash table that stores the cache entries by a factor of 8. When used together with trace flag 4618 increases the number of entries in the TokenAndPermUserStore cache store to 8192." | | 
| 4618 | (more below) **Doc2014** "Limits the number of entries in the TokenAndPermUserStore cache store to 1,024. When used together with trace flag 4610 increases the number of entries in the TokenAndPermUserStore cache store to 8192." | | 
| 4620 | (related to 4610 and 4618) causes permission checking to be done on a global cache instead of the per-user caches that were introduced in SQL 2008. The thread includes some interesting info on the cache stores, especially as relates to TokenPermAndUserStore. | [Connect](http://connect.microsoft.com/SQLServer/feedback/details/467661/sql-server-2008-has-incorrect-cache-names-in-sys-dm-os-memory-cache-counters) | 
| 4621 | Combined with a registry key, allows customization of the size of the TokenAndPermUserStore. If this flag is used in combination with 4618, 4618 takes precedence and 4621 will be ignored. | [959823](http://support.microsoft.com/kb/959823) | 
| 8020 | **Doc2014** BOL: "Disable working set monitoring. Scope global only" KB: "SQL Server uses the working set size when interpreting the global memory state signals from the OS. 8020 removes this working set size consideration. If you use this trace flag incorrectly, heavy paging occurs, and the performance is poor." | [920093](http://support.microsoft.com/kb/920093); [CSS](http://blogs.msdn.com/b/psssql/archive/2009/09/17/how-it-works-what-are-the-ring-buffer-resource-monitor-telling-me.aspx) | 
| 8809 | CSS: Referenced in passing in relation to debugging memory scribbler problems | [CSS](http://blogs.msdn.com/b/psssql/archive/2012/11/12/how-can-reference-counting-be-a-leading-memory-scribbler-cause.aspx) |


**834** **Doc2014** BOL: "Uses Microsoft Windows large-page allocations for the buffer pool. Note: If you are using the Columnstore Index feature of SQL Server 2012 to 
SQL Server 2016, we do not recommend turning on trace flag 834. Scope: global only"
SQL allocates memory at startup using Windows Large Pages; Increasing (what about decreasing?) max serv mem will not cause a change in mem usage until instance restart. 
Memory consumers that can use mem allocated via Large Pages varies by SQL version. Startup flag only. 
Bob Ward mentions this TF in his PASS2013 talk on memory, noting that while Enterprise SKU, Phys Mem >= 8GB, and LPIM are enough to have the SQL Engine tell SQLOS to 
"support" the use of Large Pages, the Large Page memory model won’t actually be used unless TF 834 is on.	
[920093](http://support.microsoft.com/kb/920093) |  [CSS](http://blogs.msdn.com/b/psssql/archive/2009/06/05/sql-server-and-large-pages-explained.aspx)


**3654** **Info** Apparently increases info found in the sys.dm_os_memory_allocations DMV (which appears to have 
[replaced](http://sqlug.be/blogs/wesleyb/archive/2007/07/25/memtoleave-issue-in-sql-server-2000-x86.aspx) 
the DBCC MEMOBJLIST command). *BWard PASS 2013 memory talk: turns on tracing for all memory allocations done by "Memory Objects" (a specific SQLOS memory term) 
and will have a significant impact on system performance.* CSS: mentioned in relation to debugging memory scribbler problems. 
[CSS](http://blogs.msdn.com/b/psssql/archive/2012/11/12/how-can-reference-counting-be-a-leading-memory-scribbler-cause.aspx) | 
[Slava](http://blogs.msdn.com/b/slavao/archive/2005/08/30/458036.aspx) |
[2888658](http://support.microsoft.com/kb/2888658/en-us) 


**4610** **Doc2014** BOL: "Increases the size of the hash table that stores the cache entries by a factor of 8. When used together 
with trace flag 4618 increases the number of entries in the TokenAndPermUserStore cache store to 8192"
This flag can be used together with both 4618 or 4621 (4621 has no effect if 4618 is enabled).
[959823](http://support.microsoft.com/kb/959823) |
[949298](http://support.microsoft.com/kb/949298/en-us) | 
[CSS](http://blogs.msdn.com/b/psssql/archive/2008/06/16/query-performance-issues-associated-with-a-large-sized-security-cache.aspx)


**4618** **Doc2014** BOL: "Limits the number of entries in the TokenAndPermUserStore cache store to 1024. When used together with trace flag 4610 increases the number of 
entries in the TokenAndPermUserStore cache store to 8192. Scope: global only"
[933564](http://support.microsoft.com/kb/933564): "...applying trace flag 4618 [limits] the number of entries per user cache store. Using trace flag 4618 
can incur a small CPU overhead because this trace flag removes old cache entries as new entries are inserted. The trace flag performs this action to limit the size of 
the cache store growth. However, the CPU overhead is spread over time."
[959823](http://support.microsoft.com/kb/959823) | 
[969086](http://support.microsoft.com/kb/969086) | 
[CSS](http://blogs.msdn.com/b/psssql/archive/2008/06/16/query-performance-issues-associated-with-a-large-sized-security-cache.aspx) |
[949298](http://support.microsoft.com/kb/949298) (a BizTalk-related KB article)




## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 845 | Enables support for Locked Pages on Standard Editions of SQL 2005 and 2008 64-bit. Startup only. | [970070](http://support.microsoft.com/kb/970070); [2708594](http://support.microsoft.com/kb/2708594) | 
| 6498 | **Doc2014** BOL: "Enables more than one large query compilation to gain access to the big gateway when there is sufficient memory available. It is based on the 80 percentage of SQL Server Target Memory, and it allows for one large query compilation per 25 gigabytes (GB) of memory. Note: Beginning with SQL Server 2014 SP2 and SQL Server 2016 this behavior is controlled by the engine and trace flag 6498 has no effect. Scope global only" | [3024815](http://support.microsoft.com/kb/3024815/en-us); [CSS](http://blogs.msdn.com/b/sql_server_team/archive/2015/10/09/query-compile-big-gateway-policy-changes-in-sql-server.aspx#.Vhgb1PlViko) | 
| 8030 | Supplied in a HF for SQL 2005; 64-bit servers w/16+ GB of RAM could encounter problems, symptomized by many dm_os_wait_stats [sic: dm_os_waiting_tasks?] rows with SOS_RESERVEDMEMBLOCKLIST or DBCC MEMORYSTATUS wait types. "This situation indicates that many multipage allocations exist." | [917035](http://support.microsoft.com/kb/917035) | 
| 8075 | Enables a fix (after applying the appropriate CU) for x64 VAS exhaustion. | [3074434](https://support.microsoft.com/en-us/kb/3074434) | 
