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
+ [Get Column types from a Select into a temp table](#Get-Column-types-from-a-Select-into-a-temp-table)
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
```http://<server>/reportserver?/<path>/<report>&rs:Command=Render&rs:Format=CSV&<parameter>=<value>&<parameter>=<value>```

Example:  
```http://blahblah.com/ReportServer?/Reports/GetThings&rs:Command=Render&rs:Format=CSV&StartDate=2014-01-01&EndDate=2015-01-01```

----------------------------------------

## Get Column types from a Select into a temp table ##

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

----------------------------------------

## Credits ##

If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
