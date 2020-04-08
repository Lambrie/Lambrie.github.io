---
layout: post
title:  "Getting started with Flask"
date:   2020-04-07 09:06:13 +0200
categories: Python-Flask
tags: Python Flask Web venv
---
So, you want to create a website? Look no further than Python and the Flask web framework.

Are you looking at creating a static website, that just services static information to your audience, or a full-fledged website with dynamic content? Never mind Flask can do it all. Okay maybe using Flask to serve static web pages might be a bit of overkill, but it`s always a good start into web development and it provides you with the opportunity to scale a static site into a dynamic information asset for yourself or organisation.

### What is Flask
Flask is a micro lightweight web framework developed for Python. Calling Flask lightweight doesn't in any fashion mean it's inferior to any other framework, in fact it provides developers with way more flexibility during development. 

Larger frameworks such as Django will come with a ton of components built-in in the framework such authentication, URL routing, database schema migration, functional admin interface, built-in template engine, ORM system, and bootstrapping tool. Flask being light weight doesn't come standard with these components, you can build custom component or install any of the flask extensions on PyPI to assist you in your project.

Flask is currently used by several high-traffic websites. Flask is usually used to accelerate development of simple websites that use static content. However, the developers still have option to extend and customize Flask according to precise project requirements.

### Tutorial Overview
In this tutorial we will discuss the following:
* What are web frameworks
* Creating Virtual Environments for your projects
* Installing Flask
* Creating your first Flask web application

### What is a web framework
A Web framework is a collection of packages or modules which allow developers to write [Web applications](https://wiki.python.org/moin/WebApplications "Web Applications") or services without having to handle such low-level details as protocols, sockets or process/thread management.

As a developer using a framework, you typically write code which conforms to convention that lets you "plug in" to the framework, delegating responsibility for the communications, infrastructure and low-level stuff to the framework while concentrating on the logic of the application in your own code. This "plugging in" aspect of Web development is often seen as being in opposition to the classical distinction between programs and libraries, and the notion of a "main loop" dispatching events to application code is very similar to that found in [GUI programming](https://wiki.python.org/moin/GuiProgramming "GUI programming").

Generally, frameworks provide support for several activities such as interpreting requests (getting form parameters, handling cookies and sessions), producing responses (presenting data as HTML or in other formats), storing data persistently, and so on. Since a non-trivial Web application will require several different kinds of abstractions, often stacked upon each other, those frameworks which attempt to provide a complete solution for applications are often known as full-stack frameworks in that they attempt to supply components for each layer in the stack.
[Python Wiki](https://wiki.python.org/moin/WebFrameworks "Python Wiki")

### Create a virtual environment
#### What is a virtual environment
A virtual environment is used to manage the dependencies in your project. So then what problem does a virtual environment exactly solve? 
The more Python projects you have, the more likely it is that you need to work with different versions of Python libraries, or even different Python versions itself. Newer versions of libraries for one project can break compatibility in another project, therefore it helps a great deal segregate each project within its own virtual environment.
Therefore, virtual environments are independent groups of Python libraries, one for each project preferably. Packages installed for one project will not affect other projects or the operating system’s packages.

#### How to create a virtual environment
For this tutorial I assume you already have Python installed, else follow this quick video guide [How to Install Python Video](https://www.youtube.com/watch?v=tUgCiB17OZw&t=195s "YouTube Video")

Create a new directory for your project, open command prompt and cd to your preferred project directory. I will call this project flask_hello_world
```shell
mkdir flask_hello_world
```

cd into your new project directory
```shell
cd flask_hello_world
C:\...\flask\flask_hello_world>
```

Issue the following command to create a virtual environment
```shell
python -m venv venv
```
This command will create a virtual environment using the venv package and will store the environment in the directory specified in the last parameter which will be venv also. Virtual Environment (venv) comes as standard module with the Python installation.

_If you encounter any issue ensure that python is set in your PATH and make sure of the naming convention used other examples include py and python3._

**It is highly recommended to create a virtual environment for every project as every project will have different set of dependencies and versions required**

In your directory you will now notice a new directory called venv
```shell
dir
2020/04/07  10:49    <DIR>    .
2020/04/07  10:49    <DIR>    ..
2020/04/07  10:49    <DIR>    venv
```

#### Activate the virtual environment
The virtual environment always needs to be activated first before it can be used.

```shell
venv\Scripts\activate
```

You will now notice the following in your command line when the environment is activated
```shell
(venv) C:\\Users\\...\\flask\\flask_hello_world>
```
The name of the virtual environment as defined above will appear in the command line.

In order to deactivate a virtual environment, just type deactivate in your terminal.
```shell
deactivate
```
Your terminal will change 
```shell
C:\\Users\\...\\flask\\flask_hello_world>
```
_Follow the Flask documentation for [instructions]("https://flask.palletsprojects.com/en/1.1.x/installation/#virtual-environments") other operating systems_

### Install Flask
We are going to use PIP to install the Flask package from [PyPI](https://pypi.org/) (Python Package Index) first. The official Flask documentation can be access through this link also [Flask Documentation](https://flask.palletsprojects.com/en/1.1.x/ "Flask documentation")

Ensure your virtual environment is activated. Then execute pip install with the flask package name from [PyPI](https://pypi.org/project/Flask/ "Download Flask Package") 
```shell
pip install flask
```

### Create your first application
In order to create our first project, we must now create the main python script to execute the application from.

You can do it through command line
```shell
echo #My first flask app > app.py
```
or open notepad and create a file with a .py extension through file explorer in your project directory.

_You don't need to call it app.py you can call it anything that you would find easy to use, just don't use flask.py, because this would conflict with Flask itself._

#### Basic Flask code
Open the app.py file with your preferred IDE, I will use PyCharm. Type the following code in your app.py file

{% highlight python linenos %}
# My first flask app
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'
{% endhighlight %}

Congratulations, you just finished writing your code to launch a working website. So how exactly do these 5 lines generate a website:

1. First line we import the Flask class from the flask package
1. We then create an instance of the Flask class called app. In order to create an instance of the class one mandatory argument is required. We then pass the name of the module or package of the application. Once the instance is created it will act as the central registry for the view functions, the URL rules, template configuration and much more. The name of the package is used to resolve resources from inside the package or the folder the module is contained in, therefore we pass the '__name__' parameter as the name of package
1. We then add the route decorator to specify the URL to trigger the following functions
1. The function is given a name which is also used to generate URLs for that function and returns the message we want to display in the user’s browser.

#### Running your first flask website
To run the application, you can either use the flask command or python’s -m switch with Flask. Before you can do that you need to tell your terminal the application to work with by setting the FLASK_APP environment variable
```shell
set FLASK_APP=app.py
set FLASK_ENV=development
flask run
 * Environment: development
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/
```
Alternatively, you can use python -m flask:
```shell
set FLASK_APP=app.py
set FLASK_ENV=development
python -m flask run
 * Environment: development
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/
```
This launches a very simple built-in server, which is good enough for testing but probably not what you want to use in production

So now you can open your browser and type in http://127.0.0.1:5000/ and there you go, your first website up and running on your local machine.

![image]({{site.baseurl}}/assets/res/_blog_data/flask_hello_world_page.png)

![image](./assets/res/_blog_data/flask_hello_world_page.png)

![image](/assets/res/_blog_data/flask_hello_world_page.png)

![image](/assets/res/_blog_data/flask_hello_world_page.png)