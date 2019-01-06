---
author: torben
date: 2011-10-03 13:03:18+00:00
draft: false
title: 'New task: Get my sd-card reader working...'
aliases: 
- /2011/10/new-task-get-my-sd-card-reader-working/
categories:
- Gadgets
- OS
tags:
- Lenovo IdeaPad Z370
- sd-card
- Ubuntu
---

New task of the day: Get my sd-card reader in my laptop working. My Lenovo IdeaPad is nice and everything but sometimes there are some annoying things. Like a not working sd-card reader. There absolut no reaction on Ubuntu if I insert a sd-card... Maybe, I will find out :-)

**Update:**
`rmmod ehci_hcd` doesn't work
`echo "usb-storage" >> /etc/modules; modprobe usb-storage` was promising, but doesn't work either

What do I know? My laptop has a "Realtek Semiconductor Corp." sd-card reader.
If I insert an sd-card, `dmesg` will show

    
    usb 1-1.3: USB disconnected, address 8
    usb 1-1.3: new high speed USB device using ehci_hcd and address 9 


**Update:**

Wohoo, 11.10 fixes this problem! I'm able to use sd-cards! Not that I realy use them, but it's nice to know, I could
