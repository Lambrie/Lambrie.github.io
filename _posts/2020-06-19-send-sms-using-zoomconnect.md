---
layout: post
title:  "Send SMS message using Python and ZoomConnect"
date:   2020-06-19 09:35:00 +0200
categories: SMS
tags: Python SMS ZoomConnect
---
How to use the ZoomConnect module in Python to send SMS messages

### Tutorial Overview
In this tutorial we will discuss the following:
* ZoomConnect Module
* Getting started
* Send a single SMS message
* Send bulk SMS messages

### ZoomConnect Module
ZoomConnect is a South African based company providing simple and reliable access to bulk SMS solutions for companies, organizations, and individuals.

ZoomConnect exposes a number of API's to interact with their platform. The ZoomConnect_sdk module is a Python wrapper for these API's, enabling a developer to use these APIs without struggling through the process of setting up the integration.

To start using ZoomConnect you must first create an account([ZoomConnect](https://www.zoomconnect.com/app/account/signup "ZoomConnect Signup")). Once an account is create login and genarte an API Token. Remember to keep this token safe and secure.

Download the [ZoomConnect_sdk](https://pypi.org/project/zoomconnect-sdk/0.0.1/) module using pip:
```
pip install zoomconnect-sdk==0.0.1
```

### Getting started
Once the package is installed, it is really simple to get going.

Firstly import the zoomconnect-sdk client.
{% highlight python %}
from zoomconnect_sdk.client import Client
{% endhighlight %}

Once the client is imported, we need to initialize the client with a api_token and account email:
{% highlight python %}
c = Client(api_token="secret",account_email="secret@keeper.co.za")
{% endhighlight %}

We can then execute any one of the functions provided by the client:
{% highlight python %}
accountBalance = c.get_account_balance{}
print(accountBalance)
{% endhighlight %}

The client has a number of functions available to execute specific commands on ZoomConnect. The following functions are available:

#### Account
* get_account_balance
* get_account_statistics
* account_transfer
* get_account_user_by_email
* create_account_user
* update_account_user
* get_account_user_by_userId
#### SMS
* get_sms
* send_sms
* get_sms_bulk
* send_sms_bulk
#### Contacts
* get_contacts_all
* get_contact
* create_contact
* delete_contact
* update_contact
* remove_contact_from_group
* add_contact_to_group
#### Groups
* get_groups_all
* get_group
* create_group
* update_group
* delete_group
* add_group_to_contact
* remove_group_from_contact
#### Messages
* get_all_messages
* get_message_analyses
* get_message_credit_cost
* get_message_encoding
* get_message_length
* check_message_length_within_max
* get_number_of_messages
* get_message
* delete_message
* mark_message_as_read
* mark_message_as_unread

### Send a single SMS message
To send a single SMS message the following simple code will perform that task for you
{% highlight python %}
from zoomconnect_sdk.client import Client
c = Client(api_token="secret",account_email="secret@keeper.co.za")
try:
    message = c.send_sms("0821231234", "Welcome to ZoomConnect SMS messaging service")
except Exception as e:
    print(e)
else:
    print(f"messages sent {message}")
{% endhighlight %}

### Send bulk SMS messages
ZoomConnect allows bulk messages to be sent also with a single api call.

{% highlight python %}
from zoomconnect_sdk.client import Client
c = Client(api_token='api_token', account_email='account_email')
try:
    recipients = ["0000000000","1111111111","2222222222"]
    messages = ["Hi Joe, Welcome to ZoomConnect SMS messaging service", 
				"Hi Jane, Welcome to ZoomConnect SMS messaging service", 
				"Hi John, Welcome to ZoomConnect SMS messaging service"]
    message = c.send_sms_bulk(recipients, messages)
except Exception as e:
    print(e)
else:
    print(f"messages sent {message}")
{% endhighlight %}

Remember to load some credits before sending messages.