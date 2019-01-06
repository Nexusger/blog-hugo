---
author: torben
date: 2011-11-16 08:21:13+00:00
draft: false
title: Backspace in Firefox and Linux
aliases: 
- /2011/11/backspace-in-firefox-and-linux/
categories:
- OS
tags:
- Backspace
- Firefox
- Linux
- Ubuntu
---

Anyone who is used to firefox under windows (and probably mac) is also used to the backspace key. under windows, firefox will go one page backwards if you hit backspace.

Under Linux if you hit backspace, there happens... nothing. Which is disgusting.

I found a [website](http://www.im-web-gefunden.de/2007/11/28/firefox-2-und-die-backspace-taste-unter-linux/) (german) which describes how to change the behavior:

<!-- more -->



1. Type in your adress bar: "about:config"
2. Confirm the warning: [![](/images/2011-11-16-backspace-in-firefox-and-linux/aboutconfig-150x150.png)
](/images/2011-11-16-backspace-in-firefox-and-linux/aboutconfig.png)
3. Search for "Backspace": [![](/images/2011-11-16-backspace-in-firefox-and-linux/backspace2-150x98.png)](/images/2011-11-16-backspace-in-firefox-and-linux/backspace2.png)
4. Alter "browser.backspace_action" by double click from "2" to "0": [![](/images/2011-11-16-backspace-in-firefox-and-linux/value-150x150.png)]
(/images/2011-11-16-backspace-in-firefox-and-linux/value.png)
5. Confirm

You are done, your backspace is working again.
