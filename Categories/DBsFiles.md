# Databases, Database Files, and Allocation (incl TempDB)

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 3449 and 8903 ([Disk and Network IO](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DiskNetworkIO.md))

(TODO: ensure 1802 is present in the See Also for Security)
(TODO: find other entry for 9394 and place an in-line link to its other home. Ditto for the other section)
(TODO: 10207 should be moved to the corrupt Checkdb section and placed here in the see also section)
TODO: is 9929 in multiple locations?

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 272 | (longer below) Reverts identity col behavior to pre-2012 (rely on t-log records for EACH identity generated rather than the MAX id of a batch) | [Connect](http://connect.microsoft.com/SQLServer/feedback/details/739013/alwayson-failover-results-in-reseed-of-identity) | 
| 647 | Avoids a new-in-SQL 2012 data check (done when adding a column to a table) that can cause ALTER TABLE… ADD <column> operations to take a very long time. *The KB has a useful query for determining the row size for a table.* | [2986423](https://support.microsoft.com/en-us/kb/2986423) |
| 670 | CSS: Disables deferred deallocation. But note Paul White’s comment on the post! The flag # may actuall by 671. | [CSS](https://blogs.msdn.microsoft.com/psssql/2009/11/17/how-it-works-controlling-sql-server-memory-dumps/) | 
| 671 | May be the correct flag # to disable deferred deallocation | [CSS](https://blogs.msdn.microsoft.com/psssql/2009/11/17/how-it-works-controlling-sql-server-memory-dumps/) | 
| 1106 | Creates a new RB in sys.dm_os_ring_buffers that tracks allocations made in TempDB. | [947204](https://support.microsoft.com/en-us/kb/947204); [Arvind](https://blogs.msdn.microsoft.com/arvindsh/2014/02/24/tracking-tempdb-internal-object-space-usage-in-sql-2012/); [BWPASS2011](https://www.youtube.com/watch?v=SvseGMobe2w&feature=youtu.be) | 
| 1165 | Outputs the recalculated #’s (every 8192 allocations) for the proportional fill algorithm when multiple files are present. Requires TF 3605, output goes to SQL error log. | [BWard](https://www.youtube.com/watch?v=SvseGMobe2w&feature=youtu.be); [PRand](http://www.sqlskills.com/blogs/paul/investigating-the-proportional-fill-algorithm/) | 
| 1180 | (Very old, may not be functional) KB notes that after a SQL 7.0 fix is installed, this flag will cause text/image data to be placed in free pages in partially-allocated extents; w/o the flag, text/image data is placed in newly-allocated extents until the file size limit is reached; only then will partially-allocated extents be used for new data. | [272220](https://support.microsoft.com/en-us/kb/272220) | 
| 1197 | BWard uses to prevent allocation of TempDB pages (by Work Tables) from being pulled from a worktable cache (see around 1:25:00). The (very old) KB references for use w/1180 in reclaiming space from inefficiently-stored text/image data. | [BWard](https://www.youtube.com/watch?v=SvseGMobe2w&feature=youtu.be); [324432](https://support.microsoft.com/en-us/kb/324432) |
| 1482 | Prints info to the Error Log (3605 not necess.) for a variety of transaction log operations, including when MinLSN value is reset, when a VLF is formatted, etc. (First discovered in Bob Ward’s PASS 2014 talk on SQL Server IO, and then tested for myself) | | 
| 1806 | Disables instant file initialization. | [PFE](https://blogs.msdn.microsoft.com/sql_pfe_blog/2009/12/22/how-and-why-to-enable-instant-file-initialization/); [PRand](http://www.sqlskills.com/blogs/paul/a-sql-server-dba-myth-a-day-330-instant-file-initialization-can-be-controlled-from-within-sql-server/); [2574695](https://support.microsoft.com/en-us/kb/2574695) | 
| 1808 | Directs SQL Server to ignore auto-closing databases even if the Auto-close property is set to ON. Must be set globally. Present in 2005+ | [Nacho](https://blogs.msdn.microsoft.com/ialonso/2012/04/11/want-your-sql-server-to-simply-ignore-the-auto_close-setting-for-all-open-databases-for-which-it-has-been-enabled/) | 
| 1816 | BWard briefly references this flag in his PASS2014 IO talk, saying it "could provide more details around errors" that occur with IO done to SQL data files in Azure Storage (stretch/http IO, I think he means). | | 
| 2330 | Stops the collection of statistics for sys.db_index_usage_stats. *CAdkin: also disables for sys.dm_db_missing_index_group_stats, and thus is useful when seeing high waits on the OPT_IDX_STATS spinlock. | [PRand](http://www.sqlskills.com/blogs/paul/the-pros-and-cons-of-trace-flags/); [2003031](https://support.microsoft.com/en-us/kb/2003031); [BOU](https://www.brentozar.com/archive/2015/11/trace-flag-2330-who-needs-missing-index-requests/); [CAdkin](https://exadat.co.uk/2015/04/14/well-known-and-not-so-well-known-sql-server-tuning-knobs-and-switches/) | 
| 2548 | "SQL 2005 has a –T2548 dbcc tracon(-1, 2548) that allows shrink* and other LOB_COMPACTION actions to be skipped. Enabling this returns shrink* behavior to that similar to SQL 2000." | [CSS](http://blogs.msdn.com/b/psssql/archive/2008/03/28/how-it-works-sql-server-2005-dbcc-shrink-may-take-longer-than-sql-server-2000.aspx) | 
| 3861 | Allows the DB startup code to move system tables to the primary filegroup. Introduced for bug in SQL 2014 upgrade process, where system tables could be created in a secondary filegroup (if that FG was the default). | [3003760](https://support.microsoft.com/en-us/kb/3003760) | 
| 9851 | Disables Hekaton’s auto-merge process; if this flag is enabled, the various merge-related procedures will need to be called manually. First seen in a Sunil Agarwal session at PASS 2014, also present in Kalen Delaney’s book on Hekaton. | | 
| 10204 | **Doc2016** "Disables merge/recompress during columnstore index reorganization. In SQL Server 2016, when a columnstore index is reorganized, there is new functionality to automatically merge any small compressed rowgroups into larger compressed rowgroups, as well as recompressing any rowgroups that have a large number of deleted rows." | | 
| 10207 | When a CCI is corrupt, allows a scan to skip corrupt segments and suppress errors 5288 and 5289, thus enabling the copy-out of data in a corrupt CCI. | [3067257](https://support.microsoft.com/en-us/kb/3067257); [RelSvcs](https://blogs.msdn.microsoft.com/sqlreleaseservices/partial-results-in-a-query-of-a-clustered-columnstore-index-in-sql-server-2014/) | 


**TF 272**: 
[Connect](http://connect.microsoft.com/SQLServer/feedback/details/739013/alwayson-failover-results-in-reseed-of-identity): "In SQL Server 2012 the implementation of the 
identity property has been changed to accommodate investments into other features. In previous versions of SQL Server the tracking of identity generation relied on 
transaction log records for each identity value generated. In SQL Server 2012 we generate identity values in batches and log only the max value of the batch. 
This reduces the amount and frequency of information written to the transaction log improving insert scalability. If you require the same identity generation semantics 
as previous versions of SQL Server there are two options available:
- Use trace flag 272. This will cause a log record to be generated for each generated identity value. The performance of identity generation may be impacted by turning on this trace flag.
- Use a sequence generator with the NO CACHE setting. This will cause a log record to be generated for each generated sequence value. Note that the performance of sequence value generation may be impacted by using NO CACHE."
Later in the Connect discussion, one commenter notes that when adding the TF as a startup flag, the flag only appears to work when using the "lowercase t" syntax rather than the more common "uppercase T" syntax.



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 1117 | (longer below) **Doc2014** all files in FG auto-grow simultaneously | | 
| 1118 | (longer below) **Doc2014** Uniform extent allocations (8 pages) are used immediately, instead of doing mixed extents first | | 
| 1140 | Workaround for a bug in 2005SP2/SP3 and SQL 2008 where mixed page allocations climb continually, due to a change in the way that mixed-page allocations are done. *KB has a great description of both the “old” and “new” way that free pages are found for a mixed-page allocation to be performed* | [2000471](https://support.microsoft.com/en-us/kb/2000471) |
| 1802 | Workaround for: "after you detach a Microsoft SQL Server 2005 database that resides on network-attached storage, you cannot reattach the SQL Server database... This problem occurs because SQL Server 2005 resets the file permissions when the database is detached. When you try to reattach the database, it cannot be attached because of limited share permissions." It sounds like this flag disables functionality in changing permissions on database files after the DB is detached, thus security implications. | [922804](https://support.microsoft.com/en-us/kb/922804); [Farlee](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2010/02/21/backup-compression-and-virtual-device-interface-vdi/) (comments, though Kevin may mean 1807) | 
| 1807 | Allows the creation of a database file on UNC paths, and is a workaround for errors 5105 and 5110. The KB describes MSFT policy towards DBs on network locations. | [304261](https://support.microsoft.com/en-us/kb/304261) | 
| 9394 | (9394 is either doing double-duty or there’s a typo. See other entry for 9394) Apparently enables a fix for an access violation when a table with Japanese characters has an indexed changed. | [3142595](https://support.microsoft.com/en-us/kb/3003760) | 
| 9929 | Enables an update that reduces the "disk footprint [of In-Memory OLTP] by reducing the In-Memory checkpoint files to 1 MB (megabytes) each." | [3147012](https://support.microsoft.com/en-us/kb/3147012) | 
| 10202 | SAgarwal PASS 2014 demo script: enables new DMV named sys.dm_db_column_store_row_group_physical_stats. DMV was not in 2014 at the time of the demo, thus appears to be in a future (or internal) version of SQL Server. | | 




**TF 1117**: **Doc2014** Upon enabling this flag, when any 1 data file needs to grow, all data files in the FG are grown (I assume each according to their filegrowth parameter). 
Note that Nacho gives a very special/rare edge case for sysfiles1. Bob Ward discusses in context of getting TempDB’s files to all auto-grow at the same time, preserving their 
equal size and thus the effectiveness of the proportional fill algorithm. Note that this TF applies to all DBs when on, not just TempDB. 
In SQL 2016, this flag has been replaced by the ALTER DATABASE <dbname> MODIFY FILEGROUP <filegroup> { AUTOGROW_ALL_FILES | AUTOGROW_SINGLE_FILE } configuration,
and the flag has no effect.
Chris Adkin has some interesting screenshots on its effect under certain workloads.
[BWard](https://www.youtube.com/watch?v=SvseGMobe2w&feature=youtu.be) 
| [PRand](http://www.sqlskills.com/blogs/paul/tempdb-configuration-survey-results-and-advice/) 
| [Nacho](https://blogs.msdn.microsoft.com/ialonso/2011/12/01/attempt-to-grow-all-files-in-one-filegroup-and-not-just-the-one-next-in-the-autogrowth-chain-using-trace-flag-1117/) 
| [CAdkin](https://exadat.co.uk/2015/04/14/well-known-and-not-so-well-known-sql-server-tuning-knobs-and-switches/) 
| [FastTrackDW](https://www.scribd.com/document/264335287/FTRARefConfigGuide-docx)
| [this post](http://sql-articles.com/articles/general/day-6trace-flag-1117-auto-grow-equally-in-all-data-file/)


**TF 1118**: **Doc2014** Changes allocation algorithm (for all DBs, not just TempDB) so that uniform extent allocations (8 pages) are used immediately, 
instead of doing mixed extents first. In SQL 2016, this flag has been replaced by the ALTER DATABASE <dbname> SET MIXED_PAGE_ALLOCATION { ON | OFF } configuration, 
and the flag has no effect.
Chris Adkin has some interesting screenshots on its effect under certain workloads.
[PRand](http://www.sqlskills.com/blogs/paul/misconceptions-around-tf-1118/) 
| [328551](https://support.microsoft.com/en-us/kb/328551) 
| [936185](https://support.microsoft.com/en-us/kb/936185) 
| [2154845](https://support.microsoft.com/en-us/kb/2154845) 
| [837938](https://support.microsoft.com/en-us/kb/837938) 
| [CSS](https://blogs.msdn.microsoft.com/psssql/2008/12/17/sql-server-2005-and-2008-trace-flag-1118-t1118-usage/) 
| [CSS](https://blogs.msdn.microsoft.com/psssql/2009/06/04/sql-server-tempdb-number-of-files-the-raw-truth/) 
| [CAdkin](https://exadat.co.uk/2015/04/14/well-known-and-not-so-well-known-sql-server-tuning-knobs-and-switches/)