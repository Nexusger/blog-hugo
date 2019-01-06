---
author: torben
date: 2016-06-29 10:16:09+00:00
draft: false
title: 'Solved: ImageJ / Fiji error: "IllegalArgumentException: adding a container
  to a container on a different GraphicsDevice"'
aliases: 
- /2016/06/solved-imagej-fiji-error-illegalargumentexception-adding-a-container-to-a-container-on-a-different-graphicsdevice/
categories:
- Computer Stuff
- Java
tags:
- Bugs
- Fiji
- ImageJ
- Solved
---

For my master thesis I am experimenting with [ImageJ](http://imagej.net/Welcome) / [Fiji](http://fiji.sc/). While working with some image registration algorithms I happend to run in a strange bug I couldn't explain to me:

    
    (Fiji Is Just) ImageJ 2.0.0-rc-49/1.51c; Java 1.8.0_66 [64-bit]; Windows 10 10.0; 476MB of 9115MB (5%)
     
    java.lang.IllegalArgumentException: adding a container to a container on a different GraphicsDevice
    	at java.awt.Component.checkGD(Component.java:1185)
    	at java.awt.Container.addImpl(Container.java:1093)
    	at java.awt.Container.add(Container.java:417)
    	at ij.gui.ImageWindow.add(ImageWindow.java:516)
    	at ij.gui.ImageWindow.<init>(ImageWindow.java:88)
    	at ij.gui.StackWindow.<init>(StackWindow.java:28)
    	at ij.ImagePlus.setStack(ImagePlus.java:673)
    	at SIFT_Align.run(SIFT_Align.java:378)
    	at ij.IJ.runUserPlugIn(IJ.java:216)
    	at ij.IJ.runPlugIn(IJ.java:180)
    	at ij.Executer.runCommand(Executer.java:137)
    	at ij.Executer.run(Executer.java:66)
    	at java.lang.Thread.run(Thread.java:745)
    




## Workaround / Fix:


If you happen to have this bug make sure all instances of ImageJ / Fiji windows are on the same monitor. This bug only occured to me when my Fiji toolbar window was on my main monitor and the plugin windows (Image Sequence loader and Linear Stack Allignment with SIFT) were on my secondary screen.


## Reproduce:


If you want to reproduce this issue, use a system with the above mentioned spec (see stacktrace).



1. Open Fiji on the main display
2. Use "File / Import / Image Sequence..." to load at least two images
3. Execute "Plugin / Registration / Linear Stack Allignment with SIFT" on the images
4. When the registration is performed, move all Fiji windows but the Fiji toolbar to a second display
5. Close all the Fiji windows but keep the Fiji toolbar open
6. Repeat step 2) and 3)
7. Exception gets thrown



## Resources:


[Related bug in the ImageJ forum](http://forum.imagej.net/t/add-slice-to-stack-throws-exception/96)

[Bug on the Oracle bug tracker](http://bugs.java.com/view_bug.do?bug_id=8003398)


