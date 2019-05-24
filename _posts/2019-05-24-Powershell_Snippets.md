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
    

----------------------------------------

## Filesystem basics ##

----------------------------------------

### Delete a folder ###

```powershell
if (Test-Path -Path $Folder) {
    Remove-Item $Folder -Recurse -Force
}
$Build_SourcesDirectory 
```


### Test Net Framework installed ###

This is a bit crap

https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/versions-and-dependencies  

```powershell
Get-Childitem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full'
```

#461814 is for Net472

Can test with https://hub.docker.com/_/microsoft-dotnet-framework-runtime/ 

 
----------------------------------------

## Credits ##

Thanks to [Mohit Goyal](https://mohitgoyal.co/2017/09/08/auto-assembly-versioning-in-visual-studio-team-services-or-vsts-build/) for this one!

_Cheers!_
