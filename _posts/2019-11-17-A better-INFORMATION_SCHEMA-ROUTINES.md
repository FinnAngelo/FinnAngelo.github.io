---
layout: post
title: "A better INFORMATION_SCHEMA.ROUTINES"
categories: sql 
published: false
---

FYI: `INFORMATION_SCHEMA.ROUTINES` only pulls the first 4000 chars of a stored procedure.

Sometimes procs are longer so my original select missed some of the tables.

----------------------------------------

## TOC ##

+ [The improved ROUTINES routine](#The-improved-ROUTINES-routine)
+ [Credits](#Credits)

----------------------------------------

## The improved ROUTINES routine ##

Here is the new sql to match tables to columns:

```sql
WITH [ROUTINES]
     AS (SELECT SCHEMA_NAME(o.schema_id) AS ROUTINE_SCHEMA,
                o.name AS ROUTINE_NAME,
                CONVERT(NVARCHAR(20),
                                   CASE
                                     WHEN o.type IN('P', 'PC')
                                     THEN 'PROCEDURE'
                                     ELSE 'FUNCTION'
                                   END) AS ROUTINE_TYPE,
                CONVERT(NVARCHAR(MAX), OBJECT_DEFINITION(o.object_id)) AS ROUTINE_DEFINITION
         FROM   sys.objects AS o
         LEFT JOIN
         sys.parameters AS c
         ON c.object_id=o.object_id
            AND c.parameter_id=0
         WHERE  o.type IN
         ('P', 'FN', 'TF', 'IF', 'AF', 'FT', 'IS', 'PC', 'FS'))
     SELECT r.ROUTINE_TYPE,
            r.ROUTINE_SCHEMA+'.'+r.ROUTINE_NAME AS ROUTINE,
            t.TABLE_SCHEMA+'.'+t.TABLE_NAME AS [TABLE],
            PATINDEX('%'+t.TABLE_NAME+'%', UPPER(r.ROUTINE_DEFINITION))
     FROM   [ROUTINES] AS r
     INNER JOIN
     INFORMATION_SCHEMA.TABLES AS t
     ON r.ROUTINE_DEFINITION LIKE '%'+t.TABLE_NAME+'%'
            ORDER BY 1, 2, 3;
GO
```

the CTE gets its ‘ROUTINES list from:
sp_helptext [INFORMATION_SCHEMA.ROUTINES]

----------------------------------------

## Credits ##

_Cheers!_
