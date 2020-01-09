---
layout: post
title: "Handy Dandy Serialization helper"
categories: sql 
published: true
---

<a href="https://pagingfunmums.com/2017/11/02/diy-cereal-killer-halloween-costume/" title="DIY Cereal killer costume" style="float:right; margin-left:3em;"><img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/DIY-Cereal-Killer-Halloween-Costume.jpg" alt="Cereal Killer" style="width:50%;"></a>

This 'Cereal Killer' joke was hilarious when I made it in the 90's, so I figure I can use it again... with better graphics this time.

I don't know what the performance is like on these extension methods, or even if it is thread safe.  
Use at your own risk, when you need a quick and dirty serializer.

```csharp 
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;
using System.Runtime.Serialization.Json;
using System.Text; 

namespace MyNamespace
{
	public static class SerializationExtensions
	{
		public static byte[] ToByteArray(this object obj)
		{
			if (obj == null)
			{
				return null;
			}

			BinaryFormatter binaryFormatter = new BinaryFormatter();

			using (MemoryStream memoryStream = new MemoryStream())
			{
				binaryFormatter.Serialize(memoryStream, obj);
				return memoryStream.ToArray();
			}
		}

		public static T FromByteArray<T>(this byte[] byteArray) where T : class
		{
			if (byteArray == null)
			{
				return default(T);
			}
			BinaryFormatter binaryFormatter = new BinaryFormatter();
			using (MemoryStream memoryStream = new MemoryStream(byteArray))
			{
				return binaryFormatter.Deserialize(memoryStream) as T;
			}
		}

		public static string ToJson<T>(this T instance)
		{
			var serializer = new DataContractJsonSerializer(typeof(T));
			using (var ms = new MemoryStream())
			{
				serializer.WriteObject(ms, instance);
				var encoding = new UTF8Encoding();
				return encoding.GetString(ms.ToArray());
			}
		}

		public static T FromJson<T>(this string json)
		{
			var serializer = new DataContractJsonSerializer(typeof(T));
			var encoding = new UTF8Encoding();
			using (var ms = new MemoryStream(encoding.GetBytes(json)))
			{
				return (T)serializer.ReadObject(ms);
			}
		}
	}
}
``` 

----------------------------------------

## Credits ##

I have no idea where I picked this up... it's pretty old  
If you find this useful, go be nice to someone. Pay it forward.

_Cheers!_
