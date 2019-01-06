---
author: torben
date: 2011-11-03 09:48:32+00:00
draft: false
title: How to install HandBrake on Ubuntu 11.10
aliases: 
- /2011/11/how-to-install-handbrake-on-ubuntu-11-10/
categories:
- OS
tags:
- Handbrake
- howto
- ppa
- tutorial
---

If not already happened: Install the synaptic package manager (search for synaptic in the Ubuntu software center)

Download [this](http://packages.debian.org/squeeze/libnotify1) package according to your architecture. If you are not sure which architecture you need, try amd64 or i386. This are the two most used architectures.

Install the package by double clicking.

Open a terminal and execute `sudo add-apt-repository ppa:stebbins/handbrake-releases`

Open synaptics package manager, go to preferences / package sources / other software. There should be two entrys like `http://ppa.launchpad.net/stebbins/handbrake-releases/ubuntu`. Mark the first one and click on `change`. Alter the distribution code from `oneiric` to `natty`. Do the same for the second entry. Apply the changes

Back in synaptic, click on reload. Now you can search for handbrake in the package search

tadaaa
