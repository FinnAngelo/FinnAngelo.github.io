---
layout: post
title: "SQL tricks and things"
categories: SQL 
published: true
---
<figure style="float:right; margin-left:3em; width:50%;display:none;">
    <a href="http://SomeGif">
        <img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/NeedAGif.jpg" alt="ToDo - get a gif" />
    </a>
    <figcaption>Needs a gif</figcaption>
</figure>

I have [Powershell Snippets](http://www.finnangelo.com/2019/05/24/Powershell_Snippets.html), so let's get somewhere for my SQL bits as well

----------------------------------------

## TOC ##

+ [See table values in Debug on SQL](#See-table-values-in-Debug-on-SQL)
+ [Get a CSV Report from SSRS with an URL](#Get-a-CSV-Report-from-SSRS-with-an-URL)
+ [Get Column types from a SELECT into a TEMP table](#Get-Column-types-from-a-SELECT-into-a-TEMP-table)
+ [Create model from TEMP table](#Create-model-from-TEMP-table)
+ [Sync tables using MERGE](#Sync-tables-using-MERGE)
+ [Credits](#Credits)

----------------------------------------

## See table values in Debug on SQL  ##

When debugging in <abbr title="SQL Server Management Studio">SSMS</abbr>, it actually has a locals watch window... that doesn't show table values.  
Unless you do this:

```sql
-- <tablename> can be an @table variable
-- Can then view @v in locals window; OK for small amounts of data

DECLARE @v XML = (SELECT * FROM <tablename> FOR XML AUTO)
```


----------------------------------------

## Get a CSV Report from SSRS with an URL ##

Sometimes you need a lot of reports from <abbr title="SQL Server Reporting Server">SSRS</abbr>, where you are going to diff the csv or whatever

+ <http://blogs.infosupport.com/modify-reporting-services-export-to-csv-behavior/>
+ <http://stackoverflow.com/questions/1078863/passing-parameter-via-url-to-sql-server-reporting-service>
+ <http://msdn.microsoft.com/en-us/library/ms155391.aspx>

The Url Format:  
```
http://<server>/reportserver?/<path>/<report>&rs:Command=Render&rs:Format=CSV&<parameter>=<value>&<parameter>=<value>
```

Example:  
```
http://blahblah.com/ReportServer?/Reports/GetThings&rs:Command=Render&rs:Format=CSV&StartDate=2014-01-01&EndDate=2015-01-01
```

----------------------------------------

## Get Column types from a Select into a temp table ##

Sometimes you want a model from a sql `SELECT`, like for [Dapper](https://github.com/StackExchange/Dapper) or something...

+ <http://stackoverflow.com/questions/8976414/get-structure-of-temp-table-like-generate-sql-script-and-clear-temp-table-for>

```sql
SELECT 'Have a nice day' AS Howdy
	,GETDATE() AS Nowby
	,*
INTO #myTempTable -- creates a new temp table
FROM tMyTable -- some table in your database

EXEC tempdb..sp_help '#myTempTable'

DROP TABLE #myTempTable

------------------------------------------------------
IF OBJECT_ID('tempdb..#TEMP') IS NOT NULL
BEGIN
	DROP TABLE #TEMP
END

SELECT *
INTO #TEMP
FROM WHEREVER

SELECT --*
	',' + c.COLUMN_NAME + ' ' + UPPER(c.DATA_TYPE) + CASE 
		WHEN c.DATA_TYPE IN (
				'char'
				,'varchar'
				,'nvarchar'
				)
			THEN '(' + CAST(c.CHARACTER_MAXIMUM_LENGTH AS VARCHAR(10)) + ')'
		WHEN c.DATA_TYPE IN ('numeric')
			THEN '(' + CAST(c.NUMERIC_PRECISION AS VARCHAR(10)) + ',' + CAST(c.NUMERIC_PRECISION_RADIX AS VARCHAR(10)) + ')'
		ELSE ''
		END + CASE 
		WHEN c.IS_NULLABLE = 'no'
			THEN ' NOT NULL'
		ELSE ''
		END
FROM tempdb.INFORMATION_SCHEMA.TABLES t
INNER JOIN tempdb.INFORMATION_SCHEMA.COLUMNS c ON t.TABLE_NAME = c.TABLE_NAME
WHERE t.TABLE_NAME LIKE '%TEMP%'
```

## Create model from TEMP table ##

```sql
---------------------------------------------------
-- Makes MODEL and MapToMODEL extension
---------------------------------------------------
IF OBJECT_ID('tempdb..#TEMP') IS NOT NULL
BEGIN
	DROP TABLE #TEMP
END

SELECT TOP 1 *
INTO #TEMP
FROM SampleDB.dbo.TestTable

--select * from INFORMATION_SCHEMA.COLUMNS c where c.TABLE_NAME =
-- Don't forget to set RtClick > Query Options > Results > Text to 8192
-- http://msdn.microsoft.com/en-au/library/ms179334%28v=sql.105%29.aspx
-- http://stackoverflow.com/questions/1484147/get-list-of-computed-columns-in-database-table-sql-server
DECLARE @classType VARCHAR(50) = 'TestTable'
DECLARE @nameSpace VARCHAR(50) = 'Company.Module.Areas.HelloWorld.Models'
DECLARE @result VARCHAR(max)
---------------------------------------------------------------------
DECLARE @TypeMapping TABLE (
	SqlType VARCHAR(20)
	, NetType VARCHAR(20)
	)

INSERT INTO @TypeMapping
SELECT 'bigint', 'long'
UNION SELECT 'binary', 'byte[]'
UNION SELECT 'bit', 'bool'
UNION SELECT 'char', 'string'
UNION SELECT 'date', 'DateTime'
UNION SELECT 'datetime', 'DateTime'
UNION SELECT 'datetime2', 'DateTime'
UNION SELECT 'DATETIMEOFFSET', 'DateTimeOffset'
UNION SELECT 'decimal', 'decimal'
UNION SELECT 'float', 'double'
UNION SELECT 'int', 'int'
UNION SELECT 'money', 'decimal'
UNION SELECT 'nchar', 'string'
UNION SELECT 'ntext', 'string'
UNION SELECT 'numeric', 'decimal'
UNION SELECT 'nvarchar', 'string'
UNION SELECT 'real', 'single'
UNION SELECT 'rowversion', 'byte[]'
UNION SELECT 'smallint', 'short'
UNION SELECT 'smallmoney', 'decimal'
UNION SELECT 'sql_variant', 'object'
UNION SELECT 'text', 'string'
UNION SELECT 'time', 'TimeSpan'
UNION SELECT 'tinyint', 'byte'
UNION SELECT 'uniqueidentifier', 'Guid'
UNION SELECT 'varbinary', 'byte[]'
UNION SELECT 'varchar', 'string'
---------------------------------------------------------------------
-- Name mapping
DECLARE @Fields TABLE (
	IsRequired VARCHAR(20)
	, StringLength VARCHAR(30)
	, NetType VARCHAR(20)
	, ColumnName VARCHAR(30)
	, FieldName VARCHAR(30)
	)

INSERT INTO @Fields
SELECT CASE ic.IS_NULLABLE
		WHEN 'NO'
			THEN '[Required]'
		ELSE ''
		END AS IsRequired
	, CASE 
		WHEN ic.Data_type IN ('char', 'nchar', 'nvarchar', 'varchar')
			THEN '[StringLength(' + cast(ic.CHARACTER_MAXIMUM_LENGTH AS VARCHAR) + ')]'
		ELSE ''
		END AS StringLength
	, tm.NetType + CASE 
		WHEN ic.IS_NULLABLE = 'YES'
			AND ic.Data_type NOT IN ('char', 'nchar', 'ntext', 'nvarchar', 'text', 'varchar')
			THEN '?'
		ELSE ''
		END AS NetType
	, ic.COLUMN_NAME AS ColumnName
	, UPPER(SUBSTRING(ic.COLUMN_NAME,1,1))
	+ SUBSTRING(ic.COLUMN_NAME,2,LEN(ic.COLUMN_NAME)-1) AS	FieldName

FROM tempdb.INFORMATION_SCHEMA.TABLES t
INNER JOIN tempdb.INFORMATION_SCHEMA.COLUMNS ic ON t.TABLE_NAME = ic.TABLE_NAME
LEFT JOIN @TypeMapping AS tm ON tm.SqlType = ic.DATA_TYPE
WHERE t.TABLE_NAME LIKE '%TEMP%'

--SELECT * FROM @Fields

---------------------------------------------------------------------
SELECT @result = cast('// Generated from the SQL Server Tables
// Because it would be crazy to write this by hand!
using System;
using System.ComponentModel.DataAnnotations;
using System.Data;

namespace ' + @nameSpace + '
{' AS VARCHAR(max))

SET @result = @result + '
    public class ' + @classType + '
    {'
SELECT @result = @result + '
        ' + f.IsRequired + f.StringLength + '
        public ' + f.NetType + ' ' + f.FieldName + ' { get; set; }'
FROM @Fields AS f

---------------------------------------------------------------------
SELECT @result = @result + '
    }

	//Needs to be external from mapped class, otherwise interferes with WebAPI
	internal static partial class ' + @classType + 'Extensions
	{
	    public static ' + @classType + ' MapTo' + @classType + '(this IDataReader dataReader)
		{
		    ' + @classType + ' result = new ' + @classType + '();'

---------------------------------------------------------------------
-- Constructor mapping
SELECT @result = @result + CASE 
		WHEN f.IsRequired != ''
			THEN '
            result.' + f.FieldName + ' = (' + f.NetType + ')dataReader["' + f.ColumnName + '"];'
		ELSE '
            result.' + f.FieldName + ' = dataReader["' + f.ColumnName + '"] as ' + f.NetType + ';'
		END
FROM @Fields AS f
SELECT @result = @result + '
            return result;
		}
    }
}	
'

PRINT @result
```

----------------------------------------

## Sync tables using MERGE ##

I have not performance tested this - it's probabaly _really_ slow

+ <http://www.toplinestrategies.com/blogs/application-development/sync-data-changes-tsql-merge>

Notice it includes the syntax for `OUTPUT`!

```sql
MERGE CustomerTarget AS t
USING CustomerSource AS s
	ON t.Email = s.Email
WHEN MATCHED
	AND (
		t.FirstName != s.FirstName
		OR t.LastName != s.LastName
		OR t.Address != s.Address
		OR t.City != s.City
		OR t.STATE != s.STATE
		OR t.PostalCode != s.PostalCode
		OR t.Email != s.Email
		)
	THEN
		UPDATE
		SET t.FirstName = s.FirstName,
			t.LastName = s.LastName,
			t.Address = s.Address,
			t.City = s.City,
			t.STATE = s.STATE,
			t.PostalCode = s.PostalCode,
			t.Email = s.Email
WHEN NOT MATCHED BY TARGET
	THEN
		INSERT (
			FirstName,
			LastName,
			Address,
			City,
			STATE,
			PostalCode,
			Email
			)
		VALUES (
			s.FirstName,
			s.LastName,
			s.Address,
			s.City,
			s.STATE,
			s.PostalCode,
			s.Email
			)
WHEN NOT MATCHED BY SOURCE
	THEN
		DELETE
OUTPUT $ACTION AS Operation,
	inserted.Email,
	inserted.City,
	inserted.STATE,
	deleted.Email,
	deleted.City,
	deleted.STATE;
```



----------------------------------------

## Credits ##

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
