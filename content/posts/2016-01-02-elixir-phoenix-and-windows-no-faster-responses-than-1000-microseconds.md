---
author: torben
date: 2016-01-02 17:24:56+00:00
draft: false
title: 'Elixir, Phoenix and Windows: No faster responses than 1000 microseconds?'
aliases: 
- /2016/01/elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/
categories:
- Computer Stuff
- Elixir
- OS
tags:
- elixir
- microseconds
- powershell
- windows
---

If you read around phoenix developers you often hear stuff like "Awesome, requests served in under xxx microseconds!". Yet if I try the Phoenix framework, I only have this results:

    
    [info] GET /
    [debug] Processing by GettingStartedPheonix.PageController.index/2
    Parameters: %{}
    Pipelines: [:browser]
    [info] Sent 200 in 1000┬Ás
    [info] JOIN rooms:lobby to GettingStartedPheonix.RoomChannel
    Transport: Phoenix.Transports.WebSocket
    Parameters: %{}
    [info] Replied rooms:lobby :ok


With special emphasis on [info] Sent 200 in 1000┬Ás . Here we have two problems:



	  1. It looks like the command line doesn't know the **μ** sign and replaces it with **┬Á**.
	  2. Did it really take 1000 microseconds to serve the request? I'm not sure, but I NEVER have a request faster than that. Sometimes I have a slower request (163 milliseconds on startup for example) but never faster than 1000 microseconds.



# Locating the error source


Lets find the culprit: I use the Powershell to start the Phoenix app. Can Powershell show the **μ** sign?

[![Powershell using the μ sign](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/canPowershellShowMicrosecond.png)
](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/canPowershellShowMicrosecond.png "It can!")

[![Powershell showing off and uses the the μ sign as variable name](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/canPowershellShowMicrosecondsYes.png)
](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/canPowershellShowMicrosecondsYes.png "Variables with the μ sign are possible as well") 

As a matter of fact, you can use the **μ** sign as variable, if you want to.

So Powershell is good. What's the problem then? Looking into the mix.ps1 we can see that it executes the mix.bat, which executes the elixir.bat which either executes erl.exe or werl.exe. So lets have a quick look if the cmd (*.bat files are executed by cmd) is capable of the microsecond sign.

[![Also the command line is can show the sign](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/evenCmdCanDoIt.png)
](/images/2016-01-02-elixir-phoenix-and-windows-no-faster-responses-than-1000-microseconds/evenCmdCanDoIt.png "Also the command line can show the sign")

So the problem isn't the command line either. I printed the final executed call to erl.exe and executed this command without the cmd as middle man. The problem remained. So I assume it is a problem with either the erl.exe (if I use the --werl command line switch, werl.exe gets executed, the phoenix app gets started but no info is shown in werl.exe), Elixir, Phoenix or some of the plumbing in between.

Lets test Elixir. I created a new Elixir app mix new micro_second_test  and wrote a single function in it:

    
    def sayMicro do
      "μ"
    end


When starting the application with iex.bat -S mix  and executing the function we get this result:

    
    iex(1)> MicroSecondTest.sayMicro
    "╬╝"


So we can rule out Phoenix as Elixir already has that problem. What about erl.exe?

    
    PS C:\Users\Torben> erl.exe
    Eshell V7.2.1  (abort with ^G)
    1> io:fwrite("μ").
    µok


Erlang can show the **μ** sign. So the culprit is either Elixir or the plumbing. I will open an GitHub Issue for this problem. For the second problem (never showing less than 1000**μs**) I am not sure how to check. I think it could be in the cowboy web server, or in the Plug.Conn. But I have no clue...
