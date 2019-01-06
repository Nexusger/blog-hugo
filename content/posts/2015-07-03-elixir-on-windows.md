---
author: torben
date: 2015-07-03 21:23:05+00:00
draft: false
title: Elixir on windows
aliases: 
- /2015/07/elixir-on-windows/
categories:
- Elixir
tags:
- Atom
- developing
- problems
- REPL
---

Lately I got interested in the programming language [Elixir](http://elixir-lang.org/). I have not yet done anything besides the very first parts of the [tutorial](http://elixir-lang.org/getting-started/mix-otp/introduction-to-mix.html) and the fantastic [introduction](http://howistart.org/posts/elixir/1) from [José Valim](https://twitter.com/josevalim).  It looks fun and very capable, even if I have to think around the corner (Elixir is functional and I'm more used to the object oriented programming paradigma)

After reading some parts of the tutorial on my daily commute to work, I was  exited to start. I [installed ](http://elixir-lang.org/install.html)all the needed stuff on my laptop (so, erlang and elixir).

After that my questions have been "What IDE should I use?" and "What is a good setup?".  First of all, all the cool kids (this includes the elexir developers) seem to use MacBooks nowadays. This is fine with me, nice peace of hardware. Still, I have a windows Laptop. And I will use this machine until it dies of old age.

## **So what tools can I use to develop in elixir on windows*?**

### Shell:
You need a shell to compile or to test your code. On windows you have two options: The command line ("cmd") or powershell. Because of reasons I took the powershell. To set the needed variables (where is erlang, elixir and git installed) I used the tipps from [here](http://onor.io/2014/02/27/configuring-elixir-for-development/):

Open the file "c:\users\yourUserName\documents\WindowsPowershell\Windows.Powershell_profile.ps1" and add the following lines:

    
    #set variables 
    Set-Variable -Name elixirBase -option Readonly -value "C:\Program Files (x86)\Elixir" 
    Set-Variable -Name erlangBase -option Readonly -value "C:\Program Files\erl6.4" 
    Set-Variable -Name gitPath -option Readonly -value "c:/program files (x86)/git"
    
    #set path $env:Path ="$erlangBase/bin;$elixirBase/bin;$python3Base/bin;$gitPath/bin;"




That way, you can type "erl" and the erlang console opens up. The same way you can type in "iex" aaand nothing happens... at least not the elixir REPL shows up. This is because the command iex is used in the powershell for command invocation.

To work around this you can use the command "iex.bat" which brings up the REPL.


### REPL:


"iex.bat" brings up a REPL in powershell. And here my problems start. I realy wanted to love the powershell/elixir combo. First issue: "tab" does not bring up auto completion. This is annoying but no showstopper. But as soon as you progress in the [tutorial on the elixir site](http://elixir-lang.org/getting-started/basic-types.html) (on the second page...) you will find this line:

    
    iex> "hellö"


which resulted for me in

    
    ** (UnicodeConversionError) invalid encoding starting at <<148, 39, 10>> (elixir) lib/string.ex:1351: String.to_char_list/1


Looks, like there is a problem somewhere... probably in the elixir.bat file. A quick check confirms that powershell can work with special characters. And as we can see from the tutorial, elixir does as well.

I found several postings on the internet suggesting to set the codepage correctly (chcp 65001) but this does not work (because this is not a powershell command but a command line command) and should as far as I know not be needed (because powershell uses 16bit unicode internaly. No codepage needed. At least not for most of the western special characters).

Soo powershell is out. The tutorial recommends in this case the use of the command ...

    
    iex --werl


... which fails because of the aforementioned reasons (iex being a powershell command). But fear not:

    
    iex.bat --werl


brings up an erlang window with the REPL started. And we even have auto completion!


### Editor:


So comming to the most important (and disappointing) part: I do not ask for much. Auto completion and syntax highlighting. See no complicated task. Also, it would be nice to have some comfort in using the editor.

So which options do I have? Judging by the screenshots and forum posts, the usual editors are [Sublime Text,](http://www.sublimetext.com/) Emacs (especialy with [Alchemist](http://www.samueltonini.com/alchemist.el/)) and [Atom.io](https://atom.io/)

Sublime costs $70, which I won't pay if I don't see enormous benefits over other tools. After all I'm a student.

Emacs (and with that Alchemist) seems to be working only if you install cygwin or some other *nix environment emulator

Atom.io looks good to me and there can be a lot of packages integrated (packages/settings View/install packages). There is a package for [auto completion](https://atom.io/packages/autocomplete-elixir), one for[ syntax highlighting](https://atom.io/packages/language-elixir) even an [integration of IEx in Atom](https://atom.io/packages/iex) ... which is  OSX only.

So Atom.io it is! Proceeding in the tutorial I want to write my first test in the KeyValue example project. And here I felt in the next pit. Under some curcumstances (OS: Windows, Keyboard layout German) you can't [enter an @ sign.](https://github.com/atom/atom-keymap/issues/35) It's just not possible. All other keys seem to work. This actualy is a showstopper.


## Conclusion:


Elixir looks nice, but up to today I did not find the right tooling under windows. Sure, I can workaround any of these issues (notepad.exe can edit text to) the experience is everything but pleasant .



*If you are asking yourself "What is this man talking about? Any text editor is enough!" then let me tell you that I'm used to .Net development environments which can easily be several gigabytes big and are doing almost everything for you.
