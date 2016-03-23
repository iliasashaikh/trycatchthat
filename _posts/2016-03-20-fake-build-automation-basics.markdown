---
layout: post
title:  "Build Automation for .NET - Basics"
date:   2016-03-20 08:39:56
categories: fsharp FAKE
---

This is going to be the first in a series of posts about how to use [FAKE](http://fsharp.github.io/FAKE/ "FAKE") for builds. I currently use Nant, and its served me well for a fair few years, and arguably I can do all I need to if I continue with Nant. It has a load of extensions and heck, with the 'script' tag in Nant, I can use C# for a fair few tasks without ever having to run the trouble of creating my own extensions.

And there in lies the rub - Nant (and msbuild) tasks are just a means of calling into functions written in a programming language. With build systems based on a first class programming lanuguage, we can cut the xml middle man out. The alternatives that I have seen so far in the .NET space are [psake](https://github.com/psake/psake "psake"), possibly [rake](https://github.com/ruby/rake) and of course [FAKE](http://fsharp.github.io/FAKE/ "FAKE"). I have wanted to learn F# for a while now with all the functional goodness that it comes with. So, I plumped for FAKE. Plus, I am hoping that I will be able to edit and debug Fake scripts from Visual Studio. 

In this first post, I want to first look at some basic concepts underlying build automation.

### What is a build automation and why do we need it?
After all in the .NET world we have the Visual Studio to 'build' our projects. The answer is that if you are working on a simple project, you may well not require any thing else apart from Visual Studio. However when your software evolves to a certain degree of complexity you will increasingly have more complex workflows and solely building and running the code from your IDE will no longer be sufficient. Once you start distributing your software, you will want to continuously build you app, run tests, move build artefacts to other locations etc. Although this can be achieved using scripts, its better to have have a tool that is built specifically for the purpose and wraps these commands for you, gives you good feedback etc.

### What about build automation servers, where do servers like TeamCity, Jenkins etc fit into the picture?
Yes, this is confusing. In my view there are at least 2 categories of build tools

1. **Build DSLs** - 'Make','Ant','Nant','Rake','FAKE', 'MSbuild' etc. fall into this category. These are created and maintained by developers. These build scripts are treated as code and checked into you version control system.

2. **Build Automation Servers** - These are tools such as 'Jenkins', 'TeamCity', 'TFS' etc. These are a class of server applications (generally web based) that are fairly sophisticated. They are used to schedule and run build scripts. They can 'watch' your source control repositories, automatically trigger builds, run unit tests and so on.

There is some degree of overlap between these tools, but each excels in the areas that they are designed for. I personally like to do as much as I can in scripts, simply because as a developer I find it easier to maintain these. I can track history of my scripts with git.

### What about Octopus deploy?
'Octopus Deploy' is a 'release automation' tool that relies on a powershell scripts to perform installs and configuration of your applications.

In my view, all these tools complement each other and optimally using these makes it easy to the automate the repetitive tedious tasks allowing us to focus on our favourite activity - writing code.

As I said earlier, my goal is to explore FAKE as my build DSL and hopefully in the following posts I will take a closer look at FAKE.
