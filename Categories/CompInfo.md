# Compilation (Informational)

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 


## Trees and Memos

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 7352 | The final query tree after Post-optimization re-write. | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/08/31/sql-server-internals-nested-loops-prefetching.aspx); [Dima](http://www.queryprocessor.com/batch-sort-and-nested-loops/); [Dima](http://www.queryprocessor.com/few-outer-rows-optimization/) | 
| 8605 | The initial logical tree (the input into query optimization). (Paul also calls this the “converted tree” in Part 4 of his series) | [Nevarez](http://www.benjaminnevarez.com/2012/04/more-undocumented-query-optimizer-trace-flags/); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/04/28/query-optimizer-deep-dive-part-1.aspx) | 
| 8606 | Displays additional logical trees, including the Input Tree, the Simplified Tree, the Join-collapsed Tree, the "Tree before Project Normalization", and the "Tree after Project Normalization" | [Nevarez](http://www.benjaminnevarez.com/2012/04/more-undocumented-query-optimizer-trace-flags/); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/04/28/query-optimizer-deep-dive-part-2.aspx); [Dima](http://www.somewheresomehow.ru/optimizer-part-1-simplification/) | 
| 8607 | (links below) Displays the optimization output tree, before Post-optimization rewrite. Has a "Query marked as cachable" note if the plan can be cached. |  | 
| 8608 | Shows the initial Memo structure | [Nevarez](http://www.benjaminnevarez.com/2012/04/inside-the-query-optimizer-memo-structure/); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/04/29/query-optimizer-deep-dive-part-3.aspx); [Dima](http://www.somewheresomehow.ru/optimizer-part-3-full-optimiztion-optimization-search0/); [Dima](http://www.somewheresomehow.ru/optimizer-part-4-optimization-full-optimization-search1/) | 
| 8612 | Dima: adds cardinality info to the various trees produced by flags 8605, 8606, and 8607. | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/06/11/hello-operator-my-switch-is-bored.aspx); [Dima](http://www.somewheresomehow.ru/optimizer-part-3-full-optimiztion-optimization-search0/); [Dima](http://www.somewheresomehow.ru/rowgoal-on-non-uniform-distribution/); [Dima](http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/) | 
| 8615 | Show the final Memo structure | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/04/29/query-optimizer-deep-dive-part-3.aspx); [Dima](http://www.somewheresomehow.ru/optimizer-part-3-full-optimiztion-optimization-search0/); [Dima](http://www.somewheresomehow.ru/optimizer-part-4-optimization-full-optimization-search1/); [Dima](http://www.somewheresomehow.ru/isnumeric_ce_bug_eng/) | 
| 8620 | PWhite: "Add memo arguments to 8619" | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx) | 
| 8621 | PWhite: "Rule with resulting tree" (use with 8619) | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx) | 


**8607** Links: 
[Nevarez](http://www.benjaminnevarez.com/2012/04/more-undocumented-query-optimizer-trace-flags/) 
| [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/04/29/query-optimizer-deep-dive-part-3.aspx) 
| [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/01/26/optimizing-t-sql-queries-that-change-data.aspx) 
| [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/02/01/a-creative-use-of-ignore-dup-key.aspx) 
| [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/06/11/hello-operator-my-switch-is-bored.aspx) 
| [PWhite](http://www.sqlperformance.com/2013/07/sql-plan/working-around-missed-optimizations) 
| [Dima](http://www.somewheresomehow.ru/optimizer-part-3-full-optimiztion-optimization-search0/) 
| [Dima](http://www.queryprocessor.com/few-outer-rows-optimization/)




## Phases and Rules 

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 2372 | Nevarez: "shows memory utilization during the different optimization stages." | [Nevarez](http://www.benjaminnevarez.com/2012/04/more-undocumented-query-optimizer-trace-flags/); [Dima](http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/) | 
| 2373 | Shows memory utilization before and after various optimizer rules are applied (e.g. IJtoIJsel). Provides a way to "trace" what rules are used when optimizing a query. | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx); [Dima](http://www.somewheresomehow.ru/optimizer-part-2-trivial-plan-optimization/); [Dima](http://www.somewheresomehow.ru/isnumeric_ce_bug_eng/); [Dima](http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/) | 
| 8609 | PWhite: "Task and operation type counts" | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx); [Dima](http://www.somewheresomehow.ru/good-enough-plan/) | 
| 8619 | PWhite: "Apply rule with description"; Dima: "Show Applied Transformation Rules" | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/02/06/incorrect-results-with-indexed-views.aspx); [Dima](http://www.somewheresomehow.ru/cardinality-estimation-framework-2014-first-look/) | 
| 8628 | When used with 8666 (see below), causes extra information about the transformation rules applied to be put into the XML showplan. | [Dima](http://www.queryprocessor.com/tf_8628/); [PWhite](http://sqlperformance.com/2015/04/sql-plan/internals-of-the-seven-sql-server-sorts-part-1) | 
| 8675 | Display query optimization phases, along with info (timing, costs, etc) about each phase. | [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/04/29/query-optimizer-deep-dive-part-3.aspx); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2013/06/17/improving-partitioned-table-join-performance.aspx); [PWhite](http://www.sqlperformance.com/2013/06/sql-indexes/recognizing-missed-optimizations); [PWhite](http://www.sqlperformance.com/2013/07/sql-plan/working-around-missed-optimizations) | 
| 8739 | Dima: "Group Optimization Info" | [Dima](http://www.somewheresomehow.ru/good-enough-plan/) | 



## General Informational

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 445 | Full functionality unknown. Prints “Compile issued:” and then the text of the sql statement being compiled to the SQL error log. Personally confirmed that this still works in SQL 2014 even though it appears to be a very old trace flag. Discovered via SQLService.se | | 
| 2315 | Aaron: personally discovered. Seems to output memory allocations taken during the compilation process (and maybe the plan as well? "PROCHDR"), as well as memory broker states & values at the beginning and end of compilation. | | 
| 2318 | Aaron: personally discovered. I’ve only seen one type of output so far: "Optimization Stage:  HEURISTICJOINREORDER". Maybe useful in combo with other compilation trace flags to see the timing of join reordering? | | 
| 2336 | Aaron: personally discovered. Appears to tie memory info and cached page likelihoods with costing. | | 
| 2398 | Aaron: personally discovered. Outputs info about "Smart Seek costing": e.g.: "Smart seek costing (75.2) :: 1.34078e+154 , 1". | | 
| 7357 | Info re: hash operators, including role reversal, recursion levels, Unique Hash optimization usage, hash-related bitmap, etc. For parallel query plans, 7357 does NOT send output to the console window. However, output to the SQL Server error log can be enabled by enabling 3605. | [Dima](http://www.queryprocessor.com/hash-join-execution-internals/); [PWhite](http://sqlblog.com/blogs/paul_white/archive/2012/05/01/query-optimizer-deep-dive-part-4.aspx) | 
| 8666 | Causes some useful info (including stat object thresholds) already present in the internal representation of a plan to be included in the XML plan output. | [Fabiano](http://blogfabiano.com/2012/07/03/statistics-used-in-a-cached-query-plan/); [DBally](http://dataidol.com/davebally/2014/04/12/reasons-why-your-plans-suck-no-56536/); [PWhite](http://sqlperformance.com/2015/12/sql-plan/optimizing-update-queries); [PWhite](http://sqlperformance.com/2016/03/sql-plan/changes-to-a-writable-partition-may-fail) | 
| 8719 | In SQL 2000, apparently would show IO prefetch on loop joins and bookmarks. I (Aaron) was unable to replicate the query plan behavior on SQL 2012 using the same test, so this flag may be obsolete. | [Hanlincrest](http://www.hanlincrest.com/SQLServerLockEscalation.htm) | 


