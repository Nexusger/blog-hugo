---
author: torben
date: 2015-12-03 22:11:04+00:00
draft: false
title: 'Elixir, Windows and Chocolatey: Updates'
aliases: 
- /2015/12/elixir-windows-and-chocolatey-updates/
categories:
- Commandline
- Elixir
tags:
- choco
- powershell
- Workaround
---

Recently I ran in a problem with Elixir on Windows, installed via [Chocolatey](https://chocolatey.org/packages/Elixir):

After an update of the Elixir package:

    
    choco upgrade elixir


(or a Windows update or something similar) I couldn't use iex.bat anymore. Every time I tried to open iex.bat I got an crash_dump:

    
    {error_logger,{{2015,12,3},{22,35,15}},crash_report,[[{initial_call,{supervisor_bridge,user_sup,['Argument__1']}},{pid,<0.22.0>},{registered_name,[]},{error_info,...


(That said, mix still worked...)

After several attempts on reinstalling Elixir (and Erlang, just to be sure) I found this comment on the Elixir package on Chocolatey.org from the package maintainer [onor_io](https://disqus.com/by/onoriocatenacci/) from July the 6th of 2015:


>"That said, I have been meaning to modify the package to copy the *.bat files and the binaries to the Chocolatey packages directory"


If I have a look at the bin folder of Chocolatey (in my case c:\programData\Chocolatey\bin) I can see the elixir *.bat files. But they are available at the Elixir\bin folder as well (c:\programData\Chocolatey\lib\Elixir\bin). For a test I removed the Elixir related files from the Chocolatey\bin folder (elixir.bat, elixirc.bat, iex.bat, mix.bat) and added the Elixir\bin folder to the path.

After a restart of the Powershell, iex.bat produces the desired result:

    
    Eshell V7.1 (abort with ^G)
    Interactive Elixir (1.1.1) - press Ctrl+C to exit (type h() ENTER for help)
    iex(1)>




### Drawback


This method isn't flawless: The uninstall (and probably upgrade) via Chocolatey does not work anymore... You can workaround this problem if you copy all *.bat files back to the Chocolatey\bin folder:

    
    copy-Item -path $Env:ChocolateyInstall\lib\Elixir\bin\*.bat -Destination $Env:ChocolateyInstall\bin



