---
author: torben
date: 2012-02-17 16:37:24+00:00
draft: false
title: KSH the second
aliases: 
- /2012/02/ksh-the-second/
categories:
- Commandline
tags:
- culprits
- korn shell
- kornshell
- KSH
- Linux
---

Woe unto me! Earlier this day I debugged a korn shell script and were not able to find the culprit. With debugging mode

    
    set -x


I was able to identify, that one of my variables were actualy not recognized as such. After a little bit of tearing my hair out I finaly found the little scallywag:

<!-- more -->

I had inserted an unnecessary space between the variable name and the equal-sign. Therefore the interpreter didn't understand what I wanted him to do. So remember:

    
    "variable = value" != "variable=value"
