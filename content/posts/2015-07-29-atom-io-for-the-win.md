---
author: torben
date: 2015-07-29 16:40:17+00:00
draft: false
title: Atom.io for the win
aliases: 
- /2015/07/atom-io-for-the-win/
categories:
- Elixir
---

Whohoo, I gave Atom.Io another try and found a <del>solution</del>  workaround to my [@ sign problem](/posts/2015-07-03-elixir-on-windows):

To fix my problem, I had to unset the keybinding for "[autoflow](https://atom.io/packages/autoflow)":



1. open ~\.atom\keymap.cson
2. add following lines to the file:

	`.platform-win32 atom-text-editor, .platform-linux atom-text-editor':`

	`'ctrl-alt-q': 'unset!`



3. restart Atom.io

Sources: [corelgott.de](http://www.corelgott.de/?p=144)
