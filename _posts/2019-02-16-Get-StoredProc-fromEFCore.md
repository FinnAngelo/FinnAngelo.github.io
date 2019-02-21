---
layout: post
title: "Get Stored Proc results from EnityFrameworkCore"
published: true
---
<figure>
	<a href="https://upload.wikimedia.org/wikipedia/commons/b/b7/Lorimerlite_framework.JPG">
		<img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/Lorimerlite_framework.JPG" alt="Oooo - pretty framework!"/>
	</a>
	<figcaption>This is a pretty framework picture</figcaption>
</figure>
Oh my, I can never remember the syntax of using the Entity Framework Core to simply get the results from a database, so here it is:

```csharp
[TestMethod]
public async Task GettingMethod_WhenRunProc_ThenIsAResult()
{
    //https://docs.microsoft.com/en-us/ef/core/querying/raw-sql
    var optionsBuilder = new DbContextOptionsBuilder<WhateverContext>();
    optionsBuilder.UseSqlServer("Server=(local);Database=WhateverDB;Trusted_Connection=True;");

    //These would be passed in by a method
    int param1 = 12345;
    DateTime param2 = DateTime.Today;
    
    StoredProcResult bob;
	
    // WhateverContext can come from DI
    // Note: EFCore has a funky way to handle the string interpolation so 
    // it isn't prone to sql injection 
    using (var context = new WhateverContext(optionsBuilder.Options))
    {
        bob = await context.StoredProc
            .FromSql($"EXEC dbo.StoredProc @Id={param1}, @Date={param2}")
            .FirstOrDefaultAsync();
    }

    Assert.AreEqual(bob.FIRSTNAME, "Robert");
}

//StoredProcResult class should be generated
public class WhateverContext : DbContext
{
    public WhateverContext(DbContextOptions<WhateverContext> options) : base(options) { }
    public DbQuery<StoredProcResult> StoredProc { get; set; }
}
```

The `FromSql` method is an extention method on the `StoredProc` property of the `WhateverContext`, which is interesting.

This is wonderfully concise, and note that _we don't have to do the mapping of the results to properties of the StoredProcResult object! Harrah!_

----------------------------------------

## Generating the result object ##

But there is a caviet - **the result object properties must match their type to the sql datatypes returned** 

That would be a pain in the butt, manually creating an object for a stored procedure with 50 columns returned. And yes, too many times I have dealt with legacy code that does exactly that.

Fortunately here is the sql to generate the return objects for the entire database

```sql

-- https://github.com/aspnet/EntityFrameworkCore/issues/245#issuecomment-403181137

DECLARE @namespace VARCHAR(100)= 'WebApplication1';
DECLARE @counter INT= 1;
DECLARE @EnD INT= 0;
DECLARE @SP_Name VARCHAR(250)= '';
DECLARE @CS_Type VARCHAR(100)= '';
DECLARE @Column_Name VARCHAR(250)= '';
DECLARE @Column_Ordinal INT= 0;
SELECT IDENTITY( INT, 1, 1) AS RowID, 
       p.NAME AS Sp_Name,
       CASE
           WHEN r.system_type_name = 'bigint'
           THEN 'long'
           WHEN r.system_type_name = 'smallint'
           THEN 'short'
           WHEN r.system_type_name = 'int'
           THEN 'int'
           WHEN r.system_type_name = 'UNIQUEIDENTIFIER'
           THEN 'Guid'
           WHEN r.system_type_name = 'smalldatetime'
           THEN 'DateTime'
           WHEN r.system_type_name = 'datetime'
           THEN 'DateTime'
           WHEN r.system_type_name = 'datetime2'
           THEN 'DateTime'
           WHEN r.system_type_name = 'date'
           THEN 'DateTime'
           WHEN r.system_type_name = 'time'
           THEN 'DateTime'
           WHEN r.system_type_name = 'float'
           THEN 'double'
           WHEN r.system_type_name = 'real'
           THEN 'float'
           WHEN CHARINDEX('numeric', r.system_type_name) = 1
           THEN 'decimal'
           WHEN r.system_type_name = 'smallmoney'
           THEN 'decimal'
           WHEN r.system_type_name = 'decimal'
           THEN 'decimal'
           WHEN r.system_type_name = 'money'
           THEN 'decimal'
           WHEN r.system_type_name = 'tinyint'
           THEN 'byte'
           WHEN r.system_type_name = 'bit'
           THEN 'bool'
           WHEN r.system_type_name = 'image'
           THEN 'byte[]'
           WHEN r.system_type_name = 'binary'
           THEN 'byte[]'
           WHEN r.system_type_name = 'varbinary'
           THEN 'byte[]'
           WHEN r.system_type_name = 'timestamp'
           THEN 'byte[]'
           WHEN r.system_type_name = 'geography'
           THEN 'Microsoft.SqlServer.Types.SqlGeography'
           WHEN r.system_type_name = 'geometry'
           THEN 'Microsoft.SqlServer.Types.SqlGeometry'
           ELSE 'string'
       END + CASE
                 WHEN R.system_type_name IN('BigInt', 'Int', 'SmallInt', 'TinyInt', 'float', 'real', 'numeric', 'smallmoney', 'decimal', 'money', 'bit', 'UNIQUEIDENTIFIER', 'smalldatetime', 'datetime', 'datetime2', 'date', 'time')
                      AND IS_Nullable = 1
                 THEN '?'
                 ELSE ''
             END AS CS_Type, 
       replace(ISNULL(r.NAME, 'Unnamed'), ' ', '_') AS Column_Name, 
       r.column_ordinal
INTO #Tentity
FROM sys.procedures AS p
     CROSS APPLY sys.dm_exec_describe_first_result_set_for_object(p.object_id, 0) AS r
WHERE r.system_Type_Name IS NOT NULL;
SET @EnD = @@IDENTITY;

--**********
PRINT 'using System;';
PRINT 'using System.Collections.Generic;';
PRINT 'using System.ComponentModel.DataAnnotations;';
PRINT 'using System.Linq;';
PRINT 'using System.Threading.Tasks;';
PRINT '';
PRINT 'namespace ' + @namespace + '.Models';
PRINT '{';
WHILE @counter <= @EnD
    BEGIN
        SELECT @SP_Name = Sp_Name, 
               @CS_Type = CS_Type, 
               @Column_Name = Column_Name, 
               @Column_Ordinal = column_ordinal
        FROM #Tentity
        WHERE RowID = @counter;
        IF @Counter > 1
           AND @Column_Ordinal = 1
            BEGIN
                PRINT '    }';
                PRINT '';
        END;
        IF @Column_Ordinal = 1
            BEGIN
                PRINT '    public class ' + @SP_Name + 'Result';
                PRINT '    {';
                PRINT '        public ' + @CS_Type + ' ' + @Column_Name + ' { get; set; }';
        END;
            ELSE
            PRINT '        public ' + @CS_Type + ' ' + @Column_Name + ' { get; set; }';
        SET @Counter = @Counter + 1;
    END;
DROP TABLE #Tentity;
PRINT ' }';
PRINT '}';

```

Although it needed a bit of tweaking to deal nicely with decimals, full credit to [rkmcquillen](https://github.com/rkmcquillen) for the sql

_Cheers!_
