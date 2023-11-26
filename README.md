# sp_drop_create_stats_external_table

The sp_drop_create_stats_external_table is able to generate all the T-SQL statements for drop and create statistics defined on SQL Server external tables in your database!

The [CREATE STATISTICS](https://learn.microsoft.com/sql/t-sql/statements/create-statistics-transact-sql?WT.mc_id=DP-MVP-4029181#limitations-and-restrictions) documentation page says: "Updating statistics is not supported on external tables. To update statistics on an external table, drop and re-create the statistics". SQL Server documentation tells us explicitly that update statistics is not supported on external tables!

When creating external table statistics, SQL Server imports the external table into a temporary SQL Server table and then creates the statistics. For sample statistics, only the sampled rows are imported. If you have a large external table, it is faster to use the default sampling instead of the full scan option.
In addition, when the external table uses DELIMITEDTEXT, CSV, PARQUET, or DELTA as data types, external tables only support statistics for one column per CREATE STATISTICS command.

The error can be worked around using one of the following options:

- Ignore the external table (this option is not available for maintenance plan managed by SQL Server Management Studio maintenance plan; third-party solutions are needed)
- Drop and create statistics as a part of the maintenance plan (drop statistics before starting the update statistics task and recreate them after the statistics maintenance)

I loved the second option, so I developed the stored procedure sp_drop_create_stats_external_table that can generate all the T-SQL statements for drop and create statistics defined on external tables; it supports statistics on multiple columns.
