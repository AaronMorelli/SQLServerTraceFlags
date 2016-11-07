# Query Execution

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 

maybe I should move Query Syntax/Rules into this section


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 646 | Prints (to SQL error log) which segments were eliminated at runtime by columnstore segment elimination. Requires 3605. | [Technet](http://social.technet.microsoft.com/wiki/contents/articles/5611.verifying-columnstore-segment-elimination.aspx); [Niko](http://www.nikoport.com/2014/07/24/clustered-columnstore-indexes-part-35-trace-flags-query-optimiser-rules/); [Niko](http://www.nikoport.com/2015/05/24/azure-columnstore-part-3-modern-segment-elimination-and-set-statistics-io/); [JSack](http://www.sqlskills.com/blogs/joe/exploring-columnstore-index-metadata-segment-distribution-and-elimination-behaviors/) | 
| 1504 | Prints to the console (w/3604) or the error log (w/3605; required for parallel index builds) when an index DDL command requires more memory to be granted to continue sorting rows in memory. | [PWhite](http://sqlperformance.com/2015/04/sql-plan/internals-of-the-seven-sql-server-sorts-part-1) | 
| 2466 | When optimizer is choosing the runtime DOP for a parallel plan, this directs it to use logic found in "older versions" (the post doesn’t say which versions) to determine which NUMA node to place the parallel plan on. This older logic relies on a polling mechanism (roughly every 1 second), and can result in race conditions where 2 parallel plans end up on the same node. The newer logic "significantly reduces" the likelihood of this happening. | [CSS](http://blogs.msdn.com/b/psssql/archive/2013/09/27/how-it-works-maximizing-max-degree-of-parallelism-maxdop.aspx) | 
| 2467 | "If target MAXDOP target is less than a single node can provide and if trace flag 2467 is enabled attempt to locate least loaded node" | [CSS](http://blogs.msdn.com/b/psssql/archive/2013/09/27/how-it-works-maximizing-max-degree-of-parallelism-maxdop.aspx); [CSS](https://blogs.msdn.microsoft.com/psssql/2016/03/04/sql-server-parallel-query-placement-decision-logic/) | 
| 2468 | "Find the next node that can service the DOP request. Unlike full mode, the global, resource manager keeps track of the last node used. Starting from the last position, and moving to the next node, SQL Server checks for query placement opportunities. If a node can’t support the request SQL Server continues advancing nodes and searching." | [CSS](https://blogs.msdn.microsoft.com/psssql/2016/03/04/sql-server-parallel-query-placement-decision-logic/) | 
| 2479 | When SQL Server is determining the runtime DOP for a parallel plan, this flag directs it to limit the NUMA Node placement for the query to the node that the connection is associated with. | [CSS](http://blogs.msdn.com/b/psssql/archive/2013/09/27/how-it-works-maximizing-max-degree-of-parallelism-maxdop.aspx); [CSS](https://blogs.msdn.microsoft.com/psssql/2016/03/04/sql-server-parallel-query-placement-decision-logic/) | 
| 2486 | In SQL 2016 (CTP 3.0 at least), enables output for the "query_trace_column_values" Extended Event, allowing the value of output columns from individual plan iterators to be traced. | [Dima](http://www.queryprocessor.com/query-trace-column-values/) | 
| 3640 | Eliminates sending DONE_IN_PROC messages to client for each statement in stored procedure. This is similar to the session setting of SET NOCOUNT ON, but when set as a trace flag, every client session is handled this way. | [MSDNblog](http://blogs.msdn.com/b/selvar/archive/2010/07/14/delete-operation-editing-a-data-source-from-a-reporting-service-2005-report-manager-fails-internalcatalogexception-and-throwing-watson-dump.aspx) | 
| 6530 | Enables a fix for poor performance when building an index on a spatial data type | [2896720](http://support.microsoft.com/kb/2896720/en-us) | 
| 7525 | Affects when cursors are closed upon ROLLBACK. This flag reverts to SQL 7.0 RTM behavior. Unsure of whether this is still applicable to modern versions. | [199294](https://support.microsoft.com/en-us/kb/199294/en-us) | 
| 9389 | **Doc2014** BOL: "Enables dynamic memory grant for batch mode operators...If the dynamic memory grant trace flag is enabled, a batch mode operator may ask for additional memory and avoid spilling to tempdb if additional memory is available." | | 



Here's the content from the Query Syntax/Rules section
# Query Syntax/Rules

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 


## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| | | | 
| | | | 
| | | | 
| | | | 
| | | | 
| | | | 


## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 107 | Enabled the insertion of '0 into columns of type float, decimal, numeric, or real. | [160732](https://support.microsoft.com/en-us/kb/160732) | 
| 262 | Enables a fix in SQL 7.0 to correct a problem with strings w/trailing spaces being truncated even when ANSI PADDING was on. | [891116](https://support.microsoft.com/en-us/kb/891116) | 
| | | | 
| | | | 
| | | | 
| | | | 



TODO: move the following to the "unable to confirm" section
237	Tells SQL Server to use correlated sub-queries in Non-ANSI standard backward compatibility mode	
242	Provides backward compatibility for correlated subqueries where non-ANSI-standard results are desired.	
243	Provides backward compatibility for nullability behavior. When set, SQL Server has the same nullability violation behavior as that of a ver 4.2: 
a)	Processing of the entire batch is terminated if the nullability error (inserting NULL into a NOT NULL field) can be detected at compile time. 
b)	Processing of offending row is skipped, but the command continues if the nullability violation is detected at run time. 
Behavior of SQL Server is now more consistent because nullability checks are made at run time and a nullability violation results in the command terminating 
and the batch or transaction process continuing.	
244	Disables checking for allowed interim constraint violations. By default, SQL Server checks for and allows interim constraint violations. 
An interim constraint violation is caused by a change that removes the violation such that the constraint is met, all within a single statement and transaction. 
SQL Server checks for interim constraint violations for self-referencing DELETE statements, INSERT, and multi-row UPDATE statements. This checking requires more 
work tables. With this trace flag you can disallow interim constraint violations, thus requiring fewer work tables.	
506	Enforces SQL-92 standards regarding null values for comparisons between variables and parameters. Any comparison of variables and parameters that contain a NULL always results in a NULL.	
8783	Allows DELETE, INSERT, and UPDATE statements to honor the SET ROWCOUNT ON setting when enabled.	
8816	Logs every two-digit year conversion to a four-digit year.	
