# Security

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 1802 [Databases, Database Files, and Allocation (incl TempDB)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DBsFiles.md), 
4606 [Instance Startup, Database Startup, and General Instance Behavior](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/InstStart_DBStart_GenInst.md)

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3625 | **Doc2005** BOL: "Limits the amount of information returned to users who are not members of the sysadmin fixed server role, by masking the parameters of some error messages using '******'. This can help prevent disclosure of sensitive information. Scope: global only" | | 
| 4606 | SQLServerFAQ: "disables password policy check during server startup". KB uses it to bypass problematic 2000->2005 upgrade where the "sa" password doesn’t conform to Windows 2003 password requirements. | [936892](http://support.microsoft.com/kb/936892); [SQLserverfaq](http://blogs.msdn.com/b/sqlserverfaq/archive/2011/05/11/inf-hey-my-sql-server-service-is-not-starting-what-do-i-do.aspx); [SQLserverfaq](http://blogs.msdn.com/b/sqlserverfaq/archive/2008/07/31/upgrade-of-sql-server-2000-instance-to-sql-server-2005-fails-with-error-similar-to-enforce-password-policy.aspx) | 
| 4612 | "...no new entries will be made into the ring buffer, if this is set." Prob just RING_BUFFER_SECURITY_ERROR rather than all ring buffers. | [Laurentiu](http://blogs.msdn.com/b/lcris/archive/2007/02/19/sql-server-2005-some-new-security-features-in-sp2.aspx) | 
| 4613 | "Generate a minidump file whenever an entry is logged into the ring buffer" (prob just RING_BUFFER_SECURITY_ERROR) | [Laurentiu](http://blogs.msdn.com/b/lcris/archive/2007/02/19/sql-server-2005-some-new-security-features-in-sp2.aspx) | 
| 4614 | Enables use of "SQL Server authenticated logins that use Windows domain password policy enforcement...even though the SQL Server service account is locked out or disabled on the Windows domain controller." | [925744](http://support.microsoft.com/kb/925744) | 
| 4616 | **Doc2005** BOL: "Makes server-level metadata visible to application roles. In SQL Server, an application role cannot access metadata outside its own database because application roles are not associated with a server-level principal. This is a change of behavior from earlier versions of SQL Server. Setting this global flag disables the new restrictions, and allows for application roles to access server-level metadata. Scope: global only" | [906549](http://support.microsoft.com/kb/906549/en-us) | 
| 9485 | **Doc2012** BOL: "Disables SELECT permission for DBCC SHOW_STATISTICS." *Available in SQL 2012 SP1; forces a regression in behavior re: the permissions necessary for DBCC SHOW_STATISTIC. Especially relevant for queries that access remote tables.*  | [2683304](http://support.microsoft.com/kb/2683304/en-us); [Nevarez](http://www.benjaminnevarez.com/2013/02/dbcc-show_statistics-works-with-select-permission/) | 





## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.