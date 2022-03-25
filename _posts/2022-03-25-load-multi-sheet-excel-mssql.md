---
layout: post
title: "Load multisheet Excel file into a single database table"
date: 2022-03-25 15:08:23 +0200
categories: Python
tags: Excel Pandas sqlalchemy SQL
---
Loading a excel with multiple sheets into a single database table with Python quick and easy.

I found this very usefull when I had an excel file where the data was split by year per sheet. The table on each sheet was the same so I was able to merge all the sheets together into a single database table with this quick Python script

{% highlight cmd %}
pip install openpyxl
pip install pandas
pip install sqlalchemy
{% endhighlight %}

{% highlight python %}
import pandas as pd
import openpyxl
from sqlalchemy import create_engine
from urllib.parse import quote_plus

## Load Excel File ##

# Create a list for dataframes to be obtained from each sheet
df_list = []
# Open excel file using built in excel reader provided by Pandas
with pd.ExcelFile('demo_data.xlsx') as file:
    # Loop over each sheet in excel
    for sheet in file.sheet_names:
        # Load each sheet as a dataframe
        df = pd.read_excel(file, sheet_name=str(sheet))
        # Add the sheet name as a column to the dataframe
        df["sheet_name"] = sheet 
        # Add the dataframe into a list
        df_list.append(df)

# Concat all the dataframes in the list into a single dataframe
df = pd.concat(df_list).reset_index()

## Insert Into DB ##

# Create connection to database
connection_string = """Driver={ODBC Driver 17 for SQL Server};
                      Server=localhost;
                      Database=MyDB;
                      UID=db_user;
                      PWD=db_user_password"""
engine = create_engine(f'mssql+pyodbc:///?odbc_connect={quote_plus(connection_string)}')

# Open connection to database
with engine.connect() as conn, conn.begin():
    table_name = "new_table"
    # remove index column
    df.reset_index(drop=True, inplace=True)
    # write dataframe to database. 
    # Replace = Drop table and create new table on each run
    df.to_sql(table_name, conn, index=False, if_exists='replace', schema="dbo")
    print(f"{table_name} table created in database")
{% endhighlight %}

