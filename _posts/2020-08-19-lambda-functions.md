---
layout: post
title: "Python Lambda Functions"
date: 2020-08-19 11:14:00 +0200
categories: Python
tags: "Lambda Functions"
---
Unlike lambda forms in other languages, where they add functionality, Python lambdas are only a shorthand notation if you’re too “lazy” to define a function - [Python.org](https://docs.python.org "Python.org")

The only advantage of using a lambda instead of a locally-defined function is that you don’t need to invent a name for the function. The downside is that it can only take 1 expression.

These functions are also known as:
* Anonymous functions
* Function literals
* Lambda expressions / abstractions / form


A lambda function can take multiple arguments, but can only have one expression
{% highlight python %}
lambda arguments : expression
{% endhighlight %}

Compared to a normal function, where it can take any number of arguments and any number of expressions. You will also have to define a name for the function
{% highlight python %}
def function_name(arguments) :
	expressions
	return result
{% endhighlight %}

A simple examples comparing a lambda function and a standard function
{% highlight python %}
# Standard Function
def standard_power(x,y):
	return x ** y

# Lambda Function
lambda_power = lambda x, y : x ** y

print(f"Standard Function: {standard_power(2,6)}")
print(f"Lambda Function: {lambda_power(2,6)}")
{% endhighlight %}

```
Standard Function: 64
Lambda Function: 64
```

A lambda function can also be assigned to a variable and can be reused, as in the example above. It can also be used on its own without assigning to a variable.

Lamda functions can be added into other function to perform specific tasks like when sorting a data dictionary according to values
{% highlight python %}
x = {1: 2, 3: 4, 4: 3, 2: 1, 0: 0}
x_sorted = {k: v for k, v in sorted(x.items(), key=lambda item: item[1])}
print(x_sorted)
{% endhighlight %}

```
{0: 0, 2: 1, 1: 2, 4: 3, 3: 4}
```

Or when you want to apply a special function over a pandas dataframe
{% highlight python %}
import pandas as pd 
  
def add(a, b, c): 
    return a + b + c + 3

def main(): 
    data = { 
            'A':[1, 2, 3],  
            'B':[4, 5, 6],  
            'C':[7, 8, 9] } 
    df = pd.DataFrame(data) 
    print("Original DataFrame:\n", df) 
    
	df['add'] = df.apply(lambda row : add(row['A'], 
                     row['B'], row['C']), axis = 1) 
    print('\nAfter Applying Function: ') 
    
    print(df) 
{% endhighlight %}

```
Original DataFrame:
    A  B  C
0  1  4  7
1  2  5  8
2  3  6  9

After Applying Function:
   A  B  C  add
0  1  4  7   15
1  2  5  8   18
2  3  6  9   121
```


