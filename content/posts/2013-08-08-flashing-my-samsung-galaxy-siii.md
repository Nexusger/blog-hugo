---
author: torben
date: 2013-08-08 18:35:22+00:00
draft: false
title: Flashing my Samsung Galaxy SIII
aliases: 
- /2013/08/flashing-my-samsung-galaxy-siii/
categories:
- Commandline
- Gadgets
---

Today I decided to flash my Samsung Galaxy SIII

I couldn't bear the badly written "improvements" which Samsung generously added to the Android anymore. My last smartphone, a now retired HTC Desire gained much out of the Cyanogen Custom Rom, therefore I gave it a try.


## Setup:


I have a Samsung Galaxy SIII (Intl) - i9300 from O2, a german mobilephone provider. My Laptop runs Windows 7 and for guidiance I use the [official cyanogen Wiki for my phone](http://wiki.cyanogenmod.org/w/Install_CM_for_i9300).

I'm feeling adventurous today, therefore I'll try the [nightly build](http://download.cyanogenmod.org/?device=i9300&type=)


## First step: Save your data!


Ok, you could do it the fancy way ([Titanium](https://play.google.com/store/apps/details?id=com.keramidas.TitaniumBackup&hl=de)), or just copy all files from the phone via explorer. Remember that some Apps doesn't save their data to file but in the database. (The only one that mattered to me was [Jiffy](https://play.google.com/store/apps/details?id=com.nordicusability.jiffy&hl=de))


## Second: Download stuff


As in the wiki advised, I downloaded the [Heimdall Suite 1.4](http://www.glassechidna.com.au/products/heimdall/) and the linked [ClockwordMod Recovery](http://download2.clockworkmod.com/recoveries/recovery-clockwork-touch-6.0.3.1-i9300.img) and because the cyanogen server aren't the fastest ones, I started to download my [CustomRom](cm-10.1-20130807-NIGHTLY-i9300.zip) as well. (Step 1 from the Wiki)


## Third: Do scary stuff


And here comes the fun: Shut down the phone (Step 2 from the Wiki) and boot in the download mode. I let the automatic device detection of windows do its work. Afterwards I startded the zadig.exe (Step 3.1). For the records, my phone was called "Gadget Serial"

[![Screenshot of zadig.exe](/images/2013-08-08-flashing-my-samsung-galaxy-siii/zadig-300x170.png)
](/images/2013-08-08-flashing-my-samsung-galaxy-siii/zadig.png)

I copied the downloaded Recovery File as mentioned in Step 4 and proceeded with Step 5.

My working commandline was

    
    heimdall.exe flash --RECOVERY recovery-clockwork-touch-6.0.3.1-i9300.img --no-reboot


It's worth to mention, that the wiki points out NOT to use an USB-Hub, but to connect your phone directly. I didn't read that part...

Other than that, no problems with the following steps.


## Fourth:


Luckily I had the adb tool already on my pc, so my adb push wasn't a pain in the ass.

I added the [Google Apps](http://goo.im/gapps) as well.

I followed the "Installing CyanogenMod from recovery" manual and had no problems. But I can't stress it enough, wipe data/factory reset is realy important!

It went well for me, and in a week or so I'll write a post about living with cyanogen 10.1
