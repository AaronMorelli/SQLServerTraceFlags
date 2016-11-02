# Instance Startup, Database Startup, and General Instance Behavior

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 4606 and 4614 ([Security](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Security.md))

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 902 | **Doc2014** BOL: "Bypasses execution of database upgrade script when installing a Cumulative Update or Service Pack. WARNING: This trace flag is meant for troubleshooting of failed updates during script upgrade mode, and it is not supported to run it continuously in a production environment. Database upgrade scripts needs to execute successfully for a complete install of Cumulative Updates and Service Packs. Not doing so can cause unexpected issues with your SQL Server instance. Scope: Global only" *KB: part of a workaround for starting SQL Server after a CU has been applied on an instance that has a UCP. (Though specific functionality is not clear).* Technet and SSC: used to halt/bypass an in-progress CU update gone horribly wrong. | [2163980](http://support.microsoft.com/kb/2163980); [Technet](http://blogs.msdn.com/b/karthick_pk/archive/2010/11/18/sqlserver2008-script-level-upgrade-for-database-master-failed-because-upgrade-step-sqlagent100-msdb-upgrade-sql-encountered-error-574-state-0-severity-16.aspx); [SSC](http://www.sqlservercentral.com/blogs/everyday-sql/2015/07/07/use-trace-flag-902-to-recover-from-a-cumulative-update-failure/); [290080](http://support.microsoft.com/kb/290080) (old fix KB for 2000 SP1) | 
| 3408 | Forces SQL startup to use a single thread when recovering all DBs, instead of using an algorithm for how many threads to allocate to DB recovery | [Nacho] (http://blogs.msdn.com/b/ialonso/archive/2012/10/09/how-much-is-crash-recovery-parallelized-in-which-order-are-databases-recovered.aspx) |
| 3663 | Causse SQL Server to use the FILE_FLAG_WRITE_THROUGH flag on the SQL error log instead of relying on OS system cache involvement for error log write performance. | [CSS](http://blogs.msdn.com/b/psssql/archive/2011/01/07/discussion-about-sql-server-i-o.aspx) |
| 4022 | Directs the SQL instance to ignore stored procedures that have been configured as "auto-start" procedures. Their auto-start configuration is not affected, so the next time the instance is started w/o this flag they will return to their normal behavior. *SSC adds: "...this flag is a subset of startup option –f. TIP: Each SP consumes one worker thread while executing so you may prefer to make one startup procedure that calls others."* Mentioned as far back as Ken Henderson’s "Guru’s Guide" books. | [MSBlog](http://blogs.msdn.com/b/sqlserverfaq/archive/2011/05/11/inf-hey-my-sql-server-service-is-not-starting-what-do-i-do.aspx) |
| 8009 | IO Basics, Ch. 2: Enables the "idle state behavior" that a SQL instance can enter under certain conditions. | [IO Basics Chapter2](http://technet.microsoft.com/en-us/library/cc917726.aspx) | 
| 8010 | Disables the "idle state" behavior that a SQL instance can enter (see 8009). The KB describes a "hung" condition that can occur on SQL 2008 R2 Express, and this TF can prevent that state from occurring. | [IO Basics Chapter2](http://technet.microsoft.com/en-us/library/cc917726.aspx); [2633271](http://support.microsoft.com/kb/2633271/en-us); |



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 9806 | From SQLService.se: "Function: Unknown. Is turned on in SQL Server 2014 CTP1 standard installation in Windows Azure VM” | [SQLService.se](http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx) |
| 9807 | From SQLService.se: "Function: Unknown. Is turned on in SQL Server 2014 CTP1 standard installation in Windows Azure VM” | [SQLService.se](http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx) |
| 9808 | From SQLService.se: "Function: Unknown. Is turned on in SQL Server 2014 CTP1 standard installation in Windows Azure VM” | [SQLService.se](http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx) |	
