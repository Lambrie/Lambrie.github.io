---
layout: post
title: "Pandas write dataframes to multiple sheets in Excel"
date: 2022-03-10 11:33:00 +0200
categories: Python
tags: Pandas Excel
---
Do you generate multiple dataframes and write each one out to its own excel or csv file? 

You can quickly generate a single excel with mutiple sheet with the below snippet

{% highlight cmd %}
pip install xlsxwriter
{% endhighlight %}
You can also use openpyxl as the excel engine, but for multiple sheets, I will recommend using xlsxwriter

I assume you already have pandas installed
{% highlight python %}
import pandas as pd

data = {'Name':['Tom', 'Brad', 'Kyle', 'Jerry'],
        'Age':[20, 21, 19, 18],
        'Height' : [6.1, 5.9, 6.0, 6.1]
        }

df = pd.DataFrame(data)
{% endhighlight %}

I will split the dataframe in 2 and save each part of the dataframe on a seperate sheet

{% highlight python %}
import xlsxwriter

with pd.ExcelWriter('file path') as writer:
	df[:2].to_excel(writer, "Sheet_name_1", header=1, index=false, engine="xlsxwriter")
	df[2:].to_excel(writer, "Sheet_name_2", header=1, index=false, engine="xlsxwriter")
	writer.save()
{% endhighlight %}
For more details on the configuartion available on the to_excel pandas function refer to [Pandas Documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_excel.html)