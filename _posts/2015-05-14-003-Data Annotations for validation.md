---
layout: post
title: "#003: Data Annotations for validation"
published: true
---

Check it out!  
<//http://stackoverflow.com/questions/3782678/how-can-i-use-the-data-validation-attributes-in-c-sharp-in-a-non-asp-net-context>

I was playing with this tonight, and quite liked how simple it all is:

-----------------------
DataAnnotation_Tests.cs
-----------------------

```csharp

//DataAnnotationTests.cs
using System;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;

namespace TestingNamespace
{
    [TestClass]
    public class DataAnnotationTests
    {
        [TestMethod]
        public void TryValidate_Test()
        {
            //http://stackoverflow.com/questions/3782678/how-can-i-use-the-data-validation-attributes-in-c-sharp-in-a-non-asp-net-context
            //given 
            var tw = new TestObject();
            //when
            var vc = new ValidationContext(tw, null, null);

            var validationResults = new List<ValidationResult>();
            var result = Validator.TryValidateObject(tw, vc, validationResults, true);

            //then
            Assert.IsNotNull(result);
            Assert.IsNotNull(validationResults);
            Assert.AreEqual(1, validationResults.Count);
        }
    }

    public class TestObject
    {
        [Required()]
        [MaxLength(6)]
        public string ThingWord { get; set; }
    }
}
```

