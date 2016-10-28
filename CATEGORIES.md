# TF Categories

Within each category, the following sections may be observed: 
- A "See Also" section with flags that could be placed in this category but fit more closely with another category. (Each flag has only 1 entry across the whole repo)
- A "High Profile Flags" section where flags with many links (i.e. commonly discussed and used) appear. Since Markdown has limited formatting and tables can become unwieldy for larger entries, flags with much content are moved out of the table and into paragraph form near the top
- A table of flags, with descriptions and links. 

### The "Description" column
Flags that are officially documented in BOL will have a "Doc<release>" tag at the beginning. The <release> is always the earliest release in which the flag was document, not necessarily the release in which the flag was introduced.
Flags that are *believed* to be informational-only have an "Info" tag at the beginning. Note, however that this repo cannot authoritatively guarantee whether a given flag is only informational; only official Microsoft documentation can do that.
For flags with longer descriptions and several different sources of info, the text is displayed in alternating normal/*italics* case to enable quick identification of the changes in source.

### The "Links" column 
The “Links” column contains hyperlinks to online content that provides a definition, detailed explanation, or practical example of using the trace flag. These links are typically KB articles, whitepapers, or blog articles from generally reputable sources (especially current or ex-Microsoft employees, and bloggers who demonstrate, in the author’s view, accuracy and trustworthy methodologies).


## Queries
1. [Query Compilation (Info only) and Stats Object-related](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/CompAndStats.md)
2. [Query Compilation Behavioral (Cardinality Estimation only)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/CompEst.md)
3. [Query Compilation Behavioral (except Cardinality Estimation)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/CompBehav.md)
4. [Plan Caching](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/PlanCache.md)
5. [Query Syntax/Rules](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/SyntaxRules.md)  (contents are work-in-progress)
6. [Query Execution](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/QueryExec.md)
7. [TF 4199 and related](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/TF4199.md)
8. [Non-TF4199 Query Performance/Execution fixes](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/NonTF4199Fixes.md)


## Databases 
1. [Databases, Database Files, and Allocation (incl TempDB)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DBsFiles.md)
2. [Transactions, Logging, and Recovery](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/TranLogRecov.md)
3. [Backup and Restore](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/BackupRestore.md)
4. [CHECKDB and Corruptions](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/CorruptAndCHECKDB.md)
5. [Locking and Waiting](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/LockWait.md)


## Server
1. [Instance Startup, Database Startup, and General Instance Behavior](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/InstStart_DBStart_GenInst.md)
2. [SQL Miscellaneous Informational (especially re: Error Log)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/MiscInfo.md)
3. [SQLOS Scheduling and CPU-related](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/SchedCPU.md)
4. [SQLOS Memory and Buffer Pool](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/MemBuf.md)
5. [Disk and Network IO](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/DiskNetworkIO.md)
6. [Connectivity](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Connectivity.md)
7. [Background Sessions and Tasks](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/BackgroundSessTask.md)
8. [High Availability/Distributed Technologies](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/HADRDist.md)
9. [Security](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Security.md)
10. [Special Features](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/SpecialFeatures.md)
11. [Exceptions and Memory Dump Behavior](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/ExceptMemDump.md)
12. [Non-Optimizer Bug Fixes (contents are work-in-progress)](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/NonOptFixes.md)


## Legacy and Other
1. [SQL 2000 Engine](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/SQL2KEng.md)
2. [SQL 2000 Optimization/Query Performance Fixes](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/2KOptFixes.md)
3. [SQL 2000 Query Execution](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/2KQueryExec.md)
4. [Pre-SQL 2000 Flags](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Pre2K.md)
5. [Mis-labeled, unable to find links, Other](https://github.com/AaronMorelli/SQLServerTraceFlags/blob/master/Categories/Other.md)

