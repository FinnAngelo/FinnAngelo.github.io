---
layout: post
title: "#010: Comparing Stored Procs in different databases"
published: true
---
Sometimes it's a pain with a Dev database and a Prod database to see where it is changed for migrating stored procs. 

This can be quite slow, but really useful

-----------------------

```sql

-- Needs table variables because the information_schema results don't compare nicely. Weird.

declare @DevDB table ([SCHEMA] varchar(255), [NAME] varchar(255), [TYPE] varchar (255), [DEFINITION] varchar(8000))

insert into @DevDB
SELECT ROUTINE_SCHEMA
	, ROUTINE_NAME
	, ROUTINE_TYPE
	, ROUTINE_DEFINITION
FROM DevDB.INFORMATION_SCHEMA.ROUTINES

declare @ProdDB table ([SCHEMA] varchar(255), [NAME] varchar(255), [TYPE] varchar (255), [DEFINITION] varchar(8000))

insert into @ProdDB
SELECT ROUTINE_SCHEMA
	, ROUTINE_NAME
	, ROUTINE_TYPE
	, ROUTINE_DEFINITION
FROM ProdDB.INFORMATION_SCHEMA.ROUTINES

SELECT 'DevDB' AS tblName, *
FROM (
	SELECT * FROM @DevDB	
	EXCEPT	
	SELECT * FROM @ProdDB
	) X
UNION ALL
SELECT 'ProdDB' AS tblName, *
FROM (
	SELECT * FROM @ProdDB	
	EXCEPT	
	SELECT * FROM @DevDB
	) X
ORDER BY [TYPE], NAME, [SCHEMA], [tblName]

```
