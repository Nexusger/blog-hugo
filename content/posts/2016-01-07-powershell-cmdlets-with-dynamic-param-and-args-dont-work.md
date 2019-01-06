---
author: torben
date: 2016-01-07 10:11:44+00:00
draft: false
title: Powershell cmdlets with dynamic param AND $args don't work
aliases: 
- /2016/01/powershell-cmdlets-with-dynamic-param-and-args-dont-work/
categories:
- Commandline
- Computer Stuff
- Elixir
tags:
- dynamicParam
- elixir
- GitHub
- mix
- OpenSource
- powershell
- validateSet
---

Over the weekend I tried to implement auto completion in Elixir's mix (in Windows). Unfortunately I didn't make it without introducing some problems. So I didn't committed my changes to upstream. Currently I try to reach some of the more renowned Elixir/Windows contributors, to discuss the changes .


# Motivation


Under normal circumstances I don't use more mix tasks then test, phoenix.server and release but sometimes you need this weird command, you just can't remember. The command mix help  is your friend here as it shows you all available commands (project aware!). Yet I don't like to look the documentation up, if I need just some information on spelling. For example in the beginning I often tried to start the phoenix project with mix phoenix.start (Hint: that does not work). I am used to auto completion in my development environments so I tried to extend mix  as well.


# Background


As I am using the Powershell for all my command line related tasks and the default file extension of Powershell is ps1, my command mix  execute the mix.ps1  in the Elixir bin folder.


# Approach


Powershell scripts can have auto completion of parameters with an so called [validateSet("Param1","Param2",...)] , which incorporate all valid parameters. Sadly this is of no help, if we have to hard code the possible values for the parameter. A possible solution to this problem is the usage of a DynamicParam with dynamic validateSet (good resource [here](https://blogs.technet.microsoft.com/pstips/2014/06/09/dynamic-validateset-in-a-dynamic-parameter/)). To test my various iterations I wrote down all [test cases](/images/2016-01-07-powershell-cmdlets-with-dynamic-param-and-args-dont-work/testcases.txt) (sorry no automated testing yet).


# Iteration 1




## Changes


If you have a look at the original mix  script ([here](https://github.com/elixir-lang/elixir/blob/master/bin/mix.ps1)) you can see that the script locates the mix.bat , flattens possible array arguments (is this still needed?) and then execute the mix.bat  with the newly flattened arguments.

The first problem we see here is the usage of the $args array. As [Keith Hill](http://stackoverflow.com/users/153982/keith-hill) points out in [this SO comment](http://stackoverflow.com/questions/12326205/accessing-the-args-array-in-powershell#comment-16543787) the $args array "... contain any argument that doesn't map to a defined function parameter...". Which introduces the first problem: The DynamicParam ONLY works for defined function parameters.

I copied the linked resource (again, [here](https://blogs.technet.microsoft.com/pstips/2014/06/09/dynamic-validateset-in-a-dynamic-parameter/)) and moved the old script to the process block. Because we are creating a script and not a function the signature of function Test-DynamicValidateSet {...}  needs to be removed. To generate the validateSet I replaced the line $arrSet = ... with

    
    $mixBatPath = (Get-ChildItem (((Get-ChildItem $MyInvocation.MyCommand.Path).Directory.FullName) + '\mix.bat'))
    $param = 'help', '--names'
    
    $arrSet = $(& $mixBatPath $param[0] $param[1])


This populates the $arrSet  with all valid task. I also changed the value of the variable $ParameterName  to 'Task'  and renamed the variable $Path  to $Task


## Test


A short test shows, the command mix  does work, the command mix help  does not. Reason for that is, we assign the first value to the parameter $Task .


# Iteration 2




## Changes


The call to mix.bat in the last row now get the $Task parameter as well:

    
    & $mixBatPath $Task $newArgs




## Test


mix  works, mixd help  works. Awesome! Lets try auto completion. mix [tab]  ...

This is weird. The auto completion takes it times (this is actually the time mix help --names  takes to return all valid tasks) yet the auto completion fills in file names from that folder... To fix that we need to make it clear, that our dynamic parameter is actually the first parameter. So after we set the $ParameterAttribute.Position = 0  (it was 1) we repeat our test.

mix  works, mixd help  works, mix [tab]  works, mix he[tab]  works also. What about arguments to parameters? like mix help --names ?

    
    mix.ps1: A positional parameter cannot be found that accepts argument "--names".


Damn.


# Iteration 3




## Changes


OK, we need positional arguments. Lets add some.

    
    [Parameter(Mandatory=$false, Position=1)]
    [string]$p1,
    [Parameter(Mandatory=$false, Position=2)]
    [string]$p2,
    [Parameter(Mandatory=$false, Position=3)]
    [string]$p3,
    [Parameter(Mandatory=$false, Position=4)]
    [string]$p4,
    [Parameter(Mandatory=$false, Position=5)]
    [string]$p5,
    [Parameter(Mandatory=$false, Position=6)]
    [string]$p6


I don't like that approach, because this script will fail on having more than seven parameters (our dynamic and $p1  - $p6 ) with the aforementioned error message.

We also have to forward our new parameters to the mix.bat :

    
    & $mixBatPath $Task $p1 $p2 $p3 $p4 $p5 $p6


OK, besides the now unused "flatten possible array parameter" logic and our "it will fail on having eight or more parameters" problem, how good are we?


## Test


All tests in the test cases pass. Yet we have some unfinished problems.


# Problems with this solution





1. We can have only a fixed amount of parameters. This is not a big problem (as we can add more parameters in the signature), but this is neither elegant nor good practice.
2. We now completely omit the "flatten array logic". I have to admit, I'm not sure if  this is still needed, so I asked the original contributor of this logic but still wait for response.
3. Most of the code was copied from our resource. We clearly added some of our own logic, yet we probably shouldn't use this code without asking for permissions. I asked the author if I could use this snippet and wait for a response.
4. Even if I omit the "flatten array logic" I tripled the Lines Of Code. I don't know if the auto complete feature is worth this much code (read about [code as a liability here](http://blogs.msdn.com/b/elee/archive/2009/03/11/source-code-is-a-liability-not-an-asset.aspx))

As soon as the problem 3 is clarified I will upload the file here. As soon as the other problems are clarified (and/or fixed) I create a pull request in GitHub to upstream the changes.
