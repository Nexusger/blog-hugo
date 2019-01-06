---
author: torben
date: 2012-11-16 23:19:00+00:00
draft: false
title: Windows 7 - HowTo change system HDD to SSD (With built in tools only)
aliases: 
- /2012/11/windows-7-howto-change-system-hdd-to-ssd-with-built-in-tools-only/
categories:
- OS
tags:
- coffee-there should be some!
- facebook
- HDD
- SSD
- WIndows 7
- xing
---

## Ingredients:

* Current Laptop or PC
* New SSD
* External Harddisk (for backup, so it'd better be fast and big)
* One empty DVD (or the recovery disc)
* Bunch of screwdrivers



## Abstract:

* Shrink your HDD partition to a size smaller than the SSD (*)
* Create a recovery disk
* Create a system image with Windows backup on the external Harddisk
* Replace your HDD with the shiny new SSD
* Boot with recovery disk
* Choose to restore from a system backup

(*) The first part is realy important. I ran in a nasty trap when I changed my disks: I shrunk my system partition to fit on the SSD, but while I recovered, the recovery tool told me there wasn't enough space on my target disk. The problem was my own lazyness: I created a new partition on the HDD where I moved all the not needed files.

The system backup tool tries to recover your whole system, not only your system partition. So be sure that all partitions on your HDD in sum aren't bigger than your SSD

HowTo shrink the partitions: [microsoft.com](http://technet.microsoft.com/en-us/magazine/gg309169.aspx)

HowTo shrink if there are "unmovable" files: [brandonchecketts.com](http://www.brandonchecketts.com/archives/how-to-shrink-a-partition-with-unmovable-files-in-windows-7)
