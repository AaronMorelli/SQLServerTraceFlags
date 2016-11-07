# Plan Cache

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 

## Functionality Toggles

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 174 | Effectively increases the maximum # of plan cache entries. Can relieve SOS_CACHESTORE spinlock contention though at the cost of increasing the amount of memory allowed for the plan cache. | [3026083](http://support.microsoft.com/kb/3026083) | 
| 205 | Randal-SQL-SDB407: "log plan recompilations and reasons for them." Messages go to the SQL error log. | [195565](http://support.microsoft.com/kb/195565/en-us) | 
| 253 | SSC repository: "Prevents ad-hoc query plans to stay in cache." *Even though many "three-digit" query plan numbers don’t seem to be relevant any more, the first SO hit indicates that this flag still changes functionality in the manner described by the SSC definition. As a side note, Martin Smith’s comment in the second Stack Overflow hit about DAC queries never being cached is interesting.*  | [SO](http://stackoverflow.com/questions/2596587/what-does-sql-server-trace-flag-253-do); [SO](http://dba.stackexchange.com/questions/11693/is-there-an-equivalent-of-optionrecompile-or-with-recompile-for-an-entire) | 
| 2861 | KB: "Instructs SQL Server to keep zero cost plans in cache, which SQL Server would typically not cache (such as simple ad-hoc queries, set statements, commit transaction and others)." *The Confio link is mildly interesting* | [325607](http://support.microsoft.com/kb/325607); [Confio](http://www.confio.com/logicalread/why-ignite-uses-sql-server-trace-flag-2861-and-zero-cost-plans/#.UzCMQPldWSp) | 
| 4136 | **Doc2014** Disables parameter sniffing; equivalent to adding an OPTIMIZE FOR UNKNOWN hint to each query which references a parameter. Dima notes that 4136 has no effect on parameter sniffing’s "cousin", runtime constant sniffing. | [980653](http://support.microsoft.com/kb/980653); [Nevarez](http://www.benjaminnevarez.com/2010/08/disabling-parameter-sniffing/); [Dima](http://www.queryprocessor.com/runtime-constants-sniffing/) | 
| 8032 | (more below) | | 



**8032** **Doc2008R2** BOL: "Reverts the cache limit parameters to the SQL Server 2005 RTM setting which in general allows caches to be larger. Use this 
setting when frequently reused cache entries do not fit into the cache and when the optimize for ad hoc workloads Server Configuration Option has failed to 
resolve the problem with plan cache. Caution: Trace flag 8032 can cause poor performance if large caches make less memory available for other memory consumers, 
such as the buffer pool." *Banerjee: Can’t be used as a startup flag; @sql_handle indicates it can ONLY be used as a startup flag in SQL 2014 (and I would guess in 2012 as well, 
since that's when the SQLOS mem rearchitecture occurred). @sql_handle tweet indicates it also increases size of log pool cache. 




## Limited Lifespan
These flags are specific to older release(s), or even build(s), typically because they enable a specific fix (typically in a CU or hotfix), appear only in CTPs, 
or have behavior that has been superceded in more recent versions.

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 2880 | Enables an upper bound for the plan cache in SQL 2000 32-bit. (The upper bound is by default disabled in 32-bit SQL 2000 and enabled in 64-bit SQL 2000) | [891707](https://support.microsoft.com/en-us/kb/891707) | 
| 2881 | Turns off an an upper bound on how large the plan cache can get. (Upper bound introduced in 64-bit SQL 2000 to solve a prob with the plan cache and adhoc sql). Does the flag still do anything? | [891707](https://support.microsoft.com/en-us/kb/891707) | 