---
layout: post
title:  "Create a RESTful API with Flask and Flask_RESTful"
date:   2020-04-07 09:06:13 +0200
categories: Python-Flask
tags: Python Flask Web venv
---
Create a flask RESTful API using the Flask_RESTful package with Flask.

In this tutorial we will build a basic RESTfull web service that will serve the current time and date, based on your location.

### Tutorial Overview
In this tutorial we will discuss the following:
* What is an API
* Basic API design
* Building a basic RESTful API endpoint

### What is an API

API, which stands for Application Programming Interface, is an interface that define how other software components or systems can interact with your application. It defines the kinds of calls or requests that can be made, how to make them, the data formats that should be used, the conventions to follow etc.

APIs are the defined interfaces through which interactions happen between an enterprise and applications that use its assets, which also is a Service Level Agreement (SLA) to specify the functional provider and expose the service path or URL for its API users. An API approach is an architectural approach that revolves around providing a program interface to a set of services to different applications serving different types of consumers.

When used in the context of web development, an API is typically defined as a set of specifications, such as Hypertext Transfer Protocol (HTTP) request messages, along with a definition of the structure of response messages, usually in an Extensible Markup Language (XML) or JavaScript Object Notation (JSON) format.

An example might be a shipping company API that can be added to an eCommerce-focused website to facilitate ordering shipping services and automatically include current shipping rates, without the site developer having to enter the shipper's rate table into a web database.

APIs have different design style:
*RESTful
*SOAP
*Webhooks
*GraphQL
*gRPC

For a better understanding about the differences, please read this [article](https://nordicapis.com/when-to-use-what-rest-graphql-webhooks-grpc/)

In plain english you can build an api to produce data that other systems can consume, or you can build an api that can consume data from other systems in your own application.

In this example we will produce data for other systems to use through a RESTful API. Our API will be waiting to be invoked by en external party, which will provide us with a capital city name, and we will send back the time for that country. In addition we will also have API`s available to obtain all the available capital cities and also a function to retrieve all capital cities by continent.

### Basic API Design

In order for us to accept a request for data from another system, we must be able to provide them with an adres where the information can be obtained from. A URL (uniform resource locator) will need to be provided to any consumer to be able to obtain the data that we will be providing.
The url will consist out of the following components:
* server_host       The physical server location, this can either be a DSN name or an IP address. In our case whilst still in development we will be hosting on our local machine adress 172.0.0.1 / localhost
* server_port       The port on which the api will be hosted. In our case during development we will be hosting on port 5000
* time_by_city	The time_by_country will be the endpoint, which will describe which function we need to perform
* {input_value}		The name of the capital city as input
* ?				    Additional query parameters can be added to provide extra capabilities for the function
* format=""			In this case the user can define the format in which they expect to receive the output ex. "dd-mm-yyyy HH:MM:SS"

_The HTTP method used will also determine the layout of the adres._

Our example will look like this during development:
```
http://127.0.0.1:5000/time_by_city/Johannesburg?format="dd-mm-yyyy HH-MM-SS"
```

and once deployed we would expect it to look somethong like this:
```
http://lambrie.github.io:5000/time_by_city/Johannesburg?format="dd-mm-yyyy HH-MM-SS"
```

For a RESTful API we will also need to assign a method to each adres. The following methods are available:
* GET		- Use GET requests to retrieve resource representation/information only – and not to modify it in any way.
* POST	- Use POST APIs to create new subordinate resources
* PUT		- Use PUT APIs primarily to update existing resource
* DELETE	- As the name applies, DELETE APIs are used to delete resources
* PATCH	- Use PATCH requests to make partial update on a resource


We will not be making any modifiactions to data therefore we will being using the GET method for our API.

We will also need to decide in which data format we will deliver the data back to the requestor. We can use any of the following data formats for data exchange:
* HTML 	-	Hyper Text Markup Language
* XML  	-	Extensable Markup Language
* JSON	- 	JavaScript Object Notation
* Text	-	Plain text
* PDF 	-	PDF documents
* JPEG	-	JPEG image documents

In this example we are going to return the data in JSON format. Why you may ask?
RESTful APIs depend on easy, reliable and fast data exchanges. JSON fits the bill for each of these attributes and python comes with built libraries to handle JSON as native data type.

The above component are important to understand as they will play a role during our development

For more information regarding RESTful api design principle following this (link)[https://restfulapi.net/]

### Building a basic RESTful API endpoint

Create a new project with an virtual environment. I will name mine flask_restful_timezone

##### Install Packages
For this project we will need to install the following packages:
* flask
* flask_restful
* flask pytz

```shell
pip install flask
pip install Flask-RESTful
pip install pytz
```

##### Create main code file - app.py
Create an app.py file within your project directory root.

We can start by importing all the required packages we just installed and also some built in packages into our app.py file
```python
from flask import Flask
from flask_restful import Api, Resource, reqparse
from pytz import timezone, all_timezones
from datetime import datetime
import os
```
##### Create config code file - config.py
Create a config.py file also within your project directory root.

We will also setup a config file to house all our configurations, which can be used accross mutiple environments.
We will setup the config file with 3 environements:
* Production
* Testing
* Development

The environment selection will then be driven of an environment variable indicating to our appliction ithon which environment it is running. Therefore we can set our environment variable so long.
Within your command prompt, ensure your virtual environment is activated, type the following to set the environment variable on your operating system:

Windows
```shell
set Environment_Config=config.Production
```
Linux / MaxOs
```shell
export Environment_Config=config.Production
```

Environment variables are very useful when it comes to managing configurations and protecting sensitive information, such as passwords etc.

Within your config.py file add
```python
class Config(object):
    '''
    Each environment will be a class that inherets from the main class config

    Configurations that will be the same across all environment will go into config,
    while configuration that are specfic to an environment will go into the relevant environment below
    ''''
    SECRET_KEY = "My$uper$ecretKey4Now"
    SERVER_TIME_ZONE = "Africa/Johannesburg"
    DEFAULT_DATETIME_FORMAT = "%Y-%m-%d %H:%M:%S"

class Production(Config):
    FLASK_ENV = 'production'
    PRODUCTION = True

class Testing(Config):
    FLASK_ENV = 'testing'
    TESTING = True

class Development(Config):
    FLASK_ENV = 'development'
    DEBUG = True
```

#### Initiliaze Flask and Flask-RESTful
In order to initialze Flask we need to provide the required configuration for Flask to start up. So we will need to import the config.py file with the Config class into our app.py file to initiliaze a flask instance with the configuartions specified for the environment within which we are going to work in.
```python
.
from config import Config

app = Flask(__name__)
app.config.from_object(os.getenv('Environment_Config'))

api = Api(app)
.
```

Flask has a built-in function to retrieve configurations from an object using the config.from_object function. In this case we will pass the location of the config class to the built-in function, by calling the os.getenv('Environment_Config') function.

In order for us to utilize the flask-restful extension package we need to initialize the flask-restful package with the flask object as the parent from which it will need to inheret from.

#### Resources

The main building block provided by Flask-RESTful are resources. Resources are built on top of Flask pluggable views, giving you easy access to multiple HTTP methods just by defining methods on your resource.
Therefore we need to create our own resources and add them to the api. This is very usefull as we can logically seperate different functions our application need to perform and make sure that code performing similar functions are logically grouped.

_We can split each resource out into its own file also within a resources directory, which can also make it easier to manage as your application scales_

We will be creating the following functions in our API aplication:
* Return a list of all capital cities
* Return a list of capital cities by continent
* Return the curent date and time for any city on the list

##### Return a list of all capital cities
Lets first create an API to show all available capital cities for which we have can return the time for currently

In order to do this we will create a class called All_Capital_Cities, which will inheret from Resources, which we imported from flask_restful above
```python
.
# Resource classes

class All_Capital_Cities(Resource):
    def get(self):
        cities = [city.split("/")[1].replace('_',' ').title() for city in all_timezones] # List Comprehension
        return {"cities":cities}

    def post(self):
        pass

    def put(self):
        pass

    def delete(self):
        pass
.
```

Above you will see that we define each http method within the class (resource, in this context), as discussed during the design section. Because we will only be returning data and not be making any changes we only need the get method.

The get method will get the list of all cities listed in the all_timezones list, which we import from the pytz package.
We will then loop through the list and stript out the continent from the name using the split function. We can easliy split out the continent because we know the format of the list and we know that it is consitent all the way through.
The following logic will then be applied on each iteration:
* Always return the second element at position *1*
* Check for any underscores and use the *replace* function to replace it with spaces, to make the name user friendly to read, ex. "new_york" will become "new york"
* Use the *title* function to format the casing, ex. "new york" will become "New York"

Watch this video to learn more about [List Comprehension](https://youtu.be/P39Fqjqv5qY)

Then we need to register the resource we just defined as a class to the api instance in the app.py file

```python
.
# Register Resources
api.add_resource(All_Capital_Cities, '/api/v1.0/capital_cities>', endpoint="get_all_capital_cities")
.
```

##### Return a list of capital cities by continent

If we want to return all capital cities per continent, we will need to register a new resource to obtain capital cities by continent.

In the app.py file just below the All_Capital_Cities class, under the "Resource Class" header, we can create a new class called All_Capital_Cities_by_Continent

```python
# Resource classes
.
class All_Capital_Cities_by_Continent(Resource):
    def get(self, continent):
        cities = [city.split("/")[1].replace('_',' ').title() for city in all_timezones if continent.title() in city] # List Comprehension
        return {"continent":cities}

    def post(self):
        pass

    def put(self):
        pass

    def delete(self):
        pass
.
```

Again, we will need to register the resource we just defined as a class to the api instance in the app.py file under the "Register Resources" header

```python
.
# Register Resources
api.add_resource(All_Capital_Cities, '/api/v1.0/capital_cities>', endpoint="get_all_capital_cities")
api.add_resource(All_Capital_Cities_by_Continent, '/api/v1.0/capital_cities_by_continent/<string:continent>', endpoint="get_all_capital_cities_by_continent")
.
```
