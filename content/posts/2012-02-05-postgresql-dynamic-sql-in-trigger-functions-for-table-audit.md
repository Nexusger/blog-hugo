---
author: torben
date: 2012-02-05 20:34:00+00:00
draft: false
title: postgreSQL - Dynamic SQL in trigger-functions for table audit
aliases: 
- /2012/02/postgresql-dynamic-sql-in-trigger-functions-for-table-audit/
categories:
- PostgreSQL
tags:
- audit
- college
- old
- pl/pgsql
- postgre
- postgresql
- tables
- trigger
- trigger functions
- trigger procedures
---

# The problem

For a college project I tried to create a pl/pgsql trigger function, which should be invoked by a trigger and then saves the unaltered dataset to an audit table. This task alone is no rocket science and therefore boring. I would have to create a trigger, a trigger function and a log table per auditing table and call the function on ervery update or delete.

<!-- more -->

Instead I aimed for this solution:

[![Flowchart for postgreSQL audit function](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/Flowchart-1-300x254.png)
](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/Flowchart-1.png)

A single function, called by different triggers which inserts the unaltered datasets to the corresponding log tables.


# The first try


The idea is simple enough: Via the trigger function special variable 'TG_TABLE_NAME' I know the name of the source table and per definition the name of the target table (it's old_table_name || '_log'). Now would I just have to insert the unaltered dataset into the log/audit-table. And here occured my problem: To be flexible enough to insert in different tables I have to use dynamic sql. But you can't make a dynamic insert statement with the trigger special variable 'old' as source.

My statement would look like

    
    execute 'insert into log_table values (now(),old)';


now() - current date and time
old – Trigger variable – The old unaltered dataset – Type: RECORD

On execution of this function, the database complaines about not knowing the column 'old'. The database assumes (because there is no variable or function called 'old') that I meant a column. So this attempt failed.

For my second try I altered the statement a little bit:

    
    execute 'insert into log_table values (now(),old.*)';


I hoped the database now knows the 'old'-variable. But it doesn't. Correctly the database assumed that I want to insert all columns of the table 'old' which is in this context not existant.

My third (and desperate) try looked like this:

    
    execute 'insert into log_table values (now(),'||old.*||')';


Because the 'old' reference is not known in the namespace of the dynamic insert, I concatenated the execute with the 'old.*' variable. Which simply gave a syntax error...


# Brace yourselves, deadline is coming


After three evenings of trying, reading the postgreSQL documentation (the search function is in my oppinion a pain in the ass), cursing and failing the deadline for my project tiptoed closer.
So I was forced to take another approach on my problem. I recapitulated, which were the core features my solution should provide:



	  * Check if a table (except log-tables) has a corresponding log-table and if not, create it
	  * Log every update or delete in the corresponding log-table
	  * The solution should be automatic. The solution is of no value, if someone has to create everything by hand



# A Solution


I created three functions which 1) created the tables, 2) created the trigger functions and last 3) created the trigger itself. If a table, function or trigger already exists I raise a notice and go ahead.
[![Flowchart for postgreSQL audit function. The working one](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/Flowchart-2-267x300.png)
](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/Flowchart-2.png)

This three functions are now called everytime when our project starts. Because of our small database (eight tables) the functions are executed in no time. After the execution of the three functions, our database looks like:
[![Relations between tables, trigger and functions](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/Flowchart-2-after-300x160.png)
](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/Flowchart-2-after.png)

Here you can download the plpgsql code of the three functions: [log_tables](/images/2012-02-05-postgresql-dynamic-sql-in-trigger-functions-for-table-audit/log_tables.txt)

If this was helpfull, please let me know :-)


# Links:


[Trigger special variables](http://www.postgresql.org/docs/9.1/static/plpgsql-trigger.html) – PostgreSQL Documentation
[Executing dynamic commands](http://www.postgresql.org/docs/9.1/static/plpgsql-statements.html#PLPGSQL-STATEMENTS-EXECUTING-DYN) – Postgresql Documentation
