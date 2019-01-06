---
author: torben
date: 2016-01-03 18:30:03+00:00
draft: false
title: 'Elixir, Phoenix and Windows: Some insights on my 1000┬Ás problem'
aliases: 
- /2016/01/elixir-phoenix-and-windows-some-insights-on-my-1000-problem/
categories:
- Commandline
- Computer Stuff
- Elixir
- OS
tags:
- elixir
- Erlang
- erlang vm
- GetSystemTime
- precision
- sys_time.c
---

On Saturday I wrote about "[Elixir, Phoenix and Windows: No faster responses than 1000 microseconds?](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/)". I described two problems I had with Phoenix in Windows: My response times seem to be stuck at 1000 microseconds and Powershell couldn't display the μ sign. I dug into some code and the mailing lists and found (with a lot of help) some answers.


# 1000 Microseconds on Windows?


The response times in Phoenix are often measured in Microseconds. Yet on Windows you won't see any requests faster than 1000 Microseconds. That's not because of a slower OS, but a not as precise as needed timer:

On Windows a developer has several options to get the system time. According to [windowstimestamp.com](http://www.windowstimestamp.com/description) there are (including the precision):



* The time service (1 ms)
* The performance counter (between 1 ms and 1 μs)
* The GetSystemTimeAsFileTime API (100 ns)

The highest precision (100 nano seconds) can be achieved with the GetSystemTimeAsFileTime API (or the GetSystemTime API, which is the same data but differently formatted).

Actually this is the API  the Erlang VM (which provides the run time for Elixir and Phoenix) is using. So in theory we should be able to get more precise data out of these API. Yet the Erlang VM only returns millisecond as smallest unit. I'm pretty there are reasons for it, but I don't understand them. If you are curious (and don't fear a little bit C code) go ahead and look at the implementation of the timing in Erlang: [sys_time.c in Erlang VM](https://github.com/erlang/otp/blob/maint/erts/emulator/sys/win32/sys_time.c)


# 1000┬Ás instead of 1000μs?


The second problem I had on saturday was the missing μ sign in my Powershell environment. I got hinted that I have to set the code page of Powershell to UTF8:

    
    chcp 65001


This fixed the problem for me. Unfortunately this introduced another, more grave bug to me: On putting out special characters in iex.bat, iex now fails to react completely. Until this bug is fixed, I strongly advise against this fix.
