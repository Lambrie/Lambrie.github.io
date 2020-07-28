---
layout: post
title: "Elegent Python one-liners"
date: 2020-07-27 15:10:00 +0200
categories: Python
tags: One-Liners
---
Simple is better than complex and flat is better than nested, but sparse is better than nested and as always readability counts.

Python one-liners are commonly used by Python developers to perform a single function quickly. They can reduce code complexity by performing a function within a single line which might have taken up several more lines, like small loops. 
{% highlight python %}
print([number for number in range(100) if number % 2 == 0])
{% endhighlight %}
So Python one-liners can bring around some form om simplicty in your code layout, but then again it can be a barrier to understanding your code as some one-liners can truely be a nested mess and it can bring down the overall readability of your code if to long and nested.
{% highlight python %}
print('\n'.join("%i bytes = %i bits which has %i possible values." % (j, j*8, 256**j-1) for j in (1 << i for i in range(8))))
{% endhighlight %}

The following python concepts can be used to build powerful one liners:
* [List Comprehensions](https://www.youtube.com/watch?v=P39Fqjqv5qY)
* Lambda Functions / Anonymous Functions
* Walrus Operator (3.8+)
* Ternary Operator
* Built in functions
* Slice Notation

{% include in_post_advertisements.html %}

Some useful one-liners I have come accross:

Get all the even numbers from a list, using a list comprehension
{% highlight python %}
evenNumbers = [number for number in range(100) if number % 2 == 0]
{% endhighlight %}
```
[0, 2, 4, 6, 8]
```

Sum values from a list, with the built in sum function
{% highlight python %}
sum(evenNumbers)
{% endhighlight %}
```
20
```

Pretty printing the output of dictionary, with the built in pprint function
{% highlight python %}
from pprint import pprint
my_dict = {'name': 'Lambo', 'age': 0, 'gender': 'Male'}
print(my_dict)
pprint(my_dict)
{% endhighlight %}
```
{'name': 'Lambo', 'age': 0, 'gender': 'Male'}
{'age': 0, 'gender': 'Male', 'name': 'Lambo'}
```

Sorting a list containing sets, using a built in function for lists and a Lambda function
{% highlight python %}
a = [(11, 3), (2, 1), (7, 4), (10, -2)]
a.sort(key=lambda x: x[1])
{% endhighlight %}
```
[(10, -2), (2, 1), (11, 3), (7, 4)]
```

Populating a dictionary using a tenary operator
{% highlight python %}
{"id": "xxx_xxx_xxx", "type": _type if _type else "Unknown" }
{% endhighlight %}
```
{'id': 'xxx_xxx_xxx', 'type': 'Unknown'}
```

Reverse the order of list or string with slice notation
{% highlight python %}
words = "Hello world!"
words[::-1]
{% endhighlight %}
```
'!dlrow olleH'
```

Perform a list concatenation using a built in join
{% highlight python %}
wordList = ['Hello', 'World', '!']
" ".join(wordList)
{% endhighlight %}
```
'Hello World !'
```

Using the walrus operator to reduce dictionary lookups (This feature is only avaiable from Python 3.8)
{% highlight python %}
if title := request.get("title"): print(f'Found title: "{title}"')
{% endhighlight %}
```
Found title: "titleName"
```
