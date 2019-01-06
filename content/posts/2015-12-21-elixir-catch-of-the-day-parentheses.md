---
author: torben
date: 2015-12-21 11:33:58+00:00
draft: false
title: 'Elixir catch of the day: parentheses'
aliases: 
- /2015/12/elixir-catch-of-the-day-parentheses/
categories:
- Elixir
---

In general parentheses can be omitted in Elixir function calls. so the call

    
    write_line(socket)


is identical to

    
    write_line socket


However, if you are using the pipe operator |> this is not the case. The parentheses are mandatory on usage of the pipeline operator.

Source: [Elixir Getting started](http://elixir-lang.org/getting-started/mix-otp/task-and-gen-tcp.html), in the block quote
