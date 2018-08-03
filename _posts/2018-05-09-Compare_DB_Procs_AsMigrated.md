---
layout: post
title: "Comparing Stored Procs in different databases"
published: true
---
Sometimes it's a pain with a Dev database and a Prod database to see where it is changed for migrating stored procs. 

This can be quite slow, but really useful

I usually use a restored version of the production database as having a linked server to prod aint right on a dev box

-----------------------

```sql
-- Needs table variables because the information_schema results don't compare nicely. Weird.

DECLARE @DevDB TABLE ([SCHEMA] VARCHAR(255), [NAME] VARCHAR(255), [TYPE] VARCHAR(255), [DEFINITION] VARCHAR(8000))

INSERT INTO @DevDB
SELECT ROUTINE_SCHEMA, ROUTINE_NAME, ROUTINE_TYPE, ROUTINE_DEFINITION
FROM DevDB.INFORMATION_SCHEMA.ROUTINES

DECLARE @ProdDB TABLE ([SCHEMA] VARCHAR(255), [NAME] VARCHAR(255), [TYPE] VARCHAR(255), [DEFINITION] VARCHAR(8000))

INSERT INTO @ProdDB
SELECT ROUTINE_SCHEMA, ROUTINE_NAME, ROUTINE_TYPE, ROUTINE_DEFINITION
FROM ProdDB.INFORMATION_SCHEMA.ROUTINES
```
-----------------------
```sql
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
