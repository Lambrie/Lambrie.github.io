---
layout: post
title: "Python Slicing Intro"
date: 2020-07-30 08:57:00 +0200
categories: Python
tags: Slices Lists
---
The Python slice notation makes it very easy to access sub lists from an existing list.

Using the square bracket notation [ ] at the end of any list enables one to quickly create a subset. The square brackets expect a minimum of 2 parameters and an optional third parameter.
Can you guess what the 2 parameters will be? Yeah sure a start and stop parameter. Like anything you want to slice in life you need to know from where to where and then an increment / interval if not the default of 1
{% highlight python %}
list[start:stop:step]
# defaults
# start = 0
# stop = len(list) - 1
# step = 1
{% endhighlight %}

Just remember how list index works, index always starts at 0 which usually makes the last index the length or count of the list minus 1
```
                +---+---+---+---+---+---+
                | P | y | t | h | o | n |
                +---+---+---+---+---+---+
Slice position: 0   1   2   3   4   5   6
Index position:   0   1   2   3   4   5
```

{% include in_post_advertisements.html %}

#### Get a single item from a list
How to get a single item from a list. I will not consider it to be a slice as it does not follow the slice convention, but it is technically still a small slice.
{% highlight python %}
lst = [1,2,3,4,5,6,7,8,9,10]
{% endhighlight %}
We will be using the above list variable throughout

{% highlight python %}
lst[0] # first item
1
lst[-1] # last item - same as lst[len(lst)-1]
10
{% endhighlight %}

#### Basic Slices
Specific subset with a known start and end point
We would like the subset from the second items to the 5th item in this numeric list
{% highlight python %}
lst[1:5]
[2, 3, 4, 5]
{% endhighlight %}
The second item will have an index of 1 and the 5th item will be at index 4 which will be accessible by using index 5 as python will slice to the end index minus 1, therefore specifying index as 5 will only return up to index 4.

Subset from the beginning of the list to a specified end point
Let’s subset the first 6 item of the list
{% highlight python %}
lst[:6]
[1, 2, 3, 4, 5, 6]
{% endhighlight %}
If the first parameter is left blank, Python will automatically assume that it should start slicing from the beginning of the list.

Subset the end of the list from a specified start point
Let’s subset the last 6 items of the list
{% highlight python %}
lst[4:]
[5, 6, 7, 8, 9, 10]
{% endhighlight %}
An empty stop parameter will lead to the slicing occurring to the end of the list by default.

Subset the entire list / copy the entire list
{% highlight python %}
copy_lst = lst[:]
print(copy_lst)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
{% endhighlight %}

#### Negative Index Slices
Using negative slices provides more flexibility

Subset an entire list except the last 2 items
{% highlight python %}
lst[:-2]
[1, 2, 3, 4, 5, 6, 7, 8]
{% endhighlight %}

Subset only the last 2 items of the list
{% highlight python %}
lst[-2:]
[9, 10]
{% endhighlight %}

#### Steps or Intervals within a slice

Subset the entire list / copy the entire list in reverse
{% highlight python %}
lst[::-1]
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
{% endhighlight %}

Subset the first 2 items in reverse
{% highlight python %}
lst[1::-1]
[2, 1]
{% endhighlight %}

Subset the last 2 items in reverse
{% highlight python %}
lst[:-3:-1]
[10, 9]
{% endhighlight %}

Subset everything except the last two items in reverse order
{% highlight python %}
lst[-3::-1]
[8, 7, 6, 5, 4, 3, 2, 1]
{% endhighlight %}
