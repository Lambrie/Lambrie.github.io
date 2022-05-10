---
layout: post
title: "Enumerate and zip"
date: 2022-05-10 12:34:00 +0200
categories: Python
tags: loops
---
Enumerate and Zip are 2 powerful functions available in python when looping over lists.

I encountered this little problem when trying to run enumerate over a zip of multiple lists

{% highlight python %}
a = [1,2,3,4]
b = [4,3,2,1]
for i,a,b in enumerate(zip(a,b)):
	print(i,a,b)
{% endhighlight %}

{% highlight cmd %}
ValueError: not enough values to unpack (expected 3, got 2)
{% endhighlight %}

In order to overcome this, the values that needs to be unpacked from the zip function needs to be defined in parentheses
{% highlight python %}
a = [1,2,3,4]
b = [4,3,2,1]
for i,(a,b) in enumerate(zip(a,b)):
	print(i,a,b)
{% endhighlight %}

{% highlight cmd %}
0 1 4
1 2 3
2 3 2
3 4 1
{% endhighlight %}

The value can then also be unpacked as a tuple
{% highlight python %}
a = [1,2,3,4]
b = [4,3,2,1]
for i,t in enumerate(zip(a,b)):
	print(i,t)
{% endhighlight %}

{% highlight cmd %}
0 (1, 4)
1 (2, 3)
2 (3, 2)
3 (4, 1)
{% endhighlight %}
