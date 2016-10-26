# Instance Startup, Database Startup, and General Instance Behavior

See also: 4606 (cat), 4614 (cat)

| Flag | Description | Links |
| ---------- | ----------- | -------- |
| 902 | In the KB article, used as part of a workaround for starting SQL Server after a cumulative update has been applied on an instance that has a UCP. (Though what the flag specifically does is not clear from this article). In the Technet blog post and the SQL Server Central blog post, the flag is used to halt/bypass an in-progress CU update that has gone horribly wrong. Based on this [related connect item](https://connect.microsoft.com/SQLServer/feedback/details/738612/sql-server-2012-cu1-upgrade-step-msdb110-upgrade-sql-encountered-error-547), it appears that 902 causes SQL Server to bypass “script upgrade” mode if the instance is still in that mode. Of minor interest is an old fix KB for SQL 2000 SP1, [KB290080](http://support.microsoft.com/kb/290080) | [KB2163980](http://support.microsoft.com/kb/2163980); [Technet](http://blogs.msdn.com/b/karthick_pk/archive/2010/11/18/sqlserver2008-script-level-upgrade-for-database-master-failed-because-upgrade-step-sqlagent100-msdb-upgrade-sql-encountered-error-574-state-0-severity-16.aspx); [SSC](http://www.sqlservercentral.com/blogs/everyday-sql/2015/07/07/use-trace-flag-902-to-recover-from-a-cumulative-update-failure/); | 
| ph | desc | links |

