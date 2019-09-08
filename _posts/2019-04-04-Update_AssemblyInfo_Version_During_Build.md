---
layout: post
title: "Update AssemblyInfo.cs Version during an Azure Devops build"
published: false
---

Currently I am spending quite a bit of time setting up builds with the Azure devops tools. Its great!

----------------------------------------

## Updating the assembly version in the old csproj files ##

Although it is easy to set the version number for assemblies that use the _'new'_ csproj format, its a bit uglier with older code.

It requires updating the AssemblyInfo.cs file during the build, _after_ the file has been committed to version control.  
I'm not really comfortable with that, which is (one of the reasons) why I like the new csproj files.
They can just reference an environmental variable.

Anyways!  
Here is the powershell you can put into a step of the build

```powershell
$AllVersionFiles = Get-ChildItem $(Build.SourcesDirectory) AssemblyInfo.cs -recurse

foreach ($file in $AllVersionFiles)
{
    (Get-Content $file.FullName) |
    %{$_ -replace 'AssemblyVersion\("[0-9]+(\.([0-9]+|\*)){1,3}"\)', "AssemblyVersion(""$(Build.BuildNumber)"")" } |
    %{$_ -replace 'AssemblyFileVersion\("[0-9]+(\.([0-9]+|\*)){1,3}"\)', "AssemblyFileVersion(""$(Build.BuildNumber).0"")" } |
    %{$_ -replace 'AssemblyInformationalVersion\("[0-9]+(\.([0-9]+|\*)){1,3}"\)', "AssemblyInformationalVersion(""$(Build.BuildNumber)"")" } |
    Set-Content $file.FullName -Force
}
```

----------------------------------------

## Credits ##

Thanks to [Mohit Goyal](https://mohitgoyal.co/2017/09/08/auto-assembly-versioning-in-visual-studio-team-services-or-vsts-build/) for this one!

_Cheers!_
