---
layout: post
title: "Convert column number to Excel column letter(s)"
date: 2022-05-11 13:03:00 +0200
categories: Python
tags: Excel One-Liners
---
Powerful one liner to convert column number into Excel column letter(s)

{% highlight python linenos %}
def convert_to_excel_column(c):
	return "".join([chr(64+((c-1) // 26)) if c > 26 else '', chr(64+(c % 26)) \
                    if c % 26 != 0 else chr(90)])

for c in [1,26,27,52,53,78,79]:
    print(f'{c = } -> {convert_to_excel_column(c)}')
	
{% endhighlight %}

{% highlight cmd %}
c = 1  -> A
c = 26 -> Z
c = 27 -> AA
c = 52 -> AZ
c = 53 -> BA
c = 78 -> BZ
c = 79 -> CA
{% endhighlight %}

Or rather a true one-liner

{% highlight python linenos %}
col_letter = lambda x: "".join([chr(64+((x-1) // 26)) if x > 26 else '', chr(64+(x % 26)) \
                                if x % 26 != 0 else chr(90)])

for x in [1,26,27,52,53,78,79]:
    print(f'{x = } -> {col_letter(x)}')
	
{% endhighlight %}

{% highlight cmd %}
x = 1  -> A
x = 26 -> Z
x = 27 -> AA
x = 52 -> AZ
x = 53 -> BA
x = 78 -> BZ
x = 79 -> CA
{% endhighlight %}