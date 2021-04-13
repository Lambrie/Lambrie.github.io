---
layout: post
title: "Django view file from Google Drive in Browser"
date: 2021-04-13 11:17:00 +0200
categories: Python
tags: Django Google Drive django-googledrive-storage
---
Django can easily load files onto the server for storage, but what if local storage is a big limitation?

Best alternative is to upload files straight to a cloud provider. A number of cloud providers can be used and integrated into with minimal effort. In this specifc use case we used Google Drive as our storage backend.

We used the django-googledrive-storage package to seamlessly integrate our file fields in our model.py to Google Drive. The setup of this solution is really quick and effortless, but the problem came in when we tried to retrieve the file from Google Drive.

When calling the file via the absolute url provided by django-googledrive-storage, the export parameter is set to download only. So when selecting the file download link the entire files gets downloaded only. If you would like to provide an in browser view, we need to obtain a different url or change the absolutle url's parameter.

In my work around I tried both approaches:

1. Change parameter to view
Changing the absolute url parameter with a replace was the easyist. Click on the below links to see the difference

[https://drive.google.com/uc?id=1VPPSJ-wLJrimsVbjv_xqhaAvLsCfUZYC&export=download](https://drive.google.com/uc?id=1VPPSJ-wLJrimsVbjv_xqhaAvLsCfUZYC&export=download)

[https://drive.google.com/uc?id=1VPPSJ-wLJrimsVbjv_xqhaAvLsCfUZYC&export=view](https://drive.google.com/uc?id=1VPPSJ-wLJrimsVbjv_xqhaAvLsCfUZYC&export=view)

model.py
{% highlight python %}
from django.db import models
from gdstorage.storage import GoogleDriveStorage
gd_storage = GoogleDriveStorage()

class Attachment(models.Model):
    id = models.AutoField(primary_key=True, editable=False)
    attachment = models.FileField(upload_to='Dispute Notes', storage=gd_storage, null=True)
	
    def url_view(self):
        return self.attachment.url.replace("download", "view")
{% endhighlight %}

1. Obatin the Google Web View Link for Goolgle Drive
Using the below link provided by the Google Drive API enhances the user experieince in brower, by loading the image or file in the Google Drive frame.

[https://drive.google.com/file/d/1VPPSJ-wLJrimsVbjv_xqhaAvLsCfUZYC/view?usp=drivesdk](https://drive.google.com/file/d/1VPPSJ-wLJrimsVbjv_xqhaAvLsCfUZYC/view?usp=drivesdk)


model.py
{% highlight python %}
from django.db import models
from gdstorage.storage import GoogleDriveStorage
gd_storage = GoogleDriveStorage()

class Attachment(models.Model):
    id = models.AutoField(primary_key=True, editable=False)
    attachment = models.FileField(upload_to='Dispute Notes', storage=gd_storage, null=True)
	
    def url_view(self):
        return gd_storage._check_file_exists(self.attachment.name)["webViewLink"]
{% endhighlight %}

A pull request has been submitted to django-googledrive-storage at time of writing. Once change is implemented the above piece can be implemented using:

model.py
{% highlight python %}
from django.db import models
from gdstorage.storage import GoogleDriveStorage
gd_storage = GoogleDriveStorage()

class Attachment(models.Model):
    id = models.AutoField(primary_key=True, editable=False)
    attachment = models.FileField(upload_to='Dispute Notes', storage=gd_storage, null=True)
	
    def url_view(self):
        return gd_storage.url_web_view(self.attachment.name)
{% endhighlight %}