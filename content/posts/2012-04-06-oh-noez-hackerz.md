---
author: torben
date: 2012-04-06 14:22:59+00:00
draft: false
title: Oh noez, Hackerz!!!!
aliases: 
- /2012/04/oh-noez-hackerz/
categories:
- Java
- javascript
- Security
tags:
- culprits
- xing
---

Today I detected that one of my sites was hacked. Some punks got access to one of my webservers, added some files and altered some other files. Lucky me, on this webserver is php forbidden and they couldn't do any harm.

But from the start:

I host the website for one of the local sport clubs. The website is static, showing only some pics, contact and legal stuff. Once a year there I make a short report how the website is doing and what I've done. The deadline for the report is next saturday and so I decided to look on the webserver. And what did I found there? A recaptcha.php file:

[![Screenshot showing a recaptcha.php file in a filebrowser](/images/2012-04-06-oh-noez-hackerz/little_bugger-300x40.png)
](/images/2012-04-06-oh-noez-hackerz/little_bugger.png)

This is funny, 'cause php isn't allowed on the server. And hence it won't work... I downloaded the file and promptly my antivirus software alerted me, that there is a "PHP/Agent.FA" virus in the file. Now I'm curious.

In the file is a decrypted javaScript call to an russian webserver. Good for me, the service provider disabled all *.php files, returning only a message that php is disabled. So the JavaScript couldn't be executed even once. But if the hacker got access to the server and could already upload files... maybe he altered some too!

He did. In my index file is a new `<script>`-tag. It's 100 lines of obfuscated code, very complex and nested. After half'n'hour of starring at this horror, I decided to decode it.

I wrote a [small program](/images/2012-04-06-oh-noez-hackerz/JavaScript-deObfuscator.zip) in java which translates the \x64 characters into more readable chars. After that I only had to follow the functions... Unfortunatly all functions where named "I","j","l" and so on...

Finaly, eager to know what this function was intended to do, I set up a virtual machine (without network), and executed the script. The function added a new iframe to the site which loaded a new website. I googled the site and got warnings all over... This one should load a virus to my pc...

So in theory some people could have been infected with viruses by my site. Disgusting thoughts... Good for me, the website wasn't well programmed (not my fault ^^) and had a problem with the frame/noframes tags. This circumstance in combination with the generic nature of the attack saved saved the visitors of the site from viruses.


### Conclusion:


I was blind to the risks of hackers. The password seemed to be to short/easy or was hacked... I reported the hack to the chairman of my club, uploaded a clean version of the website and altered the Password. Hopefully this is enough.
