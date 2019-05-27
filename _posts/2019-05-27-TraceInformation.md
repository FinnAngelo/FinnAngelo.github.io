---
layout: post
title: "Logging with System.Diagnostics, .Net 1.1"
published: true
---

I always forget the config syntax for the really simple built in logging in c#  

It's quick and dirty, and doesn't need 'Yet-Another-Logging-Framework(tm)'.  
And yeah, for real work I prefer [Serilog](https://serilog.net)

Helpful Hint: Stay away from the `Debug.Write` stuff. It only works in debug mode 
which is _frekkin' annoying_ when you are on prod and desperately need the logging.  
Use the log level/filter type in the config.

-----
Trace
-----

```xml
  <system.diagnostics>
    <trace autoflush="true">
      <listeners>
        <add name="BibiddyBoo" type="System.Diagnostics.TextWriterTraceListener" initializeData="c:\temp\MyProject.log" />
      </listeners>
    </trace>
  </system.diagnostics> 
```

Or with a bit more detail
  
```xml
  <system.diagnostics>
    <sharedListeners>
      <add name="ErrorListener"
           type="System.Diagnostics.XmlWriterTraceListener"
           initializeData="c:\temp\MyProject.Error.svclog" >
        <filter type="System.Diagnostics.EventTypeFilter"
                initializeData="Error" />
      </add>
      <add name="VerboseListener"
           type="System.Diagnostics.TextWriterTraceListener"
           traceOutputOptions="DateTime"
           initializeData="c:\temp\MyProject.Verbose.log" />
    </sharedListeners>
    <trace autoflush="true" indentsize="8">
      <listeners>
        <clear/> 
        <add name="VerboseListener" />
        <add name="ErrorListener" />
      </listeners>
    </trace>
  </system.diagnostics>
  ```
 
  and it can be used to log unhandled exceptions in web forms with the `Global.asax.cs` file
  
  ```csharp
using System.Diagnostics;

//etc...

void Application_Error(object sender, EventArgs e)
{
  var ex = Server.GetLastError();
  //Trace.TraceInformation("Information");
  Trace.TraceError(ex.ToString());
}
```

-----------
TraceSource
-----------


```xml
  <system.diagnostics>
    <sources>
      <source name="MyProject" switchValue="All" >
        <listeners>
          <clear/>
          <add name="VerboseListener"/>
          <add name="ErrorListener"/>
        </listeners>
      </source>
    </sources>
    <sharedListeners>
      <add name="ErrorListener"
           type="System.Diagnostics.XmlWriterTraceListener"
           traceOutputOptions="DateTime,Callstack,LogicalOperationStack,ProcessId,ThreadId,Timestamp"
           initializeData="c:\temp\MyProject.Error.svclog" >
        <filter type="System.Diagnostics.EventTypeFilter"
                initializeData="Error" />
      </add>
      <add name="VerboseListener"
           type="System.Diagnostics.TextWriterTraceListener"
           traceOutputOptions="DateTime"
           initializeData="c:\temp\MyProject.Verbose.log" />
    </sharedListeners>
    <trace autoflush="true" indentsize="8" />
  </system.diagnostics>
  ```
  
  and it can be used to log unhandled exceptions in web forms with the `Global.asax.cs` file
  
  ```csharp
void Application_Error(object sender, EventArgs e)
{
  var ex = Server.GetLastError();

  var log = new TraceSource("MyProject");
  log.TraceEvent(TraceEventType.Verbose, 0, "Howdy, Verbose");
  log.TraceInformation("Howdy, Information");
  log.TraceData(TraceEventType.Error, 1000, ex);
}
```

----------------------------------------
_Cheers!_
