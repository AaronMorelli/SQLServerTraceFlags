# Statistics and Estimation

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 8666 (CompInfo)



## Cardinality Estimation Behavior

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 2301 | (more below) Causes the old CE to use a number of different core mathematical assumptions during estimation. | | 
| 2312 | Forces the use of the new CE. | [2801413](http://support.microsoft.com/kb/2801413); [Nevarez](http://www.sqlperformance.com/2013/12/t-sql-queries/a-first-look-at-the-new-sql-server-cardinality-estimator) | 
| 2324 | Disables the creation of "implied predicates" | [SQLPerformance](https://answers.sqlperformance.com/questions/2299/why-not-seek-predicate.html) |
| 2328 | Reverts back to SQL 2000 behavior when comparing 2 constants ("constants" here includes parameters or variables). Dima: demonstrates 2328 leading to a selectivity "guess" (calling CScaop_Comp::GuessSelect) | [IJose](http://blogs.msdn.com/b/ianjo/archive/2006/03/28/563419.aspx); [Dima](http://www.somewheresomehow.ru/isnumeric_ce_bug_eng/) | 
| 2327 | Switches the threshold for a performance-based recompile (for a given stats object) from the default of 20%+500 rows to SQRT(1000*<num rows in table>). Now the default in SQL 2016. | [2754171](http://support.microsoft.com/kb/2754171); [SRGolla](http://blogs.msdn.com/b/srgolla/archive/2012/09/04/sql-server-statistics-explained.aspx); [SAPonSQLServer](http://blogs.msdn.com/b/saponsqlserver/archive/2011/09/07/changes-to-automatic-update-statistics-in-sql-server-traceflag-2371.aspx) | 
| 2389 | (more below) Cause optimizer to look for ascending key nature of stat objects | | 
| 2390 | (more below) Closely tied to 2389 and the ascending key problem. Read Ian Jose’s blog entry carefully before using 2390. | | 
| 2453 | Applies familiar temp table rowcount recompilation thresholds to table variables. Bertrand points out important caveats. | [2952444](http://support.microsoft.com/kb/2952444); [2012SP2](http://support.microsoft.com/kb/2958429); [CSS](http://blogs.msdn.com/b/psssql/archive/2014/08/11/if-you-have-queries-that-use-table-variables-sql-server-2012-sp2-can-help.aspx); [Bertrand](http://sqlperformance.com/2014/06/t-sql-queries/table-variable-perf-fix) | 
| 4137 | Modifies estimation for multiple conjunctive (AND) predicates. PWhite covers "Minimum Selectivity" well, and also notes that 9471 replaces this flag in the new CE. | [2658214](http://support.microsoft.com/kb/2658214); [PWhite](http://www.sqlperformance.com/2014/01/sql-plan/cardinality-estimation-for-multiple-predicates); [Dima](http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/801908/sql-server-2014-cardinality-estimation-regression) | 
| 4138 | Disables "Row Goal" logic in the optimizer. The 2 interesting StackExch links show Martin Smith at work. | [2667211](https://support.microsoft.com/en-us/kb/2667211); [PWhite](https://answers.sqlperformance.com/questions/1609/trying-to-figure-out-how-to-resolve-the-data-skew.html); [StackExch](http://dba.stackexchange.com/questions/55198/huge-slowdown-to-sql-server-query-on-adding-wildcard-or-top); [StackExch](http://dba.stackexchange.com/questions/57018/why-is-sql-server-using-a-clustered-index-scan-for-a-self-referencing-fk-cascade); [PWhite](http://sqlperformance.com/2015/07/sql-plan/locking-and-performance) | 
| 9471 | Enables "Minimum Selectivity" cardinality estimation behavior for SQL 2014 under the new CE. Note that, unlike 4137 (which only applied "Minimum Selectivity" for conjunctive predicates), 9471 applies it to both conjunctive and disjunctive predicates. | [PWhite](http://www.sqlperformance.com/2014/01/sql-plan/cardinality-estimation-for-multiple-predicates); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/801908/sql-server-2014-cardinality-estimation-regression); [Milo](http://milossql.wordpress.com/2014/01/02/new-cardinality-estimator-part-4-single-table-and-multiple-predicates/); | 
| 9472 | Assumes independence for multiple WHERE predicates in the SQL 2014 CE. Predicate independence was the default before SQL 2014, and thus this flag can be used to emulate pre-SQL 2014 CE behavior in a more specific fashion than TF 9481. | [PWhite](http://www.sqlperformance.com/2014/01/sql-plan/cardinality-estimation-for-multiple-predicates); [Connect](http://connect.microsoft.com/SQLServer/feedback/details/801908/sql-server-2014-cardinality-estimation-regression) | 
| 9476 | Forces new CE to use simple containment (old CE default) instead of base containment (new CE default). (Also, per Paul White: "ignores the histograms (avoiding coarse alignment) and simply assumes containment at the join.") | [3189675](https://support.microsoft.com/en-us/kb/3189675); [Dima](http://www.queryprocessor.com/ce_join_base_containment_assumption/) | 
| 9479 | Dima: "forces the optimizer to use Simple Join [estimation] even if a histogram is available…will force optimizer to use a simple join estimation algorithm, it may be CSelCalcSimpleJoinWithDistinctCounts, CSelCalcSimpleJoin or CSelCalcSimpleJoinWithUpperBound, depending on the compatibility level and predicate comparison type." (Paul White: “uses simple containment instead of base containment for simple joins”) | [Dima](http://www.queryprocessor.com/join-estimation-internals/) | 
| 9481 | **Doc2014** Forces the 2014 (and newer) optimizers to use the old CE model. | [2801413](http://support.microsoft.com/kb/2801413) | 
| 9482 | Turns off the "overpopulated primary key" adjustment that the optimizer might use when determining that a "dimension" table (the schema could be OLTP as well) has many more distinct values than the "fact" table. (The seminal example is where a Date dimension is populated out into the future, but the fact table only has rows up to the current date). Since join cardinality estimation occurs based on the contents of the histograms of the joined columns, an "overpopulated primary key" can result in higher selectivity estimates, causing rowcount estimates to be too low. | [Dima](http://www.queryprocessor.com/ce_opk/) | 
| 9483 | Forces the optimizer to create (if possible) a filtered statistics object based on a predicate in the query. This filtered stat object is not persisted. In Dima’s example, the filtered stat object is actually created on the join column…i.e. "CREATE STATISTICS [filtered stat obj] ON [table] (Join column) WHERE (predicate column = 'literal')" | [Dima](http://www.queryprocessor.com/ce_filteredstats/) | 
| 9488 | Reverts the estimation behavior for multi-statement TVFs back to 1 row (instead of the 100-row estimate behavior that was adopted in SQL 2014). | [Dima](http://www.queryprocessor.com/ce_mtvf/) | 
| 9489 | Turns off the new CE logic that handles ascending keys.  | [Dima](http://www.queryprocessor.com/ce_asckey/) | 





**2301** (Note that the online descriptions of 2301 describe behavioral changes in the optimization process that are primarily tied to cardinality estimation. 
However, it is possible that this TF enables other, non-cardinality-related behavioral changes). KB: "Makes your optimizer work harder by enabling advanced 
optimizations that are specific to decision support queries, applies to processing of large data sets."
Database-Wiki: "Trace flag 2301 affects costing, which affects plan choice. Trace flag 2301 discourages order-preserving parallelism. The most significant 
effect is that trace flag 2301 discourages parallel merge join. Trace flag 2301 enables the following advanced logic in the cardinality estimation: 
- Base containment assumption
- Integer-based interpolation
- Use of the histogram even when the cardinality estimate is low
- Unlimited density remapping  
When a potential cardinality estimation issue exists, enable trace flag 2301 to determine whether the issue is addressed by the logic that is enabled by this trace flag. 
When you should not use trace flag 2301:
The potential disadvantage of this trace flag is that it uses more time and more memory during optimization. Do not use this trace flag for 
OLTP queries and for frequently compiled queries. One known issue is that if applications perform many column remapping functions 
(such as CONVERT, CAST, UPPER, or LOWER) and have many densities, enabling trace flag 2301 consumes lots of memory.."
[920093](http://support.microsoft.com/kb/920093) 
| [IJose](http://blogs.msdn.com/b/ianjo/archive/2006/04/24/582219.aspx) 
| [Dima](http://www.queryprocessor.com/ce_join_base_containment_assumption/) 
| [Connect](https://connect.microsoft.com/SQLServer/feedback/details/772232/make-optimizer-estimations-more-accurate-by-using-metadata)


**2389** (See also 2388, 2390, and 4139 in this document) Causes the optimizer to observe the max value in the lead column of a stat object. After 3 consecutive occurrences of a larger-than-last-time value, 
the statistic is branded as "Ascending". Itzik has very insightful comments about this flag and the ascending key problem under both the old and new CE.
Kejser notes that (at the time of his posts) this TF doesn’t work with partitioned tables, and has his own solution.
[Itzik](http://sqlmag.com/sql-server/seek-and-you-shall-scan-part-ii-ascending-keys) 
| [IJose](http://blogs.msdn.com/b/ianjo/archive/2006/04/24/582227.aspx)
| [Stellato](http://sqlperformance.com/2016/07/sql-statistics/trace-flag-2389-new-cardinality-estimator)
| [TKejser Part 1](http://blog.kejser.org/2011/07/01/the-ascending-key-problem-in-fact-tables-part-one-pain/)
| [TKejser Part 2](http://blog.kejser.org/2011/07/07/the-ascending-column-problem-in-fact-tables-part-two-stat-job/)
| [Nevarez](http://www.benjaminnevarez.com/2013/02/statistics-on-ascending-keys/)
| Hotfixes: [922063](http://support.microsoft.com/?kbid=922063), [929278](http://support.microsoft.com/?kbid=929278), and [2952101](http://support.microsoft.com/kb/2952101)


**2390** Closely tied to 2389 and the ascending key problem. Read Ian Jose’s blog entry carefully before using 2390.
[IJose](http://blogs.msdn.com/b/ianjo/archive/2006/04/24/582227.aspx) 
| [2952101](http://support.microsoft.com/kb/2952101) (enabled by TF 4139) fixes some limitations of this flag in SQL 2012 and 2014.




## Other 

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 2309 | In SQL 2014, enables output from a 3rd parameter for DBCC SHOW_STATISTICS such that the partial statistics histogram (for just one partition) is shown. | [ErinStel](http://sqlperformance.com/2015/05/sql-statistics/incremental-statistics-are-not-used-by-the-query-optimizer); [DBI-Services](http://www.dbi-services.com/index.php/blog/entry/sql-server-2014-new-incremental-statistics) | 
| 2363 | (more below) prints info re: statistics and estimations in the new CE optimization process | | 
| 2388 | (more below) Changes DBCC SHOW_STATISTICS to show a single row that is focused on whether the lead col of the stat object is ascending or not. | | 
| 7471 | Allows multiple statistics objects on the same table to be updated concurrently. | [3156157](https://support.microsoft.com/en-us/kb/3156157); [TigerTeam](https://blogs.msdn.microsoft.com/sql_server_team/boosting-update-statistics-performance-with-sql-2014-sp1cu6/) | 
| 8721 | KB: "dump[s] information into the error log when AutoStat has been run." The KB also describes locks taken by autostats. | [195565](http://support.microsoft.com/kb/195565/en-us); [SQLSasquatch](http://sql-sasquatch.blogspot.com/2016/06/sql-server-trace-flag-8721-auto-stats.html) | 
| 9204 | PWhite: for the old CE, gives "the "interesting" statistics which end up being fully loaded and used to produce cardinality and distribution estimates for some plan alternative or other." | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2011/09/21/how-to-find-the-statistics-used-to-compile-an-execution-plan.aspx) | 
| 9292 | PWhite: for the old CE, gives "a report of statistics objects which are considered 'interesting' by the query optimizer when compiling, or recompiling the query in question. For [these] potentially useful statistics, just the header is loaded." | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2011/09/21/how-to-find-the-statistics-used-to-compile-an-execution-plan.aspx) | 
| 9348 |  | | 
| | | | 
| | | | 


**2363** New in SQL 2014 and outputs information regarding statistics information used and derived during the (new CE) optimization process.
Dima shows how 2363 gives insight into the new "calculator" framework in 2014 for deriving statistics at intermediate nodes of the tree. 
It also shows which histograms (statistics objects) were loaded, and acts as a replacement for 9204 and 9292, which do not appear to work under the new CE.
[Dima](http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/) 
| [PWhite](http://www.sqlperformance.com/2014/01/sql-plan/cardinality-estimation-for-multiple-predicates)


**2388** Changes the output of DBCC SHOW_STATISTICS. Instead of the normal Header/Vector/Histogram output, instead we get a single row that gives information 
related to whether the lead column of the stat object is considered to be ascending or not. This TF is primarily helpful in watching the state of a stat object 
change from  "Unknown", to "Ascending" (and potentially to "Stationary").
[Nevarez](http://www.benjaminnevarez.com/2013/02/statistics-on-ascending-keys/) 
| [SQLSasquatch](http://sql-sasquatch.blogspot.ca/2014/03/in-search-of-sqlserver-stats.html) 
| [Stellato](http://sqlperformance.com/2016/07/sql-statistics/trace-flag-2389-new-cardinality-estimator)



## Limited Lifespan

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 4139 | (related to 2389) Enables a CU fix where if 90% of newly-inserted values were NOT higher than the highest key value, the column would not be marked as ascending. 3192117 notes this flag can cause access violations on certain SQL 2016 builds. | [2952101](http://support.microsoft.com/kb/2952101); [3192117](https://support.microsoft.com/en-us/kb/3192117) | 
| | | | 
| | | | 
| | | | 
| | | | 
