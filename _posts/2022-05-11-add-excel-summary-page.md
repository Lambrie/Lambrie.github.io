---
layout: post
title: "Create summary sheet for Excel with homogeneous data across multiple sheets"
date: 2022-05-11 14:24:00 +0200
categories: Python
tags: Excel Pandas xlsxwriter
---
Quick script to add a summary page to any Excel containing homogeneous data over multiple sheets, with some useful links and formatting for an enhanced end user experience

{% highlight cmd %}
pip install pandas
pip install xlsxwriter
{% endhighlight %}

{% highlight python %}
import pandas as pd
import xlsxwriter
################################## Load Excel File ##################################
# Create a dictionary for dataframes to be obtained from each sheet
df_list = {}
# Open excel file using built in excel reader provided by Pandas
with pd.ExcelFile('demo_data.xlsx') as file:
    # Loop over each sheet in excel
    for sheet in file.sheet_names:
        # Load each sheet as a dataframe
        df = pd.read_excel(file, sheet_name=str(sheet))
        # Add the sheet name as a column to the dataframe
        df["Year"] = int(sheet)
        # Add the dataframe into a dictionary to preserve the sheet name
        df_list[sheet] = df

# Concats all the dataframes in the list into a single dataframe
df_summary = pd.concat(df_list).reset_index()

# Summarize all data from all sheets according to the sheet name
df_summary = df_summary.groupby(['Year']).mean()
################################## New Excel File ##################################
with pd.ExcelWriter('demo_data_summary.xlsx') as writer:
    # Create the excel hyperlink formatting
    hyperlink_formatting = writer.book.add_format({'underline': True, 'font_color': 'blue'})
    # Write the summarize data into the summary sheet first
    df_summary.to_excel(writer,"summary",header=1,index=True, engine="xlsxwriter")
    # Loop over dictionary of data obtained from excel above
    for sheet_name, df in df_list.items():
        # Drop the sheet name used as the group by column
        df.drop(["Year"], axis=1 ,inplace=True)
        # Write each dataframe back to each own sheet as in the original file
        df.to_excel(writer,sheet_name,header=1,index=False, engine="xlsxwriter")
        # Reference the new worksheet
        worksheet = writer.sheets[sheet_name]
        # Obtain the dimensions of the dataframe
        row_length, column_length = df.shape
        # Convert the next available column to excel syntax 65 -> A
        excel_column = chr(65+(column_length))
        # Add hyperlink back to the summary page
        worksheet.write_formula(f'{excel_column}1'\
							, f'=IFERROR(HYPERLINK("#summary!A1","Back"),"")')
        # Apply hyperlink formatting to column
        worksheet.set_column(f'{excel_column}:{excel_column}',10,hyperlink_formatting)

    # Reference the summary worksheet
    worksheet = writer.sheets['summary']
    # Obtain the dimensions of the dataframe
    row_length, column_length = df_summary.shape
    # Add hyperlink to each row referencing the relevent sheet with detailed data
    for index in range(2,row_length + 2):
        # Convert the next available column to excel syntax 65 -> A
        excel_column = f'{chr(65+(column_length+1))}'
        # Add hyperlink from summary page to detailed sheet from the original Excel file
        worksheet.write_formula(f'{excel_column}{str(index)}' \ 
                                ,f'=IFERROR(HYPERLINK("#"&A{str(index)}&"!A1","Link"),"No Link")')
        # Apply hyperlink formatting to column
        worksheet.set_column(f'{excel_column}:{excel_column}',10,hyperlink_formatting)

    writer.save()
{% endhighlight %}


