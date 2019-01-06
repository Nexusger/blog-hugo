---
author: torben
date: 2015-11-16 13:42:51+00:00
draft: false
title: 'Phoenix and Windows: js not executed'
aliases: 
- /2015/11/phoenix-and-windows-js-not-executed/
categories:
- Elixir
tags:
- Brunch.io
- stackoverflow
- windows
---

I'm currently on my way to learn a little bit of Elixir. I like the idea of functional programming language made easy. For starters I'm doing all the tutorials I can find. My most recent tutorial was the [Channels Tutorial](http://www.phoenixframework.org/docs/channels) of the Phoenix Framework.

As I did the tutorial, I encountered a problem: My _socket.js_ file was concatenated in the _app.js_ file via [Brunch.io](http://brunch.io/) but the promised message "Joined successfully" did not show up in the console.

After some fiddling with JavaScript I gave up and searched for the error. I found a similar error description on Stack "[js in html is not executing in Phoenix framework sample app](http://stackoverflow.com/questions/32674245/js-in-html-is-not-executing-in-phoenix-framework-sample-app)". As it turns out the tutorial/the getting started is not really covering the Windows side... To fix this problem edit your /brunch-config.js file.

At the bottom is a modules key. Change the value from

    
    modules: {
        autoRequire: {
          "js/app.js": ["web/static/js/app"]
        }
      }


to

    
    modules: {
        autoRequire: {
          "js/app.js": ["web/static/js/app"],
          "js\\app.js": ["web/static/js/app"]
        }
      }


Voilà, _brunch.io_ is now auto requiring the missing _app.js_ even under Windows
