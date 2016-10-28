# SQLOS Scheduling and CPU-related

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

*See also:* Several trace flags affect or control memory dump behavior that is specific to SQLOS scheduling. These flags have been placed in the [Exceptions and Memory Dump Behavior](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/ExceptMemDump.md)
section: 1260, 1262, 1264, 8024. Also, note that TFs 2466, 2467, and 2479 (placed in the [Query Execution](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/QueryExec.md) section) all adjust how the runtime DOP is calculated for a parallel plan, and affect different parts of a formula that is inherently focused on the number of free workers on the NUMA nodes on a system.

The longer descriptions and larger number of links make a table format impractical.

TODO: ensure 6531 and 6533 are in the see also of query execution


**839** [CSS](http://blogs.msdn.com/b/psssql/archive/2010/04/02/how-it-works-soft-numa-i-o-completion-thread-lazy-writer-workers-and-memory-nodes.aspx): 
(Apparently) forces SQL Server to treate all NUMA memory as "flat", as if it was SMP.

**1531** [Ewald](http://sqlonice.com/anatomy-and-psychology-of-a-sqlos_spinlock/) speculates that it may be involved in writing certain Spinlock events to a ring buffer.

**1615** Khen2005, page 385 (paraphrased): directs SQL to use threads instead of fiber even if the "lightweight pooling" config option is on. (Apparently, sometimes SQL wouldn’t start successfully when using lightweight pooling, and so this lets you get SQL up and running, so that you can turn the config option off)	

**3601** [MSBlog](http://blogs.msdn.com/b/sqlserverfaq/archive/2009/05/27/info-sql-2000-msde-installation-will-fail-if-you-have-number-of-cpus-on-a-box-which-is-not-in-power-of-2.aspx): 
Appears to disable CPU instruction prefetching. The Blog post to the right uses it, in concert with 3603, to enable SQL 2000 to run on a machine with a # of processors that is *not* a power of 2.

**3603** [MSBlog](http://blogs.msdn.com/b/sqlserverfaq/archive/2009/05/27/info-sql-2000-msde-installation-will-fail-if-you-have-number-of-cpus-on-a-box-which-is-not-in-power-of-2.aspx): 
Disables “Simultaneous Multithreading Processor check”. Used in concern with 3601 in the blog post to the right to enable SQL 2000 to run on a machine with a # of processors that is *not* a power of 2.

**6531** Enables adjustment in the SQLOS scheduling layer to handle queries that issue many short-duration calls to spatial data (which is implemented via CLR functions).
[3005300](http://support.microsoft.com/kb/3005300): "This fix introduces the trace flag 6531 to indicate to the SQLOS hosting layer that the spatial data type should avoid 
preemptive protections. This can reduce the CPU consumption and improve the overall performance for spatial activities. Only use this trace flag if the individual, spatial 
method invocations (per row and column) take less than ~4ms. Longer invocations without preemptive protection could lead to scheduler concurrency issues and SQLCLR 
punishment messages logged to the error log."

**6533** [3107399](https://support.microsoft.com/en-us/kb/3107399): "… improves performance of query operations with spatial data types in SQL Server 2012 and 2014. The 
performance gain will vary, depending on the configuration, the types of queries, and the objects." Note that [3132545](https://support.microsoft.com/en-us/kb/3132545) warns 
against using the STRelate and STAsBinary functions when TF 6533 is on, as those functions can return unexpected results. 	

**8002** Kalen2008, p24: if you specify a CPU mask because you want to limit the # of CPUs your instance can use (e.g. say you have multiple instances and you want to 
partition the processing on the box), but you still want your schedulers to be able to move between their affinitized CPUs, turn this flag on.
Note that the first 2 KB articles to the right are practically identical. Also, note that some of the KB articles are written for SQL 2000.
[818765](http://support.microsoft.com/default.aspx?scid=kb;en-us;818765) 
| [818769](http://support.microsoft.com/kb/818769) 
| [CSS](http://blogs.msdn.com/b/psssql/archive/2011/11/11/sql-server-clarifying-the-numa-configuration-information.aspx) 
| [921928](http://support.microsoft.com/kb/921928/en-us)

**8008** Khen2005, p383 (paraphrased): sessions normally store the last scheduler that they ran on, and pass this as a hint to SQLOS to encourage SQLOS to 
schedule the session’s work on the same scheduler as last time (to reuse CPU cache). In some cases, this can cause problems, so TF 8008 forces SQLOS to ignore 
this hint and always put new work on the least-loaded scheduler. [CAdkin](http://chrisadkin.org/2015/04/14/well-known-and-not-so-well-known-sql-server-tuning-knobs-and-switches/) 
has some valuable screenshots showing a much more evenly-balanced distribution of work across CPUs when this flag is on. 
[CSS](http://blogs.msdn.com/b/psssql/archive/2013/08/13/how-it-works-sql-server-2012-database-engine-task-scheduling.aspx) 
| [Forum Q](http://www.stillhq.com/sqldownunder/archives/msg05089.html)

**8012** [920093](http://support.microsoft.com/kb/920093): "SQL Server records an event in the schedule ring buffer every time that one of the following events occurs: 
-	A scheduler switches context to another worker. 
-	A worker is suspended or resumed. 
-	A worker enters the preemptive mode or the non-preemptive mode. 
You can use the diagnostic information in this ring buffer to analyze scheduling problems. For example, you can use the information in this 
ring buffer to troubleshoot problems when SQL Server stops responding. Trace flag 8012 disables recording of events for schedulers." Scope Startup only.

**8015** Have SQL [on a NUMA box] pretend it is SMP. Startup flag only. *According to the first CSS link on the right, TF 839 does the same thing (or something similar). 
Not sure why the duplication, perhaps 839 is for SQL 2000?*	
[CSS](http://blogs.msdn.com/b/psssql/archive/2010/04/02/how-it-works-soft-numa-i-o-completion-thread-lazy-writer-workers-and-memory-nodes.aspx) 
| [CSS](http://blogs.msdn.com/b/psssql/archive/2008/09/05/sql-server-2005-setup-fails-in-wow-x86-on-computer-with-more-than-32-cpus.aspx?Redirected=true) 
| [CSS](http://blogs.msdn.com/b/psssql/archive/2012/12/13/how-it-works-sql-server-numa-local-foreign-and-away-memory-blocks.aspx) (comments)
| [948450](http://support.microsoft.com/kb/948450) 
| [2813214](http://support.microsoft.com/kb/2813214/en-us) 


**8016** [CSS](http://blogs.msdn.com/b/psssql/archive/2013/08/13/how-it-works-sql-server-2012-database-engine-task-scheduling.aspx): 
Force new tasks to always be assigned to the preferred scheduler for a connection.

**8017** Khen2005, page 387 (paraphrased): basically means "no offline schedulers". Normally, when using affinity to restrict the CPUs that SQL can use, 
SQLOS starts up schedulers for every CPU on the box, but then keeps schedulers that it is not allowed to use in an offline state. However, those schedulers 
are using resources, so you can prevent SQL from ever creating those schedulers by turning on this flag. You can combine this with 8002 to achieve the 
"move among CPUs" effect for your schedulers. This flag appears to have been turned on by default in SQL 2005 Express Edition, as evidenced by all of the 
upgrade warnings people were experiencing when trying to upgrade to SQL 2008 Express. 
[PWhite](http://dba.stackexchange.com/questions/48580/trace-flag-and-which-need-to-be-turned-off-and-why)

**8021** Khen2005, p404: Sameer’s wording isn’t totally clear to me, but this flag appears to override default behavior where SQL could start up in SMP 
mode even on a NUMA box. The default logic (at the time of this writing, at least) was to check the # of CPUs in each NUMA node. If each NUMA node had only 
1 CPU (this was true of some early NUMA architectures), and there were 4 or fewer total processors on the machine, SQL would start up as if it were running on an 
SMP architecture. This flag would override that behavior and cause SQL to start up in NUMA node. 
[CSS](http://blogs.msdn.com/b/psssql/archive/2011/11/11/sql-server-clarifying-the-numa-configuration-information.aspx)

**8022** **Info** Khen2005, p399: [warning: this TF can produce  a lot of information] This flag gives more information about the conditions when a 
non-yielding scheduler/situation was encountered. The whitepaper linked to on the right gives example output for this flag	
[DiagCorrect17883etc](http://msdn.microsoft.com/en-us/library/cc917684.aspx)

**8025** Khen2005, p405: SQL on NUMA normally does most of its allocation on Node 1, because usually Windows and other programs will allocate from Node 0. 
However, if you want SQL to do its resource allocation on the default node (node 0), turn on this flag.
Bob Ward notes in his SQL 2012 memory talk at PASS 2013 that SQL no longer does this “allocate from Node 1” logic anymore, as Windows has corrected its 
problems with imbalance in its allocations.	[CSS](http://blogs.msdn.com/b/psssql/archive/2011/11/11/sql-server-clarifying-the-numa-configuration-information.aspx)

**8033** Suppresses messages of the form “The time stamp counter of CPU on scheduler id 1 is not synchronized with other CPUs” from being placed in the SQL Error 
log when CPU drift is noticed.
[Skorlinski](http://blogs.msdn.com/b/chrissk/archive/2008/06/19/i-o-requests-taking-longer-than-15-seconds-to-complete-on-file.aspx)
| [931279](http://support.microsoft.com/kb/931279/en-us) 
| [CSS](http://blogs.msdn.com/b/psssql/archive/2007/08/19/sql-server-2005-rdtsc-truths-and-myths-discussed.aspx)

**8038** [972767](http://support.microsoft.com/kb/972767): "The SQL Server database engine and SQL Server Reporting Services both use a shared component 
called SQLOS. SQLOS exposes an internal timer. When the internal timer is set to a 1ms granularity, more power consumption than desired may occur on 
Windows client computers... After you apply this hotfix, SQLOS will not use the 1ms granularity for the internal timer as the default... 
this trace flag will also affect the granularity of some diagnostics, such as dynamic management views... If you want to use the 1ms timer even after 
you apply this CU package, or after you upgrade to later builds and releases of SQL Server that contain this change, you can enable trace flag 8049 
at startup to force the use of the 1ms timer." Several vendor links are relevant (describing problems related to Windows timers).
[972767](http://support.microsoft.com/kb/972767) 
| [2022911](http://support.microsoft.com/kb/2022911/en-us) 
| [CSS](http://blogs.msdn.com/b/psssql/archive/2010/08/18/how-it-works-timer-outputs-in-sql-server-2008-r2-invariant-tsc.aspx)
| [CSS](http://blogs.msdn.com/b/psssql/archive/2009/05/29/how-it-works-sql-server-timings-and-timer-output-gettickcount-timegettime-queryperformancecounter-rdtsc.aspx) 
| [HP](http://h20565.www2.hp.com/portal/site/hpsc/template.PAGE/public/kb/docDisplay/?spf_p.tpst=kbDocDisplay&spf_p.prp_kbDocDisplay=wsrp-navigationalState%3DdocId%253Demr_na-c02110402-1%257CdocLocale%253D%257CcalledBy%253D&javax.portlet.begCacheTok=com.vignette.cachetoken&javax.portlet.endCacheTok=com.vignette.cachetoken) 
| [IBM](http://www-947.ibm.com/support/entry/portal/docdisplay?brand=5000008&lndocid=MIGR-5084072)

**8048** For memory objects (CMemObj) that are already partitioned by Node, enabling this (startup-only) flag causes them to be partitioned by CPU instead, 
which can reduce CMEMTHREAD and SOS_SUSPEND_QUEUE waits in some circumstances. No longer functional in SQL 2016. 
[CSS 1](http://blogs.msdn.com/b/psssql/archive/2012/12/20/how-it-works-cmemthread-and-debugging-them.aspx) 
| [CSS 2](http://blogs.msdn.com/b/psssql/archive/2011/09/01/sql-server-2008-2008-r2-on-newer-machines-with-more-than-8-cpus-presented-per-numa-node-may-need-trace-flag-8048.aspx) 
| [CSS 3](http://blogs.msdn.com/b/psssql/archive/2013/11/19/spatial-indexing-from-4-days-to-4-hours.aspx) 
| [BobSQL](https://blogs.msdn.microsoft.com/bobsql/2016/06/22/does-sql-server-2016-require-trace-flag-t8048/) 
| [2809338](http://support.microsoft.com/kb/2809338/en-us) 
| [2887888](http://support.microsoft.com/kb/2887888/en-us) 
| [944902](http://support.microsoft.com/kb/944902/en-us)

**8049** Forces use of “Multi-Media” timer for time measurements. 
This [CSS](http://blogs.msdn.com/b/psssql/archive/2010/08/18/how-it-works-timer-outputs-in-sql-server-2008-r2-invariant-tsc.aspx) article is a great entry point. 
See also 8038. [972767](http://support.microsoft.com/kb/972767)


