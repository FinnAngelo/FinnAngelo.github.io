---
layout: post
title: "Pester and Powershell"
published: true
---
<figure style="float:right; margin-left:3em; width:50%;">
	<a href="https://cps-static.rovicorp.com/3/JPG_400/MI0000/090/MI0000090531.jpg">
		<img src="https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/BNL_BornOnAPirateShip.jpg" alt="Bare Naked Ladies - Born on a Pirate Ship album cover"/>
	</a>
	<figcaption>Fun album, but daaang that kid looks annoying...</figcaption>
</figure>	
The transition from using manky old batch files through to powershell hasn't been a smooth one for me.  

It's just too easy to whip up a *.bat file and get running, instead of using that fiddly "learnin'" that us programmers are supposed to be good at. 

------------------------------------------------

Enter [Pester](https://github.com/pester/Pester)
------------------------------------------------

Yes - the ubiquitous testing framework for powershell. It's been helpful so far...

Note to self - although pre-installed on Windows 10, update it with:

```powershell
Install-Module -Name Pester -Force -SkipPublisherCheck
```

----------------------------------------

## Testing ExecutionPolicy ##


This is a bit goofy, and helpful to explore the ExecutionPolicy stuff in powershell.

Make a file called ... well, it says in the comments, but call it  **`ExecutionPolicy.Tests.ps1`**  
I tend to use a formula for these test files

```powershell
#cd C:\Users\jfinnangelo\Desktop\pester
#Set-ExecutionPolicy Unrestricted

#Invoke-Pester -Script ./ExecutionPolicy.Tests.ps1
#Invoke-Pester -?
#get-help Invoke-Pester -examples

# Pester tests
Describe 'ExecutionPolicy' {

  Context "Set ExecutionPolicy" {
    It "Given valid Policy '<Policy>', it returns '<Expected>'" -TestCases @(
      @{ Policy = 'unrestricted'; Expected = 'Unrestricted' }
      @{ Policy = 'default'  ; Expected = 'Restricted' }
    ) {
      param ($Policy, $Expected)

      Set-ExecutionPolicy $Policy
      Get-ExecutionPolicy | Should Be $Expected
    }
  }
}

Set-ExecutionPolicy Default
```

Things to notice:

+ Setting execution policy at the start, and resetting it at the end

----------------------------------------

## How Modules work ##


Make a **`Module.Library.ps1`** file:

```powershell
Set-StrictMode  –version Latest

function Get-HelloValue($value)
{
    return "Hello " + $value
}
```

Things to notice:

+ Did you notice the `Set-StrictMode` at the start? I like that.
+ Syntax for a function
+ Syntax for parameters
+ string joining

Make a **`Module.Tests.ps1`** file:

```powershell
#cd C:\Users\jfinnangelo\Desktop\pester
#Set-ExecutionPolicy Unrestricted
#Set-ExecutionPolicy Default

#Invoke-Pester -Script ./Module.Tests.ps1
#Invoke-Pester -?
#get-help Invoke-Pester -examples

#https://stackoverflow.com/questions/8501225/how-to-call-a-function-in-another-powershell-script-when-executing-powershell-sc

function Get-HelloWorld
{
    return "Hello World"
}

# Pester tests
Describe 'Explore Powershell Modules' {

  BeforeAll {
    Set-StrictMode –version Latest
    . .\Module.Library.ps1
  }

  AfterAll {
  }

  Context "Check functions and referenced files" {
    
    It "Given the HelloWorld function, Then there is a result" {
      $result = Get-HelloWorld
      $result | Should Be "Hello World"
    }
        
    It "Given the HelloValue function in a separate file, Then there is a result" {
      $result = Get-HelloValue("Value")
      $result | Should Be "Hello Value"
    }
  }

  Context "Check StrictMode" {
    It "Given an undefined value, Then there is an exception" {
      { $result = $undefinedValue } | Should Throw "cannot be retrieved because it has not been set"
    }
  }
  
  Set-ExecutionPolicy Default
}
```

Things to notice:

+ `BeforeAll` and `AfterAll` for tests
+ Referencing the `Module.Library.ps1` file  with a `.`
+ The `Throw` test

Here is the successful test run:  
![Powershell - completed run of Module.Tests.ps1](https://github.com/FinnAngelo/FinnAngelo.github.io/raw/master/_posts/images/Completed_Run_of__Module_Tests_ps1.png)

----------------------------------------

_Cheers!_