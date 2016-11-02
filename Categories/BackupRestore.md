# Backup and Restore

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 

TODO: 3028 may need to be moved to a fix flag section, ditto for 3057, 3111, 3117 
TODO: 3607 should at least be referenced in the TranLog Recovery section, and in the CheckDB Corruption section.
TODO: 9958 should at least be referenced in the TranLog Recovery section

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3001 | Erland: "suppresses logging to msdb.backuphistory...undocumented with all that means. On the other hand, I got [it] from a Microsoft engineer who said it was OK to share them." | [Erland](http://bytes.com/topic/sql-server/answers/162385-how-do-i-prevent-sql-2000-posting-message-event-viewer-application-log) | 
| 3004 | *Info** Prints trace info to the SQL error log about RESTORE (and BACKUP?) operations and can be used to view file initialization; use with 3605 to direct to error log | [CSS](http://blogs.msdn.com/b/psssql/archive/2008/01/23/how-it-works-what-is-restore-backup-doing.aspx); [SQLPFE](http://blogs.msdn.com/b/sql_pfe_blog/archive/2009/12/23/how-and-why-to-enable-instant-file-initialization.aspx); [SQLMunkee](http://www.sqlmunkee.com/2014/04/sql-trace-flag-3004-and-backup-database.html?m=1) | 
| 3014 | **Info** Prints info to the error log about config option values chosen during the BACKUP command. In the CSS blog post, used with 3213 (not sure how they are different; more testing is needed). | [CSS](http://blogs.msdn.com/b/psssql/archive/2008/02/06/how-it-works-how-does-sql-server-backup-and-restore-select-transfer-sizes.aspx) | 
| 3034 | (is this just for VDI?) overrides the server default, and thus always forces backup compression unless the backup command had the no_compression clause explicitly present. | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/02/24/vdi-backups-and-backup-compression-default.aspx) | 
| 3035 | (is this just for VDI?) overrides the server default to always avoid compression, unless the backup command explicitly uses the compression clause. If both 3034 and 3035 are enabled, 3035 takes precedence | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/02/24/vdi-backups-and-backup-compression-default.aspx) | 
| 3039 | As long as the SQL edition supports backup compression, this will allow VDI backups to be affected by the default compression setting just as non-VDI BACKUP commands are affected.  | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/02/24/vdi-backups-and-backup-compression-default.aspx) | 
| 3042 | **Doc2012** BOL: Bypasses the default backup compression pre-allocation algorithm to allow the backup file to grow only as needed to reach its final size." May have perf overhead. See BOL links for details on pre-allocation algorithm. *The KB article discusses the algorithm used to estimate space when the TF is NOT on.* | [2001026](http://support.microsoft.com/kb/2001026); [CSS](http://blogs.msdn.com/b/psssql/archive/2011/08/11/how-compressed-is-your-backup.aspx?Redirected=true) | 
| 3205 | **Doc2005** BOL:"By default, if a tape drive supports hardware compression, either the DUMP or BACKUP statement uses it. This flag disables hardware compression for tape drivers. This is useful when you want to exchange tapes with other sites or tape drives that do not support compression. Scope: global or session" | | 
| 3210 | **Info** BWard PASS 2014 IO talk: prints information about "collisions and wait times" that occur between the various "Asynchronous Disk Pool" threads during BACKUP (what about RESTORE?) operations.	 | | 
| 3212 | **Info** Prints "Backup stats" to the SQL log | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/10/24/why-does-restoring-a-database-needs-tempdb.aspx) | 
| 3213 | Prints info about BACKUP parameter values used, especially regarding Buffer size/number and Max Transfer size. | [CSS](http://blogs.msdn.com/b/psssql/archive/2008/02/06/how-it-works-how-does-sql-server-backup-and-restore-select-transfer-sizes.aspx); [CSS](http://blogs.msdn.com/b/psssql/archive/2008/01/28/how-it-works-sql-server-backup-buffer-exchange-a-vdi-focus.aspx) | 
| 3216 | **Info** Prints info about RESTORE internals. Only seems to print to the error log (TF 3605 is required). Not able to find an official link. | [JamesSQL](http://jamessql.blogspot.com/2013/07/trace-flag-for-backup-and-restore.html) | 
| 3222 | Disables the read ahead that is used by the recovery operation during roll forward operations | [268081](http://support.microsoft.com/kb/268081/en-us) | 
| 3226 | **Doc2008** BOL: "By default, every successful backup operation adds an entry in the SQL Server error log and in the system event log...With this trace flag, you can suppress these log entries. This is useful if you are running frequent log backups and if none of your scripts depend on those entries."	[PRand](http://www.sqlskills.com/blogs/paul/fed-up-with-backup-success-messages-bloating-your-error-logs/); [StorEng](http://blogs.msdn.com/b/sqlserverstorageengine/archive/2007/10/30/when-is-too-much-success-a-bad-thing.aspx) | | 
| 3400 | Connect: appears (based on context) to print information re: the RESTORE process. BWard PASS 2014 IO talk: explained as printing out "checkpoint pages/sec" (to the Error Log, presumably) | [Connect](http://connect.microsoft.com/SQLServer/feedback/details/392158/recovery-portion-of-sql-2008-restore-takes-much-longer-than-normal-when-restoring-from-sql-2005-backup) |
| 3422 | PRand: "Cause auditing of transaction log records as they're read (during transaction rollback or log recovery). This is useful because there is no equivalent to page checksums for transaction log records and so no way to detect whether log records are being corrupted.” [There is a CPU hit for turning this on]. | [PRand](http://www.sqlskills.com/blogs/paul/how-to-tell-if-the-io-subsystem-is-causing-corruptions/); [IO Basics, chapter 2](https://technet.microsoft.com/en-us/library/cc917726.aspx) |
| 3607 | Khen2005, page 80: lets SQL open master w/out running recovery on it. Other sources say that SQL doesn’t try to "start up" master. The differences in wording may not be important. | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/10/24/why-does-restoring-a-database-needs-tempdb.aspx) |
| 3608 | **Doc2008** BOL: "Prevents SQL Server from automatically starting and recovering any database except the master database. Databases will be started and recovered when accessed. Some features, such as snapshot isolation and read committed snapshot, might not work. Use for Move System Databases and Move User Databases. Do not use during normal operation."  | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/05/04/the-seven-reasons-why-auto-update-stats-event-will-not-trigger-despite-how-many-modifications-affect-any-of-the-tables-involved-in-a-compiled-plan.aspx); [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/10/24/why-does-restoring-a-database-needs-tempdb.aspx); | 
| 3609 | Skips the creation of the tempdb database at startup. Use this trace flag if the device or devices on which tempdb resides are problematic or problems exist in the model database. | [PRand](http://www.sqlskills.com/blogs/paul/search-engine-qa-18-whats-the-current-uptime-of-sql-server/); [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/10/24/why-does-restoring-a-database-needs-tempdb.aspx) | 
| 3660 | W/o this flag, for DBs that have Auto_Close=true and for DBs on Express Edition, DB recovery is normally deferred until first user access when SQL starts up. This TF forces DB recovery to always run (well, only for DBs that actually need recovery done) at SQL Server startup. | [Nacho](http://blogs.msdn.com/b/ialonso/archive/2012/10/09/how-much-is-crash-recovery-parallelized-in-which-order-are-databases-recovered.aspx) | 





## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3023 | **Doc2014** Enables CHECKSUM option as default for BACKUP command...Note: Beginning with SQL Server 2014 this behavior is controlled by setting the backup checksum default configuration option. Scope: global and session | [2656988](http://support.microsoft.com/kb/2656988) | 
| 3028 | Enables a hotfix for a problem encountered when backing up to tape with specific backup options | [940379](http://support.microsoft.com/kb/940379/en-us) | 
| 3031 | Will turn the NO_LOG and TRUNCATE_ONLY options into checkpoints in all recovery modes (applicable to SQL 2005) | [PRand](http://www.sqlskills.com/blogs/paul/backup-log-with-no_log-use-abuse-and-undocumented-trace-flags-to-stop-it/) | 
| 3057 | Enables a hotfix change that allows a log backup that was taken on a t-log file hosted on a drive with "Bytes per physical sector"=512 to be restored onto a log file/drive that has "Bytes per physical sector"=4096 | [2987585](http://support.microsoft.com/kb/2987585/en-us) | 
| 3101 | Causes the RESTORE process to bypass a CDC upgrade operation that can cause slow RESTORE operations under certain conditions | [2567366](http://support.microsoft.com/kb/2567366/en-us) | 
| 3111 | "FIX: Backup or Restore Using Large Transaction Logs May Return Error 3241" Causes LogMgr::ValidateBackedupBlock to be skipped during backup and restore operations, allowing backups of very large T-logs to succeed. | [297104](http://support.microsoft.com/kb/297104/en-us) | 
| 3117 | Enables a fix in 2005 when restoring from a file- or filegroup-based snapshot backup. 3117 causes the restore process to use an approach found in SQL 2000 rather than 2005's logic. | [915385](https://support.microsoft.com/en-us/kb/915385) | 
| 3231 | SQL 2000/2005 - Will turn the NO_LOG and TRUNCATE_ONLY options into no-ops in FULL/BULK_LOGGED recovery mode, and will clear the log in SIMPLE recovery mode. | [PRand](http://www.sqlskills.com/blogs/paul/backup-log-with-no_log-use-abuse-and-undocumented-trace-flags-to-stop-it/); [KTripp](http://www.sqlskills.com/blogs/kimberly/understanding-backups-and-log-related-trace-flags-in-sql-server-20002005-and-2008/) | 
| 9109 | Used to workaround a problem with query notifications and restoring a DB with the NEW_BROKER option enabled. | [2483090](http://support.microsoft.com/kb/2483090/en-us) | 
| 9958 | Enables a fix that allows the restore of a Hekaton transaction log backup when a certain bug is hit. | [3171002](https://support.microsoft.com/en-us/kb/3171002) | 
