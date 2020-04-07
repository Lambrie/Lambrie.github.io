---
layout: post
title:  "Getting started with Flask"
date:   2020-04-07 09:06:13 +0200
categories: Python
tags: Python Flask "Web Development" PyCharm
---
So you want to create a website? Look no futher than Python and the Flask web framework. \
Are you looking at creating a static website, that just services static information to your audience, or a full fledged website with dynamic content. Nevermind Flask can do it all. Okay maybe using Flask to serve static web pages might be a bit of overkill, but it`s always a good start into web development and it provides you with the opportunity to scale a static site into a dynamic information asset for yourself or organisation.

In order for this starter tutorial to make sense I would assume you have some basic knowledge of the following components, else following the links to gain a basic understanding of these topics before contiuning:
* [Python](https://www.google.com "What is Python")
* [PIP](https://www.google.com "What is PIP")
* [HTML](https://www.google.com "What is HTML") \
If you are comfortable with the above topics you can skip over and get started below

### What is a web framework
A Web framework is a collection of packages or modules which allow developers to write [Web applications](https://wiki.python.org/moin/WebApplications "Web Applications") or services without having to handle such low-level details as protocols, sockets or process/thread management.

As a developer using a framework, you typically write code which conforms to some kind of conventions that lets you "plug in" to the framework, delegating responsibility for the communications, infrastructure and low-level stuff to the framework while concentrating on the logic of the application in your own code. This "plugging in" aspect of Web development is often seen as being in opposition to the classical distinction between programs and libraries, and the notion of a "mainloop" dispatching events to application code is very similar to that found in [GUI programming](https://wiki.python.org/moin/GuiProgramming "GUI programming").

Generally, frameworks provide support for a number of activities such as interpreting requests (getting form parameters, handling cookies and sessions), producing responses (presenting data as HTML or in other formats), storing data persistently, and so on. Since a non-trivial Web application will require a number of different kinds of abstractions, often stacked upon each other, those frameworks which attempt to provide a complete solution for applications are often known as full-stack frameworks in that they attempt to supply components for each layer in the stack.
[Python Wiki](https://wiki.python.org/moin/WebFrameworks "Python Wiki")

### Create a virtual environment
#### What is a virtual environment
Use a virtual environment to manage the dependencies for your project, both in development and in production. What problem does a virtual environment solve? 
The more Python projects you have, the more likely it is that you need to work with different versions of Python libraries, or even Python itself. Newer versions of libraries for one project can break compatibility in another project.
Virtual environments are independent groups of Python libraries, one for each project. Packages installed for one project will not affect other projects or the operating system’s packages.

#### How to create a virtual environment
For this tutorial I assume you already have Python installed, else follow this quick video guide [Install Python Video](https://www.youtube.com/watch?v=tUgCiB17OZw&t=195s "Youtube Video")\

Create a new directory for your project, open command prompt and cd to your preferred project directory. I will call this project flask_hello_world
```shell
C:\Users\Lambrie\Documents\projects\python\flask>mkdir flask_hello_world
```

cd into your new project directory
```shell
C:\Users\Lambrie\Documents\projects\python\flask>cd flask_hello_world
C:\Users\Lambrie\Documents\projects\python\flask\flask_hello_world>
```

Issue the following command to create a virtual environment
```shell
C:\Users\Lambrie\Documents\projects\python\flask\flask_hello_world>python -m venv venv
```
This command will create a virtual environment using the venv package and will store the environment in the directory specified in the last parameter which will be venv also. Virtual Environment (venv) comes as standard module with the Python installation. \
__If you encounter any issue ensure that python is set in your PATH and make sure of the naming conevtion used other examples include py and python3.__ \
**It is highly recommended to create a virtual environement for every project as every project will have different set of dependencies and versions required**

In your directory you will now notice a new directory called venv
```shell
C:\Users\Lambrie\Documents\projects\python\flask\flask_hello_world>dir
2020/04/07  10:49    <DIR>          .
2020/04/07  10:49    <DIR>          ..
2020/04/07  10:49    <DIR>          venv
```

#### Activate the virtual environment
The virtual environment always needs to be activated first before it can be used.

```shell
C:\Users\Lambrie\Documents\projects\python\flask\flask_hello_world>venv\Scripts\activate
```

You will now notice the folloing in your command line when the environment is activated
```shell
(venv) C:\Users\Lambrie\Documents\projects\python\flask\flask_hello_world>
```
The name of the virtual environement as defined above will appear in the command line.

### Install Flask
We need to use PIP to install the Flask package from PYPI first. The official Flask documentation can be acces through this link also [Flask Documentation](https://flask.palletsprojects.com/en/1.1.x/ "Flask documentation")

#### On Windows 
```shell
pip install Flask
```

{% highlight shell %}
pip install Flask
{% endhighlight %}

```python
from flask import Flask
```

{% highlight python linenos %}
from flask import Flask
{% endhighlight %}

#### Setting up PyCharm Community for Flask


#### Setting up PyCharm Proffesional for Flask


### Create your first application


