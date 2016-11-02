# SQLServerTraceFlags

*Purpose:* A Markdown-based repository for SQL Server trace flags, both documented and undocumented. Flags are organized in a topical manner, with links to relevant, useful, and generally trustworthy sources.

*Editor/Primary Compiler:* Aaron Morelli ([Blog](http://sqlcrossjoin.wordpress.com/) | [Twitter](https://twitter.com/sqlcrossjoin))

[Change Log](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/CHANGELOG.md)

[Categories](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/CATEGORIES.md)

*Disclaimer:* The majority of these flags are undocumented by Microsoft, and therefore unsupported. The author has experimented with only a handful of these flags and cannot vouch for complete accuracy in the descriptions provided, nor has authority to do so. The author assumes no responsibility for any negative (or positive!) consequences arising from the use or misuse of these trace flags in any production or non-production environment. Use at your own risk.

*Official Trace Flag Documentation from Microsoft:* (Currently no distinction made betweeen a "2014" version of the page or a "2016" version)
[Current](http://technet.microsoft.com/en-us/library/ms188396.aspx) 
| [2012](http://technet.microsoft.com/en-us/library/ms188396(v=sql.110).aspx) 
| [2008 R2](http://technet.microsoft.com/en-us/library/ms188396(v=sql.105).aspx) 
| [2008](http://technet.microsoft.com/en-us/library/ms188396(v=sql.100).aspx) 
| [2005](http://technet.microsoft.com/en-us/library/ms188396(v=sql.90).aspx) 
| [TF 4199](http://support.microsoft.com/kb/974006)
| [KB920093](https://support.microsoft.com/en-us/kb/920093)

*Notable unofficial trace flag repositories:*
[Technet Wiki article](http://social.technet.microsoft.com/wiki/contents/articles/13105.trace-flags-in-sql-server.aspx)
| [SQL Server Central](http://www.sqlservercentral.com/articles/trace+flags/70131/) (and for [2000 and 7.0](http://www.sqlservercentral.com/articles/Monitoring/traceflags/737/))
| [SQLService.se](http://sqlservice.se/sv/start/blogg/updated-microsoft-sql-server-trace-flag-list.aspx)
| [Amit Banerjee 2012](http://troubleshootingsql.com/2014/01/20/sql-server-2012-trace-flags/)
| [Database-Wiki](http://database-wiki.com/2012/10/20/documented-sql-server-trace-flags-use-them-cautiously/)
| [SQL Handle TF4199 series](http://sql-sasquatch.blogspot.com/2014/01/trace-flag-4199-complex-risk-assessment.html)
| [Paul Randal presentation](http://www.scribd.com/doc/109431789/Randal-SQL-SDB407-Undocumented)

*Print Sources:*
At this time, only two print sources have been used, and even these are only source material for a handful of trace flags: 
Kalen Delaney’s [SQL Server 2008 Internals](https://www.amazon.com/Microsoft%C2%AE-Server%C2%AE-Internals-Developer-Reference/dp/0735626243/ref=sr_1_2?ie=UTF8&qid=1477503776&sr=8-2&keywords=Kalen+Delaney+2008) referred to in shorthand as “Kalen2008”, and 
Ken Henderson’s [SQL Server 2005 Practical Troubleshooting: The Database Engine](https://www.amazon.com/SQL-Server-2005-Practical-Troubleshooting/dp/0321447743/ref=sr_1_1?ie=UTF8&qid=1477503810&sr=8-1&keywords=Ken+Henderson+2005) referred to in shorthand as “Khen2005”.

## Trace Flag Usage and Categorization
Most (but not all) trace flags can be enabled at SQL Server startup by using the –T (capital letter) startup option. However, most (but not all) trace flags can also be enabled 
at startup by using the –t (lowercase letter) startup option. (Besides the upper/lowercase difference, the syntax is otherwise identical). However, 
[BOL](https://msdn.microsoft.com/en-us/library/ms190737.aspx) indicates that other, unknown trace flags are enabled by the lowercase option, and thus the uppercase method 
should be used except under the direction of Microsoft support.

[Be careful](http://blogs.msdn.com/b/psssql/archive/2010/02/19/did-you-start-your-sql-server-engine-correctly.aspx) when adding Trace Flags (or other startup options) to the SQL Server via the Configuration Manager GUI.

Microsoft employee Nacho Alonso Portillo [notes](http://blogs.msdn.com/b/ialonso/archive/2011/12/05/what-is-the-expected-behavior-from-an-attempt-to-enable-a-trace-flag-which-is-not-defined-in-the-targeted-version-of-the-product.aspx) the following about the DBCC TRACEON command:
- The valid range for trace flags for 2008 R2 and before is -1 to 9299. (Newer releases have introduced flags higher in the 9000 range and even above 10000)
- However, “-1” has the well-known special meaning of enabling/disabling the flag globally. Note that “9299” as an upper limit is clearly dated, as there are now trace flags with #’s higher than 9299, such as the well-known 9481, and apparently even a 10202 in “vNext” (the version after SQL 2014).
- The “-1” wildcard can appear in any position in the DBCC TRACEON syntax, though by convention most online use of it always pushes it to the back of the command, after other TF #’s.
- Some flags can only be used with the “-T” startup option, and some can only be used with the DBCC TRACEON command. Attempting to enable “-T”-only flags with DBCC TRACEON will result in an error.

Trace flags that affect query plan or query execution behavior can be enabled for individual queries (rather than session- or system-wide) via the QUERYTRACEON query hint. Not all trace flags are officially supported with QUERYTRACEON; [KB2801413](http://support.microsoft.com/kb/2801413/en-us) documents those that are.

[KB974006](http://support.microsoft.com/kb/974006/en-us) notes: “If DBCC TRACEON\TRACEOFF is used this does not regenerate a new cached plan for stored procedures. Plans could be in cache that were created without the trace flag”

TF 4199 (officially described in [KB974006](http://support.microsoft.com/kb/974006/en-us)) is a special flag:
- Starting with SQL 2000 SP3, the query optimizer team has released all hotfix code under a policy such that the fix remains disabled until TF 4199 is enabled, unless that fix corrects a bug related to incorrect results or assertions or the like. 
- SQL Sasquatch ([Blog](http://sql-sasquatch.blogspot.com/) | [Twitter](https://twitter.com/sql_handle)) has a short blog series on TF 4199, in which he notes that there are [risks inherent](http://sql-sasquatch.blogspot.com/2014/01/trace-flag-4199-complex-risk-assessment.html) in enabling TF 4199 system-wide. This makes Microsoft’s official support for the QUERYTRACEON individual query hint even more important and useful.
- In the same blog series, SQL Sasquatch [points out](http://sql-sasquatch.blogspot.com/2014/01/trace-flag-4199-complex-risk-assessment_6.html) that Microsoft has released several fixes recently that seem to violate the TF 4199 policy described above.

A 5-year-old [blog post](http://sqlblog.com/blogs/andrew_kelly/archive/2009/06/21/trace-flag-groupings.aspx) by Andrew Kelly relays some information from Paul Randal about how trace flags are categorized. This provides some level of insight into how Microsoft categorizes (or perhaps categorized at some point in the past) their trace flags, though even a cursory glance through the lists below show that many flags do not fall into any of the below categories.
- 25xx, 52xx are DBCC related 
- 8xx are buffer pool 
- 36xx are SQL Server general 'run-time' 
- 6xx are Storage Engine access methods 
- 12xx are lock manager 
- 14xx are database mirroring 
- 30xx, 31xx, 32xx are backup/restore 
- 55xx are FILESTREAM 
- 73xx, 74xx are query execution 
- 75xx are cursors 
- 82xx are replication

