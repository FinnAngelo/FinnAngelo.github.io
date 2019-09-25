---
layout: post
title: "How to Mock a DataReader"
published: false
---

Very exciting!
Here is how to setup data for a datareader for a moq!
(ToDo: Get picture)

----------------------------------------

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
using System.Collections.Generic;
using System.Data;
using System.Dynamic;
using System.Linq;

//https://gist.github.com/XelaNimed/d66bb06873f62fc66a693955bb0cd17d
//https://stackoverflow.com/questions/2643909/how-to-mock-an-sqldatareader-using-moq-update
namespace MoqReader
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod]
        public void TestMethod1()
        {
            var list = new List<ExpandoObject>()
            {
                GetExpando(1,"item1", DateTime.MinValue, true),
                GetExpando(2,"item2", DateTime.MaxValue, false)
            };

            var dt = list.ToDataTable("List");

            Assert.IsNotNull(dt);

            using (var dr = dt.CreateDataReader())
            {
                while (dr.Read())
                {
                    if ((int)dr["Id"] == 1)
                    {
                        Assert.AreEqual(1, dr["Id"]);
                        Assert.AreEqual("item1", dr["Name"]);
                        Assert.AreEqual(DateTime.MinValue, dr["Date"]);
                        Assert.AreEqual(true, dr["IsBool"]);
                    }
                    else if ((int)dr["Id"] == 2)
                    {
                        Assert.AreEqual(2, dr["Id"]);
                        Assert.AreEqual("item2", dr["Name"]);
                        Assert.AreEqual(DateTime.MaxValue, dr["Date"]);
                        Assert.AreEqual(false, dr["IsBool"]);
                    }
                }
            }
        }

        private ExpandoObject GetExpando(int id, string name, DateTime date, bool isBool)
        {
            dynamic result = new ExpandoObject();
            result.Id = id;
            result.Name = name;
            result.Date = date;
            result.IsBool = isBool;
            return result;
        }
    }



    public static class Extensions
    {

        /// <summary>
        /// Create a datatable from a list of <see cref="IEnumerable{T}"/>
        /// </summary>
        /// <param name="data">The list can be created from a dictionary with Dictionary.Values.ToList()</param>
        /// <param name="tableName">Name of the data table</param>
        /// <returns></returns>
        public static DataTable ToDataTable(this IEnumerable<dynamic> data, string tableName)
        {
            return data.ToList().ToDataTable(tableName);
        }
        /// <summary>
        /// Create a datatable from a list of ExpandoObjects
        /// </summary>
        /// <param name="list">The list can be created from a dictionary with Dictionary.Values.ToList()</param>
        /// <param name="tableName">Name of the data table</param>
        /// <returns></returns>
        public static DataTable ToDataTable(this List<ExpandoObject> list, string tableName)
        {

            if (list == null || list.Count == 0)
            {
                return null;
            }

            //build columns
            var props = (IDictionary<string, object>)list[0];
            var t = new DataTable(tableName);
            foreach (var prop in props)
            {
                t.Columns.Add(new DataColumn(prop.Key, prop.Value.GetType()));
            }
            //add rows
            foreach (var row in list)
            {
                var data = t.NewRow();
                foreach (var prop in (IDictionary<string, object>)row)
                {
                    data[prop.Key] = prop.Value;
                }
                t.Rows.Add(data);
            }
            return t;
        }
    }

}
```
