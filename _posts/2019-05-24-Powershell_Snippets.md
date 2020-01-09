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

+ [Change password](#Change-password)
+ [Comments and more](#Comments-and-more)
+ [Filesystem basics](#Filesystem-basics)
  + [Delete a folder](#Delete-a-folder)
  + [Test Net Framework installed](#Test-Net-Framework-installed)
+ [Credits](#Credits)

----------------------------------------

## Change password ##

I think this could be useful in docker instances...?

[Pureinfotech - Change account password with powershell](https://pureinfotech.com/change-account-password-powershell-windows-10/)

```powershell
$Password = Read-Host "Enter the new password" -AsSecureString

$UserAccount = Get-LocalUser -Name "admin"
$UserAccount | Set-LocalUser -Password $Password
```

----------------------------------------

## Comments and more ##

[Comments and more in powershell](https://www.red-gate.com/simple-talk/sysadmin/powershell/comments-and-more-in-powershell/)

This only works at the start of scripts, but is pretty cool

```powershell
#Requires -RunAsAdministrator
#Requires -Version 5.0 
```

----------------------------------------

## Filesystem basics ##

### Delete a folder ###

```powershell
if (Test-Path -Path $Folder) {
    Remove-Item $Folder -Recurse -Force
}
```

### Test Net Framework installed ###

This is a bit crap

<https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/versions-and-dependencies>

```powershell
Get-Childitem 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full'
```

#461814 is for Net472

Can test with [mcr.microsoft.com/dotnet/framework/runtime:4.8](https://hub.docker.com/_/microsoft-dotnet-framework-runtime/)

----------------------------------------
<a name="Credits"></a>
## Credits ##

_Cheers!_
