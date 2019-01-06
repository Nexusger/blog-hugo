---
author: torben
date: 2015-11-02 11:00:49+00:00
draft: false
title: Python, pandas and json_read with utf-8 encoding
aliases: 
- /2015/11/python-pandas-and-json_read-with-utf-8-encoding/
categories:
- Computer Stuff
- Python
tags:
- data science
- pandas
- python
---

The pandas library is a fantastic python toolkit to work with data. Recently I needed to read some json files in a pandas dataframe. Usually you can do that easily with the built in method:

    
    import pandas as pd
    pd.read_json('example.json')


But this method fails, if it encounters utf-8 encoded files. In contrast to the more often used methods `_read_table_` and `_read_csv_`, `_read_json_` does not provide an `_encoding_` parameter. To work around this you have to import the codecs module and use the `_open_` method:

    
    import codecs
    import pandas as pd
    pd.read_json(codecs.open('example.json', 'r', 'utf-8'))



