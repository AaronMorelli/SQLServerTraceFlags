# Exceptions and Memory Dump Behavior

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 4613 (Generate minidump upon new entry in security ring buffer) [Security](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Security.md)

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 1260 | **Doc2014** "Disable scheduler monitor dumps." Whitepaper: Disables mini-dump generation for "any of the 17883, 17884, 17887, or 17888 errors. The trace flag can be used in conjunction with trace flag –T1262. For example, you could enable –T1262 to get 10- and a 60-second interval reporting and also enable –T1260 to avoid getting mini-dumps."  | [DiagCorrect17883etc](http://msdn.microsoft.com/en-us/library/cc917684.aspx) | 
| 1262 | Changes minidump (for 1788* errors) behavior from dump-on-first-only to "initial occurrence plus 60 sec intervals for that same occurrence" SQL 2000 startup-only, 2005+ TRACEON works. | [DiagCorrect17883etc](http://msdn.microsoft.com/en-us/library/cc917684.aspx) | 
| 1264 | Originally forced inclusion of process names (the .exe filename) in memory dumps for non-yielding schedulers. The CSS post cast doubt on whether this is still necessary. | [2630455](http://support.microsoft.com/kb/2630455/en-us); [2630458](https://support.microsoft.com/en-us/kb/2630458/en-us); [CSS](http://blogs.msdn.com/b/psssql/archive/2010/05/27/sql-server-2008-r2-new-non-yield-ring-buffer-information.aspx) | 
| 2542 | CSS: "skip mini-dump". KB: (that discusses SQLDumper.exe more generally) mentions this flag off-hand. | [CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx); [827690](http://support.microsoft.com/kb/827690/en-us) | 
| 2544 | Creates a full memory dump | [917825](http://support.microsoft.com/kb/917825); [912422](http://support.microsoft.com/kb/912422/en-us); [AskJay](http://blogs.msdn.com/b/askjay/archive/2010/02/05/how-can-i-create-a-dump-of-sql-server.aspx); [Forum](http://social.msdn.microsoft.com/Forums/sqlserver/en-US/13ce4292-b8a7-41fa-a173-645693957d70/sqldumper?forum=sqldisasterrecovery) | 
| 2546 | All threads in SQL Server process are dumped (mini dump) | [917825](http://support.microsoft.com/kb/917825); [CSS](http://blogs.msdn.com/b/psssql/archive/2008/09/12/sql-server-2000-2005-2008-recovery-rollback-taking-longer-than-expected.aspx); [AskJay](http://blogs.msdn.com/b/askjay/archive/2010/02/05/how-can-i-create-a-dump-of-sql-server.aspx); [Forum](http://social.msdn.microsoft.com/Forums/sqlserver/en-US/13ce4292-b8a7-41fa-a173-645693957d70/sqldumper?forum=sqldisasterrecovery) | 
| 2551 | KB: "Produces a filtered memory dump" | [917825](http://support.microsoft.com/kb/917825); [CSS](http://blogs.msdn.com/b/psssql/archive/2008/09/12/sql-server-2000-2005-2008-recovery-rollback-taking-longer-than-expected.aspx); [AskJay](http://blogs.msdn.com/b/askjay/archive/2010/02/05/how-can-i-create-a-dump-of-sql-server.aspx); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/477863/sql-server-is-terminating-because-of-fatal-exception-c0150014) | 
| 3628 | CSS: "Includes 'other errors' in the dump based on a severity." | [CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx) | 
| 3629 | CSS: A memory dump will "include messages marked to include with this trace flag enabled." | [CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx) | 
| 6527 | (more below) **Doc2008** Disables memory dumps for OOM in the CLR, with some exceptions. | | 
| 8004 | Connect: "This trace flag will force SQL server to create a mini-dump once (the first time) when an out of memory condition is hit. The dump file will be placed in the SQL Server LOG folder." | [Connect](http://connect.microsoft.com/SQLServer/feedback/details/342691/not-enough-memory-was-available-for-trace-error-when-attempting-to-profile-sql-2008) | 
| 8018 | **Doc2014** BOL: "Disable the exception ring buffer. Scope: global only" Banerjee: Can’t be used as a startup flag | [920093](http://support.microsoft.com/kb/920093); [Amit Banerjee 2012](http://troubleshootingsql.com/2014/01/20/sql-server-2012-trace-flags/) | 
| 8019 | **Doc2014** BOL: "Disable stack collection for the exception ring buffer." Banerjee: Can’t be used as a startup flag | [920093](http://support.microsoft.com/kb/920093); [Amit Banerjee 2012](http://troubleshootingsql.com/2014/01/20/sql-server-2012-trace-flags/) | 
| 8024 | (More below) Adds conditions to the mini-dump generation logic for the 1788* errors | | 
| 8026 | CSS: "…tells dump trigger to remove the trigger after the first dump has been triggered. (Does not disable the automatic triggers)" | [917825](http://support.microsoft.com/kb/917825); [CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx) | 


**3656** SQLCAT: "Enables the call stacks to be resolved...requires the sqlservr.pdb file reside in the same directory as sqlservr.exe" Their post includes an 
example of using the flag combined with an XEvent trace. *CSS (in direct conflict?): "Disables loading of dbghelp so the stack should not be put in the error log if 
this is enabled". Since their post doesn’t have an example and the SQLCAT post displays output, CSS appears to be wrong here.*
[SQLCAT](http://blogs.msdn.com/b/sqlcat/archive/2010/05/11/resolving-dtc-related-waits-and-tuning-scalability-of-dtc.aspx) | 
[CSS](http://blogs.msdn.com/b/psssql/archive/2009/11/17/how-it-works-controlling-sql-server-memory-dumps.aspx) | 
[PRand](http://www.sqlskills.com/blogs/paul/determine-causes-particular-wait-type/) | 
[PRand](http://www.sqlskills.com/blogs/paul/sos_scheduler_yield-waits-and-the-lock_hash-spinlock/) | 
[JKeh](http://sqlblog.com/blogs/jonathan_kehayias/archive/2010/12/24/an-xevent-a-day-24-of-31-what-is-the-callstack-Action.aspx)


**6527** **Doc2008** BOL: "Disables generation of a memory dump on the first occurrence of an out-of-memory exception in CLR integration. By default, 
SQL Server generates a small memory dump on the first occurrence of an out-of-memory exception in the CLR. The behavior of the trace flag is as follows:
- If this is used as a startup trace flag, a memory dump is never generated. However, a memory dump may be generated if other trace flags are used.
- If this trace flag is enabled on a running server, a memory dump will not be automatically generated from that point on. However, if a memory dump 
has already been generated due to an out-of-memory exception in the CLR, this trace flag will have no effect. Scope: global only"


**8024** Adds conditions to the mini-dump generation logic for the 1788* errors: "To capture a mini-dump, one of the following checks must also be met.
- The non-yielding workers CPU utilization must be > 40 percent.
- The SQL Server process is not starved for overall CPU resource utilization.
Additional check #1 is targeted at runaway CPU users. Additional check #2 is targeted at workers with lower utilizations that are probably stuck in an 
API call or similar activity."	[DiagCorrect17883etc](http://msdn.microsoft.com/en-us/library/cc917684.aspx)



## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.
