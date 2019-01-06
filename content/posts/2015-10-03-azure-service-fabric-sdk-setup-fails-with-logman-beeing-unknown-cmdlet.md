---
author: torben
date: 2015-10-03 08:20:57+00:00
draft: false
title: Azure Service Fabric SDK setup fails with logman beeing unknown cmdlet
aliases: 
- /2015/10/azure-service-fabric-sdk-setup-fails-with-logman-beeing-unknown-cmdlet/
categories:
- Computer Stuff
---

Recently I looked into microservies. I've read the excelent book "[Building Microservices](http://www.amazon.de/gp/product/1491950358/ref=as_li_tl?ie=UTF8&camp=1638&creative=19454&creativeASIN=1491950358&linkCode=as2&tag=krams0c-21)![](http://ir-de.amazon-adsystem.com/e/ir?t=krams0c-21&l=as2&o=3&a=1491950358)
" (affiliate link) and tried to combine Microservices with Microsoft. Luckily for me, I was born in exciting times and Microsoft just released an[ SDK for their new Microservice Framework Service Fabric](https://azure.microsoft.com/en-us/campaigns/service-fabric/).

Unfortunatly I had problem with the installation. Download and installation went well, but on the step [Install and start a new cluster](https://azure.microsoft.com/en-us/documentation/articles/service-fabric-get-started/#install-and-start-a-local-cluster) I got the error that the cmdlet logman is not known. After some googling I found out that logman is in fact on my laptop. It's just unknown to my powershell...

    
    set-alias logman C:\Windows\System32\logman.exe


Will fix this nasty error.




