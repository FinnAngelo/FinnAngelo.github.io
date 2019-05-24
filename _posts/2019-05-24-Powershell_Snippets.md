---
layout: post
title: "Powershell Snippets"
published: true
---

Yeah - I'm lovin' the powerhell goodness...

But I need a way to remember all of these bits and bobs of snippets.  
I guess this is what the internet is for!

This will obviously update as I find more 

----------------------------------------
+ [Filesystem basics](#Filesystem-basics)
    + [Delete a folder](#Delete-a-folder)
    + [Test Net Framework installed](#Test-Net-Framework-installed)
+ [Credits](#Credits)    

----------------------------------------
<a name="Filesystem-basics">
## Filesystem basics ##
</a>
----------------------------------------
<a name="Delete-a-folder"></a>
### Delete a folder ###

```powershell
if (Test-Path -Path $Folder) {
    Remove-Item $Folder -Recurse -Force
}
$Build_SourcesDirectory 
```

<a name="Test-Net-Framework-installed"></a>
### Test Net Framework installed ###

This is a bit crap

https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/versions-and-dependencies  

```powershell
Get-Childitem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full'
```

#461814 is for Net472

Can test with [mcr.microsoft.com/dotnet/framework/runtime:4.8](https://hub.docker.com/_/microsoft-dotnet-framework-runtime/)

 
----------------------------------------
<a name="Credits"></a>
## Credits ##

_Cheers!_
