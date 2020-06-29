---
layout: post
title: "Other Cute Bits-and-Bobs"
published: true
---

More goodness - but not powershell...

This will obviously update as I find more 

----------------------------------------

+ [Taskbar directory](#Taskbar-directory)
+ [Chrome incognito shortcut](#Chrome-incognito-shortcut)
+ [EF Core Scaffold Table](#EF-Core-Scaffold-Table)
+ [Credits](#Credits)    

----------------------------------------

## Taskbar directory ##

So where do all the shortcuts on the taskbar live?

`C:\Users\jfinnangelo\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar`

----------------------------------------

## Chrome incognito shortcut ##

How do I open chrome as incognito from a shortcut?

Clone a shortcut to chrome, and change the `target` field to this:  
`"C:\Program Files (x86)\Google\Chrome\Application\chrome_proxy.exe" --profile-directory=Default -incognito https://mail.google.com https://www.onenote.com`

Note that it opens **two** web pages in separate tabs - harrah!  
I keep the shortcut in the Taskbar directory

----------------------------------------

## EF Core Scaffold Table ##

I can never remember this, but its pretty cool!

```powershell
#https://docs.microsoft.com/en-us/ef/core/get-started/aspnetcore/existing-db
Scaffold-DbContext "Server=MyServer\MyInstance;Database=MyDB;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables MyTable1,MyTable2
```

----------------------------------------
<a name="Credits"></a>
## Credits ##

_Cheers!_
