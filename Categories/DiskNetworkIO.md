# Disk and Network IO

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 840 [SQLOS Memory and Buffer Pool](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/MemBuf.md), 
1816 [Databases, Database Files, and Allocation (incl TempDB)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DBsFiles.md), 
8744  ([Query Compilation Behavioral (except Cardinality Estimation)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/CompBehav.md))

TODO: move 1816 here.

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 652 | Disables IO read-ahead across the whole SQL instance. *PWhite forum answer to see TF 652 juxtaposed/contrasted with TF 8744.* Niko Tweet: "652 does not have any effect on CStore index scan" | [920093](https://support.microsoft.com/en-us/kb/920093); [Connect](https://connect.microsoft.com/SQLServer/feedback/details/780194/make-dbcc-trace-flags-available-as-option-querytraceon); [PWhite](https://answers.sqlperformance.com/questions/392/there-are-2-identical-worksets-in-question-this-is.html) | 
| 830 | Suppresses "SQL Server has encountered 2 occurrence(s) of I/O requests taking longer than 15 seconds to complete" messages from the SQL Server error log | [Skorlinski](https://blogs.msdn.microsoft.com/chrissk/2008/06/19/io-requests-taking-longer-than-15-seconds-to-complete-on-file/); [TechNet](https://technet.microsoft.com/en-us/library/aa175396%28v=SQL.80%29.aspx?f=255&MSPPError=-2147217396); [897284](https://support.microsoft.com/en-us/kb/897284) | 
| 8903 | Allows SQL Server to use a specific API (SetFileIoOverlappedRange) when Locked Pages in Memory is enabled | [CSS](https://blogs.msdn.microsoft.com/psssql/2012/03/20/setfileiooverlappedrange-can-lead-to-unexpected-behavior-for-sql-server-2008-r2-or-sql-server-2012-denali/); [2679255](https://support.microsoft.com/en-us/kb/2679255); [CSS](https://blogs.msdn.microsoft.com/psssql/2013/10/16/every-time-i-attach-database-sql-logs-error-1314-for-setfileiooverlappedrange/) | 
| 3917 | BWard PASS2014 IO talk: enables trace output (3605 is required) for the Eager Write functionality that is used with bulk logged operations (such as SELECT INTO) | | 
| 3940 | BWard PASS 2014 IO talk: forces the Eager Write functionality to throttle at 1024 outstanding eager writes | | 





## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 3449 | Enables a specific CU fix that, combined with indirect checkpoint, fixes a problem where creating a DB on a server with multi-TB amount of RAM takes a long time | [3158396](https://support.microsoft.com/en-us/kb/3158396) | 
