---
author: torben
date: 2012-10-22 21:45:21+00:00
draft: false
title: Setting up Wifi on Ubuntu 12.04 Server via command line (yeah, finally some
  technical stuff )
aliases: 
- /2012/10/setting-up-wifi-on-ubuntu-12-04-server-via-command-line-yeah-finally-some-technical-stuff/
categories:
- Commandline
- OS
tags:
- culprits
- facebook
- howto
- Linux
- Ubuntu
- weird errormessages
- WiFi
- Wlan
- xing
---

## Problem:


Setting up a wireless adapter on Ubuntu server 12.04 LTS via CLI isn't THAT easy. Plug'n'Play doesn't simply work. Good thing there is Google. And a lot of helpful sites.


## Target:


Insert the stick (or boot with it) and connect automaticly to the preferred network

<!-- more -->


*This is not a tutorial. And it's realy not for the uninitiated. It's more like a short reminder for me, or people who have a basic idea what's happening here...*


## Setup:

* Software: Vanilla Ubuntu 12.04 Server LTS
* Hardware:  A USB-Wifi Stick (here a TP-Link TL-WN821N)

## What was done:

This has to be done only once

Change to su mode

    
    sudo su


Execute following command, whereas you have to replace the two <> variables with your actual values. It will create a WPA2 Pre Shared Key based on your ESSID and passphrase

    
    wpa_passphrase <yourNetwork> <YourPassphrase> >> /etc/wpa_supplicant.conf


add at the top of the /etc/wpa_supplicant.conf file (above the "network{}") the line

    
    ctrl_interface=/var/run/wpa_supplicant


then exit the su mode

    
    exit


Now we have to check if our settings work:

    
    sudo wpa_supplicant –D wext –i wlan1 –c/etc/wpa_supplicant.conf


If everything works like expected, some message like "CTRL-EVENT-CONNECTED" will appear. Fine. Now lets move on:

Again, enter su mode and edit the file /etc/rc.local

add the following two lines just before the exit 0;

    
    sudo wpa_supplicant –D wext –i wlan1 –c/etc/wpa_supplicant.conf -B
    sudo dhclient -4 wlan1


This should do the trick. Our Wifi will work. Obviously the rc.local could be a little bit more ... niftier. It could, for example check if there is a wifi stick present and if it is active for wlan1. But for a first shot, this will be enough.

One "problem" still exist: When you execute the wpa_supplicant part, I encounter some weird error messages:

    
    ioctl[SIOCSIWENCODEEXT]: Invalid argument


The important part for me: I've got no problems with my wifi. just this message. When I've some time I will investigate a little bit.


## Sources:


[http://ubuntuforums.org/showthread.php?t=1798927](http://ubuntuforums.org/showthread.php?t=1798927)

[http://www.wikihow.com/Set-up-a-Wireless-Network-in-Linux-Via-the-Command-Line](http://www.wikihow.com/Set-up-a-Wireless-Network-in-Linux-Via-the-Command-Line)

[http://caleudum.com/how-to-connect-wifi-using-command-line-on-ubuntubacktrack/](http://caleudum.com/how-to-connect-wifi-using-command-line-on-ubuntubacktrack/)
