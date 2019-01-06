---
author: torben
date: 2015-12-19 12:00:31+00:00
draft: false
title: Atom.io now works on German keyboards!
aliases: 
- /2015/12/atom-io-now-works-on-german-keyboards/
categories:
- Computer Stuff
- Elixir
- Software
- Tools
---

## Summary:


If you have problems with non us-keyboards and the editor Atom.io install this package: [keyboard-localization](https://atom.io/packages/keyboard-localization)


## Shortly longer:


Ok, I blogged about my problems with Atom.io [before](/posts/2015-07-03-elixir-on-windows/) ([and before](/posts/2015-07-29-atom-io-for-the-win/)) and the underlying issues aren't fixed in the chromium parts of Atom.io. Yet there were workarounds: For example I created my own keybinding file, which fixed my most important issue the missing "@" sign (who is using that, anyway?). Now I needed an working "\" in my [elixir code](http://elixir-lang.org/getting-started/mix-otp/genserver.html#our-first-genserver). I tried to fix it in the same way as I did before, but failed. Fortunately for me [Andreas Scherer](https://github.com/andischerer) and [David Badura](https://github.com/DavidBadura) created an Atom.io package which fixed all my keyboard issues with Atom.io.

So if you have a keyboard issue while working in Atom.io, which could be a localization issue give this package a try: [keyboard-localization](https://atom.io/packages/keyboard-localization)
