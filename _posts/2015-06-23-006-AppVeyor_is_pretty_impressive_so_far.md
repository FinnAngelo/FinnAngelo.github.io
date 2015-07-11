---
layout: post
title: "#006: AppVeyor is pretty impressive so far"
published: true
---

Build tools and err... stuff

I have been playing with AppVeyor (as per Hanselman at 
http://www.hanselman.com/blog/AppVeyorAGoodContinuousIntegrationSystemIsAJoyToBehold.aspx
) and I'm liking it. 

I have mostly been playing with it on my ZXSpectrum project;

* With [FAKE - F# Make - A DSL for build tasks](http://fsharp.github.io/FAKE/), it is _awesome easy_
  * I can test my build on my local box without too much bother, commit to github and it works  
    The exception to that is the NewLine issue; I still need to look at this.
* I prefer [NUnit](http://www.nunit.org/) to MSTest for unit testing.  
  This is possibly because the FAKE documentation for NUnit is better
* I have also got a [Hello World Build](https://github.com/FinnAngelo/Tomfoolery/tree/master/HelloWorldBuild) 
  in my [FinnAngelo/Tomfoolery](https://github.com/FinnAngelo/Tomfoolery) project
* When playing with the [Jenkins-CI](https://jenkins-ci.org/) build tool on Linux, I was a little frustrated 
  that I couldn't unit test database stuff - I use MS SQL Server and stored procedures by choice - but adding 
  other software in a build is merely a chocolatey `cinst` away

And my favourite bit. 

* The build badge [![Build status](https://ci.appveyor.com/api/projects/status/c7pd6su7824jiuv8?svg=true)](https://ci.appveyor.com/project/FinnAngelo/dalify) is brilliant.  
  (This is linking to my DALify project build) 
