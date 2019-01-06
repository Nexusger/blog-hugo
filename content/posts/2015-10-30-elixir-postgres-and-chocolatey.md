---
author: torben
date: 2015-10-30 11:00:47+00:00
draft: false
title: Elixir, postgres and chocolatey
aliases: 
- /2015/10/elixir-postgres-and-chocolatey/
categories:
- Commandline
- Computer Stuff
- Elixir
- OS
- PostgreSQL
tags:
- ecto
- elixir
- phoenix
- postgresql
- powershell
- Solved
---

You try to use the phoenix getting started guide on windows and the task "mix ecto.create" fails with an useless error? Chances are your postgresql database isn't available with the ecto default credentials "postgres":"postgres". Try to logon to the database with the credentials "postgres":"Postgres1234" and change the password for the user to "postgres". Also, don't forget to change the password of the Windows user "postgres".

**Update 15.11.2015:** You also have to change the logon information for the service, otherwise postgresql won't start after an restart.


# Long version


If you happen to start with [elixir](http://elixir-lang.org/) and [phoenix ](http://www.phoenixframework.org/)you will probably install [postgres ](http://www.postgresql.org/)at some point. If you also happen to use Windows AND are a user of [chocolatey](https://chocolatey.org/) (which you should be!) it could happen that you run in a nasty, not very helpful error message when you try to use the [phoenix getting started guide](http://www.phoenixframework.org/docs/up-and-running) on the mix task:

    
    mix ecto.create


The error states exactly nothing:

    
    ** (ArgumentError) argument error
        (stdlib) :io.put_chars(:standard_error, :unicode, [[[], <<42, 42, 32, ...>>], 10])
        (mix) lib/mix/cli.ex:64: Mix.CLI.run_task/2
        (stdlib) erl_eval.erl:669: :erl_eval.do_apply/6
        (elixir) lib/code.ex:168: Code.eval_string/3


Which is not that helpful. Because of the error message you can't exactly google for that particular error. (I mean, yeah you could look the codepoint up but... really?)

Because the ecto tasks failed and ecto is the database mapper, I tried to connect to my recently installed postgres database. Which immediately made my mistake clear: ecto expects an user "postgres" with the password "postgres" for the database connection. But these credentials didn't work!

I tried to find the default password for postgres ("postgres" being the only answer I found) but failed after a quick googling. So I uninstalled the postgres package via chocolatey

    
    chocolatey uninstall postgresql


from my computer (which unfortunately didn't work... I deleted the folder afterwards :-/ ) and reinstalled it.

    
    chocolatey install postgresql


On the installation log I found the default password afterwards:

    
    Deleting and recreating postgres windows account...
     Cannot delete user. User postgres doesn't exist. Which is perfectly fine, it will be created in the next step.
     Removing postgres from Users failed. Please do that manually
     The account postgres has been created with the password set to Postgres1234. Please change the password for the postgres account and update the services to that password


I logged on to the database and changed the password:

    
    alter user postgres with password 'postgres';


Afterwards I changed the password for the windows user, "postgres" created on the installation accordingly.

Now the mix task works as expected.
