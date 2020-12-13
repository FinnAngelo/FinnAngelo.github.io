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

+ [Basics](#Basics)
  + [ExecutionPolicy](#ExecutionPolicy)
  + [Install Chocolatey](#Install-Chocolatey)
+ [Working with SqlServer](#Working-with-SqlServer)
+ [Change password](#Change-password)
+ [Comments and more](#Comments-and-more)
+ [Filesystem basics](#Filesystem-basics)
  + [Delete a folder](#Delete-a-folder)
  + [Test Net Framework installed](#Test-Net-Framework-installed)
+ [Persistent profile](#Persistent-profile)
+ [List all PCs on Domain](#List-all-PCs-on-Domain)
+ [dotnet build](#dotnet-build)
+ [Credits](#Credits)
  + [Links](#Links)

----------------------------------------

## Basics ##

### ExecutionPolicy ##

```powershell
Get-ExecutionPolicy -?  

Set-ExecutionPolicy -? 

#Unrestricted 
#Default 
```

### Install Chocolatey ###

Just do it, ok?

[chocolatey - install](https://chocolatey.org/install)

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')) 
```

----------------------------------------

## Working with SqlServer ##

I think this part will end up as its own post...  
[Install SQL Server Powershell module](https://docs.microsoft.com/en-us/sql/powershell/download-sql-server-ps-module)

```powershell
Install-Module -Name SqlServer -AllowClobber
```

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

## Persistent profile ##

This is for all those snippets I want to always be available, like my user secrets stuff

+ [MS Powershell guy - understanding the six profiles](https://devblogs.microsoft.com/scripting/understanding-the-six-powershell-profiles/)
+ [Red-gate - persistent powershell](https://www.red-gate.com/simple-talk/sysadmin/powershell/persistent-powershell-the-powershell-profile/)

### Adding a profile ###

To make a new profile for `CurrentUserAllHosts`  
This is at `%USERPROFILE%\Documents\WindowsPowerShell\profile.ps1` (but not really because my `Documents` folder isn't on my home drive...)

```powershell
if (!(Test-Path $Profile.CurrentUserAllHosts)) {
    New-Item -Type file -Path $Profile.CurrentUserAllHosts -Force }
```

Don't forget about the executionpolicy

### Profile locations ###

| Profile	                | Location
|-------------------------|---------
| AllUsersAllHosts        | $PsHome\profile.ps1
| AllUsersCurrentHost	    | $PsHome\HostId_profile.ps1
| CurrentUserAllHosts	    | $Home\Documents\WindowsPowerShell\profile.ps1
| CurrentUserCurrentHost  | $Home\Documents\WindowsPowerShell\HostId_profile.ps1

----------------------------------------

## List all PCs on Domain ##

```powershell
#import-module ActiveDirectory
#Get-Help Get-ADComputer -Full
cls
Get-ADComputer -Filter * -Property * | Format-Table Name,OperatingSystem,OperatingSystemServicePack,OperatingSystemVersion -Wrap –Auto #| Out-File 'Out-File.txt'
```

----------------------------------------

## dotnet build ##

```powershell
$versionNumber = $(Get-Date -format 'yyMM.ddhh.mmss')
dotnet build "E:\blah\MyProj.csproj" -p:Version=$versionNumber
dotnet pack "E:\blah\MyProj.csproj" -p:PackageVersion=$versionNumber --no-build 
```
----------------------------------------

## Credits ##

### Links ###

Pester

+ [Github - pester](https://github.com/pester/Pester)
+ [heyscriptingguy - what is pester](https://blogs.technet.microsoft.com/heyscriptingguy/2015/12/14/what-is-pester-and-why-should-i-care/)
+ [Red-gate - powershell unit testing](https://www.red-gate.com/simple-talk/sysadmin/powershell/practical-powershell-unit-testing-getting-started)
+ [McpMag - BeforeEach and AfterEach](https://mcpmag.com/articles/2017/02/16/run-code-before-and-after-a-pester-test.aspx)

Secrets

+ [kloud - using saved credentials securely](https://blog.kloud.com.au/2016/04/21/using-saved-credentials-securely-in-powershell-scripts)

_Cheers!_
