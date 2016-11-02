# SQL Miscellaneous Informational (especially re: Error Log)

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 830 ([Disk and Network IO](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DiskNetworkIO.md))

## Functionality Toggles
| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 260 | **Info** **Doc2005** BOL: "Prints versioning information about extended stored procedure dynamic-link libraries (DLLs). For more information about __GetXpVersion(), see Creating Extended Stored Procedures. Scope: global or session" | | 
| 2505 | Randal-SQL-SDB407: "Turns off tracing calls to DBCC TRACEON/TRACEOFF". *(Very old) KB: prevents messages of the form "99/03/04 12:42:07.28 spid10 DBCC TRACEON 208, SPID 10" from being written to the SQL error log. (DBCC TRACEON(208) just means "SET QUOTED IDENTIFIER ON")* | [Randal-SQL-SDB407](https://www.scribd.com/document/109431789/Randal-SQL-SDB407-Undocumented); [243352](http://support.microsoft.com/kb/243352/en-us) | 
| 2588 | **Info** Forces DBCC HELP to return syntax of undocumented DBCC statements. Also affects dbcc help ('?') | [PRand](http://www.sqlskills.com/blogs/paul/dbcc-writepage/) | 
| 3604 | **Info** Enables output from a large number of trace flags and DBCC commands to be sent back to the client. *The Connect issue notes that problems can occur when using 3604 with a query that executes across a linked server.* The CSS page points out that 3604 is necessary for DBCC DumpTrigger('display'). | [CSS](http://blogs.msdn.com/b/psssql/archive/2009/05/11/how-do-i-determine-which-dump-triggers-are-enabled.aspx); [AskJay](http://blogs.msdn.com/b/askjay/archive/2011/01/21/why-do-we-need-trace-flag-3604-for-dbcc-statements.aspx); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/306380/trace-flag-issue-7300-3604) | 
| 3605 | **Info** Sends various info to the SQL error log instead of to the user console. Often referenced in KB and blog articles in the context of other trace flags (e.g. 3604).  | [AskJay](http://blogs.msdn.com/b/askjay/archive/2011/01/21/why-do-we-need-trace-flag-3604-for-dbcc-statements.aspx) | 
| 3659 | **Info** Appears to enable extra error logging (and perhaps just during SQL startup) to the SQL error log. In a number of places online, this flag seems to have been associated with changing server collation (e.g. [here](http://spaghettidba.com/2011/05/26/changing-server-collation/) and [here](http://www.sqlservercentral.com/Forums/Topic269173-146-1.aspx)), but as Gail says, the flag itself is completely separate from the "-q" startup parameter or the topic of collation. | [CSS](http://blogs.msdn.com/b/psssql/archive/2008/09/05/sql-server-2005-setup-fails-in-wow-x86-on-computer-with-more-than-32-cpus.aspx?Redirected=true); [WBeattie](http://archive.msdn.microsoft.com/Wiki/View.aspx?ProjectName=addselftosqlsysadmin) | 
| 3688 | Suppresses messages 19030 (SQL Trace ID %d was started by login "%s") and 19031 (SQL Trace stopped. Trace ID = '%d'. Login Name = '%s') from going to the SQL error log. KB: notes that pre-2005 versions of SQL Server could accomplish similar behavior with the sp_altermessage procedure. | [922578](http://support.microsoft.com/kb/922578) | 



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 2520 | **Info** Equivalent of 2588 for SQL 2000 (and below?) | [PRand](http://www.sqlskills.com/blogs/paul/dbcc-writepage/) | 