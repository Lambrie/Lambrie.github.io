---
layout: post
title:  "Generate PDF on AWS Lambda and store to AWS S3"
date:   2020-06-08 17:01:00 +0200
categories: Lambda
tags: AWS Lambda S3 Python Reportlab
---
Under construction - Use AWS Lambda to generate a PDF document and store the PDF document to S3 and return a signed URL to download the report generated.

### Tutorial Overview
In this tutorial we will discuss the following:
* What is AWS Lambda
* What is AWS S3
* Packages
* Upload Code to AWS Lambda
* Add python modules to AWS Lambda
* The function
* AWS Lambda temp storage
* How to generate a pdf
* Provide AWS Lambda permission to access S3
* Generate a pdf with test data in the console

### What is AWS Lambda
[AWS Lambda](https://aws.amazon.com/lambda/ "AWS Lambda") lets you run code without provisioning or managing servers. You pay only for the compute time you consume.

With Lambda, you can run code for virtually any type of application or backend service - all with zero administration. Just upload your code and Lambda takes care of everything required to run and scale your code with high availability. You can set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app.

The benefits of using AWS Lambda are:
#### No servers to manage
AWS Lambda automatically runs your code without requiring you to provision or manage servers. Just write the code and upload it to Lambda.
#### Continuous Scaling
AWS Lambda automatically scales your application by running code in response to each trigger. Your code runs in parallel and processes each trigger individually, scaling precisely with the size of the workload.
#### Subsecond Metering
With AWS Lambda, you are charged for every 100ms your code executes and the number of times your code is triggered. You pay only for the compute time you consume.
#### Consistent Performance
With AWS Lambda, you can optimize your code execution time by choosing the right memory size for your function. You can also enable Provisioned Concurrency to keep your functions initialized and hyper-ready to respond within double digit milliseconds.

AWS provides a free tier, which is more than enough to get started and play around without any [cost](https://aws.amazon.com/lambda/pricing/ "AWS Pricing") up front. They provide 1 million free requests per month and 400,000 GB-seconds of compute time per month.

### What is AWS S3
Amazon Simple Storage Service ([AWS S3](https://aws.amazon.com/s3/ "AWS S3")) is an object storage service that offers industry-leading scalability, data availability, security, and performance. This means customers of all sizes and industries can use it to store and protect any amount of data for a range of use cases, such as websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics. Amazon S3 provides easy-to-use management features so you can organize your data and configure finely-tuned access controls to meet your specific business, organizational, and compliance requirements. Amazon S3 is designed for 99.999999999% (11 9's) of durability, and stores data for millions of applications for companies all around the world.

The benefits of using AWS S3 are:
#### Industry-leading performance, scalability, availability, and durability
Scale your storage resources up and down to meet fluctuating demands, without upfront investments or resource procurement cycles. 
#### Wide range of cost-effective storage classes
Save costs without sacrificing performance by storing data across the S3 Storage Classes, which support different data access levels at corresponding rates.
#### Unmatched security, compliance, and audit capabilities
Store your data in Amazon S3 and secure it from unauthorized access with encryption features and access management tools
#### Easily manage data and access controls
S3 gives you robust capabilities to manage access, cost, replication, and data protection
#### Query-in-place services for analytics
Run big data analytics across your S3 objects (and other data sets in AWS) with our query-in-place services
#### Most supported cloud storage service
Store and protect your data in Amazon S3 by working with a partner from the AWS Partner Network (APN) — the largest community of technology and consulting cloud services providers

AWS also provides a [free tier](https://aws.amazon.com/s3/pricing "AWS Pricing") for S3. Upon sign-up, new AWS customers receive 5GB of Amazon S3 storage in the S3 Standard storage class; 20,000 GET Requests; 2,000 PUT, COPY, POST, or LIST Requests; and 15GB of Data Transfer Out each month for one year.

### Packages
The AWS Lambda environment comes standard with all the standard Python libraries and the AWS Python SDK [boto3](https://pypi.org/project/boto3/ "AWS Python SDK").

So we don't need to upload the boto3 package along with our code. Side note to this is, not including the Boto3 package will help with the cold start problem in AWS Lambda as there are less packages to load in the container when AWS Lambda starts up. But, also AWS keeps the boto3 package updated with the latest versions and could possibly lead to unexpected errors in the future. In order to mitigate this risk you can upload the boto3 package along with your code to ensure you use a single version of boto3 everytime.

In order for us to generate a PDF document in python we will need an external packages. For this I have used the [Reportlab](https://www.reportlab.com/opensource/ "ReportLab") package.

#### ReportLab
ReportLab is the time-proven, ultra-robust open-source engine for creating complex, data-driven PDF documents and custom vector graphics. It's free, open-source , and written in Python.  The package sees 50,000+ downloads per month, is part of standard Linux distributions,  is embedded in many products, and was selected to power the print/export feature for Wikipedia.

The ReportLab Toolkit has evolved over the years in direct response to the real-world reporting needs of large institutions. The library implements three main layers:
* A graphics canvas API that 'draws' PDF pages
* A charts and widgets library for creating reusable data graphics.
* A page layout engine - PLATYPUS ("Page Layout and TYPography Using Scripts") - which builds documents from elements such as headlines, paragraphs, fonts, tables and vector graphics.

Create a new project with virtual environment and use pip to install
```shell
pip install reportlab
```

### Upload Code to AWS Lambda

AWS Lambda provides a built in editor for interpreted languages (Python and Javascript) which is very usefull for small functions. The limitation comes in when the packages and code files are larger than 10MB. In this case the built in code editor will not be available and the code needs to be uploaded via Amazon S3. If your packages are less than 10MB your packages can be uploaded and displayed by uploading a zip file

I will assume you already have an AWS Account. Log into your account and from the dashboard Lambda will be available under the All Services section, or you can search for Lambda.

<img src="/assets/res/blogData/AWS_Dashboard.PNG" width="100%">

When selecting AWS Lambda, AWS will take you to the Lambda landing page listing all your created functions. 

<img src="/assets/res/blogData/AWS_Lambda_Functions.PNG" width="100%">

To create a new function select New Function at the top right. A new page will be provided to specify the name of the Lambda Function and also specifiy the runtime environment. In our case we use the latest available version for Python which is version 3.8. Version 3.6 and 3.7 are also available.\
We can also specify permission at the bottom, but this page will default to creating a new execution role.

<img src="/assets/res/blogData/AWS_Lambda_New_Function.PNG" width="100%">

A new function will then be created. The new page will load the online editor in section 2 Function Code.

<img src="/assets/res/blogData/AWS_Lambda_Built_in_editor.PNG" width="100%">

Let's test the editor quickly. Select Test at the top, and a new window will appear with option to create a new test, or edit an existing event. As we don`t have any tests yet, we can now only create a new one. Lets create a HelloWorldTest. Edit the json message below and create a 'name' key and add your name into the value.

<img src="/assets/res/blogData/AWS_Lambda_Tests.PNG" width="100%">

In the code editor add the following code

{% highlight python linenos %}
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': f'Hello {event.get("name")}!'
    }
{% endhighlight %}

Click save and then test.

```shell
Response:
{
  "statusCode": 200,
  "body": "Hello Lambo !"
}

Request ID:
"81f77177-9063-4229-ba37-bb00372edc8c"

Function Logs:
START RequestId: 81f77177-9063-4229-ba37-bb00372edc8c Version: $LATEST
END RequestId: 81f77177-9063-4229-ba37-bb00372edc8c
REPORT RequestId: 81f77177-9063-4229-ba37-bb00372edc8c	Duration: 1.08 ms	Billed Duration: 100 ms	Memory Size: 128 MB	Max Memory Used: 49 MB	Init Duration: 114.02 ms
```

### Add python modules
In our case we would like to add ReportLab. The easiest way is to create a new project on your local machine, create a virtual environment, add all the required modules and zip the site packages for upload.

The site packages will be located at /venv/Lib/site-packages

You can zip all the packages avalilable in the site-packages, but to keep the size of the zip file down, we can add the following folders into a seperate zip file:
* PIL
* Pillow-7.1.2.dist-info
* reportlab
* reportlab-3.5.42.dist-info

### The function
The lambda function is declared in the handler section. In our case we will keep it to the default as lambda_function.lambda_handler, which refers to the python file lambda_function.py and the lambda_handler function within.

The lambda function expects 2 parameters, event and context.
* event  	AWS Lambda uses this parameter to pass in event data to the handler. This parameter is usually of the Python dict type. It can also be list, str, int, float, or NoneType type
* context 	AWS Lambda uses this parameter to provide runtime information to your handler. For details, see [AWS Lambda context object in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-context.html AWS Lambda context object in Python)

In this tutorial we will not be using the context parameter.

### How to generate a pdf
Before we look at the code, go to S3 and create a new bucket with the name "lambdapdfs".

The below code will need to be stored in a seperate .py file, named lambda_function.py. 

{% highlight python linenos %}
import os, boto3
from datetime import datetime
from reportlab.pdfgen.canvas import Canvas
from reportlab.lib.pagesizes import A5

s3_client = boto3.client('s3')
def generatePDF(event,lambda_path):
    c = Canvas(lambda_path, pagesize=A5)
    c.setTitle("My first lambda pdf")
    c.setAuthor(event.get('name','anonymous'))
    c.drawString(A5[0]*0.01,A5[1]*0.9, f"{event.get('name','anonymous')}`s First Lambda PDF")
    return c.getpdfdata() # Return file as bytes

def lambda_handler(event, context):
    file_name = f"{event.get('name','anonymous')}.pdf" # Name of file and extention
    lambda_path = f"/tmp/{file_name}" # Temp storage location in Lambda
    s3_bucket_name = "lambdapdfs" # Name of bucket created in S3
    s3_path = os.path.join(str(datetime.now().strftime('%Y_%m_%d')),file_name) # Specify the location where file needs to be stored in S3
    file_created = generatePDF(event, lambda_path) # This method will create the pdf and return the byte information
    if file_created:
        with open(lambda_path, "wb") as file:
            file.write(file_created)
        try:
            s3_client.upload_file(lambda_path, s3_bucket_name, s3_path)
        except ClientError as e:
            return {"result": "failure", "errorMessage": e}
        else:
            os.remove(lambda_path) # Remove file from temp storage in Lambda container
            return {"result": "success", "errorMessage": None}
	else:
        return {"result": "failure", "errorMessage": "No file generated"}
{% endhighlight %}

*Quick code break down*
1. The lambda handler will receive the event data as a dictionary. We will then create a file name based on the name key received in the event dictionary.
1. The temp storage location of aws lambda will be at "/temp". So we will store the lambda location by combining the file name and temp location as "/tmp/Lambrie.pdf"
1. We will define the s3 bucket name as create at the beginning of this section. The s3 bucket name can also be added as an environment variable and referenced from there.
1. The S3 location where the file will be stored at in S3 will then be the date the file was generated and the file name as combined in line 1 of the function "/2020_06_08/Lambrie.pdf"
1. The generatePDF function will then execute the reportlab functions and generate a basic pdf document taking the name received in the event dictionary and adding it to the pdf. The byte data of the pdf will then be returned to the main function.
1. Once some data is returned from the generatePDFfunction, we will physically write the data to a file in the temp directory.
1. Once the physical file is created, the boto3 client will upload the file to AWS S3.
1. If no exception occured a final result can be sent back to the user

This file needs to be added to the zip file generated above containing the information regarding to the external packages that will be needed.

Then upload the file to AWS lambda, in this case the folder is less than 10MB so we can upload the zip file straight into Lambda and we it will not be required to upload it to S3 first.

<img src="/assets/res/blogData/AWS_Lambda_zip_upload.png" width="100%">

### Provide AWS Lambda permission to access S3
The final step will be to ensure AWS Lambda has sufficient access to S3 to be able to upload the pdf file to S3

Click on the Permissions tab in the AWS Lambda function console
<img src="/assets/res/blogData/AWS_Lambda_permissions.PNG" width="100%">

Select the current role created for this function. AWS will then take you to Identity and Access Management (IAM) console.
<img src="/assets/res/blogData/AWS_Lambda_permissions_1.PNG" width="100%">

Click on attach policy, search for S3, select AmazonS3Full Access policy and Attach Policy.
<img src="/assets/res/blogData/AWS_Lambda_permissions_2.PNG" width="100%">

### Generate a pdf with test data in the console
Once uploaded and policy adjustments made, we can reuse the same test we used before and click "Test".

You should see the following result in the console:
<img src="/assets/res/blogData/AWS_Lambda_final_test.PNG" width="100%">
