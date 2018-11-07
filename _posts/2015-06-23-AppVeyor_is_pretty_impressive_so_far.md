---
layout: post
title: "AppVeyor is pretty impressive so far"
published: true
---

<a href="https://flic.kr/p/64Qc2m" title="Building to Heaven">![Building to Heaven](https://raw.githubusercontent.com/FinnAngelo/FinnAngelo.github.io/master/_posts/images/3325135786_9f28847c3c_z.jpg)</a>

**Update: None of these projects exist anymore, but the sentiment holds.  And I use VSTS now.**

----------------------------------------

## Build tools and err... stuff ##

I have been playing with [AppVeyor](https://ci.appveyor.com) - as per Hanselman at [AppVeyor - A good continuous integration system is a joy to behold](http://www.hanselman.com/blog/AppVeyorAGoodContinuousIntegrationSystemIsAJoyToBehold.aspx
) - and I'm liking it. 

Things I found with it on my [ZXSpectrum](https://github.com/FinnAngelo/ZXSpectrum) project;

* With [FAKE - F# Make - A DSL for build tasks](http://fsharp.github.io/FAKE/), it is _awesome easy_
  * I can test a build on my local box without too much bother, commit to github and it works  
    The exception to that is the NewLine issue; I still need to look at this.
* I prefer [NUnit](http://www.nunit.org/) to MSTest for unit testing.  
  This is possibly because the FAKE documentation for NUnit is better
* I have also got a [Hello World Build](https://github.com/FinnAngelo/Tomfoolery/tree/master/HelloWorldBuild) 
  in my [FinnAngelo/Tomfoolery](https://github.com/FinnAngelo/Tomfoolery) project
* When playing with the [Jenkins-CI](https://jenkins-ci.org/) build tool on Linux, I was a little frustrated 
  that I couldn't unit test database stuff - I use MS SQL Server and stored procedures by choice - but adding 
  other software in a build is merely a [chocolatey](https://chocolatey.org/) `cinst` away

And my favourite bit. 

* The build badge [![Build status](https://ci.appveyor.com/api/projects/status/c7pd6su7824jiuv8?svg=true)](https://ci.appveyor.com/project/FinnAngelo/dalify) is brilliant. It's so _gratifying_ for the ego...  
  This is linking to the build for my [DALify project](https://github.com/FinnAngelo/DALify). 

----------------------------------------

## Bonus pic ##

<a href="https://flic.kr/p/9YhRK2" title="Building Trust">![Building Trust](https://raw.githubusercontent.com/FinnAngelo/FinnAngelo.github.io/master/_posts/images/5887867043_8386af87b1_z.jpg)</a>  
This has **nothing** to do with builds or servers, or anything really. But it's cute.
