---
author: torben
date: 2011-08-07 17:52:30+00:00
draft: false
title: Problems... Weird ones...
aliases: 
- /2011/08/problems-weird-ones/
categories:
- Gadgets
- OS
---

I just bought a nice and sleek Asus 23" Display for my PC. Because of my old Hardware, I'am not able to attach it via HDMI to it. Good for me I've had my Lenovo IdeaPad Z370. I've attached it to the display and you know what? It works. Just out of the box...

... Until I decided, to change the alignment of the two displays (the Asus and the laptop one). It works fine if the big display is right of the small laptop monitor. If I choose to have the big one left of the small one, I've got a weird behaviour: The Asus display won't show the lower-left part of the screen!

See the two pictures attached for details:
![Bildschirmfoto1](/images/2011-08-07-problems-weird-ones/Bildschirmfoto1-1024x336.png "Bildschirmfoto1")
![Bildschirmfoto2](/images/2011-08-07-problems-weird-ones/Bildschirmfoto2-1024x336.png "Bildschirmfoto2")

Bildschirmfoto1.png was captured with the external Asus display on the right, Bildschirmfoto2.png with the external Asus Display on the left. The point of disapprovement is the left black part on bildschirmfoto2.png. The black parts on the screenshots are usable for the mouse (the cursor will get rendered) but no window will get rendered. It's the same behaviour like the black parts on the right side, where in reality is no display (because of the lower resolution on the laptop display)

My guess is, that some of the internals (x? gnome? some fancy abstraction layer?) think that there is no display. But... why?

Technical data:
Laptop: Lenovo IdeaPad Z370 on 1366 x 768
Display: Asus VS247H 23,6 Inch (connected via HDMI) on 1920 x 1080
OS: Ubuntu 11.04 (Kernel 2.6.38-10)
