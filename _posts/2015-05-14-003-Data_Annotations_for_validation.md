---
layout: post
title: "#003: Data Annotations for validation"
published: true
---
<a href="https://www.flickr.com/photos/tutescin/3797594257" title="Gatekeepers by Matías Ávalos, on Flickr"><img src="../images/003_Door_Guardians.jpg" width="1024" height="479" alt="Gatekeepers"></a>  
I think the data annotations (sometimes known as `System.ComponentModel.DataAnnotations`) is a great idea; in the past I have played around with custom validation rules, but it was always difficult to find out how to trigger the validation event when you weren't in an asp.net page (webforms or mvc)  
I was playing with this tonight, and quite liked how simple it all is:

-----------------------
DataAnnotationTests.cs
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
            //given 
            var t = new TestObject();
            var vc = new ValidationContext(t, null, null);
            var validationResults = new List<ValidationResult>();
            
            //when
            var result = Validator.TryValidateObject(t, vc, validationResults, true);

            //then
            Assert.AreEqual(false, result, "Obviously failed validation");
            Assert.IsNotNull(validationResults);
            Assert.AreEqual(1, validationResults.Count, "It only includes the 'required' validation");
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

References:   
<http://stackoverflow.com/questions/3782678/how-can-i-use-the-data-validation-attributes-in-c-sharp-in-a-non-asp-net-context>
