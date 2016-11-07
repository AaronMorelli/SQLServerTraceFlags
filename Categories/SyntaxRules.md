# Query Syntax/Rules

(Reminder: [Official BOL location](http://technet.microsoft.com/en-us/library/ms188396.aspx))

See also: 

TODO: this category is not very well defined


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
