+++
author = "Torben"
title = "Host static files with Flask"
date = "2021-09-23"
description = "Webserver as a function"
categories = ["Cookbook",]
tags = ["Python", "Flask", "Serverless", "FaaS",]
+++

# TL;DR
Flask already serves static files by default.
This makes hosting with a Cloud Function a breeze.

# Motivation 
* Very fast setup of a web server locally and on every cloud provider
* One deployment artefact contains static, dynamic and backend
* Other approaches like static hosting with a storage bucket are not possible

# Prerequisites
1. Installed python
2. Flask library is available

# Approach
## Default setup
Flask already serves static files by default. 
This Hello World example already serves static files.

{{< highlight python >}}
from flask import Flask
from flask import request

app = Flask(__name__)

if __name__ == '__main__':
  app.run()
{{< /highlight >}}

Important are the parameters *static_folder* and *static_folder*.

*static_folder* is the path to the folder containing the static files. 
The path can be relative to the application *root_path* or absolute.
The constructor sets *static_folder* by default to 'static'.

*static_url_path* is the path on which the static content is hosted.
The value defaults to the value of *static_folder*.

{{< details title="These default values are set implicitly" open=true >}}
{{< highlight python >}}
app = Flask(__name__, static_folder='static', static_url_path=None)
{{< / highlight >}}
{{< /details >}}

## Host a simple website
To illustrate we create a folder called 'static' and add an example html file.
The directory structure should look like this:
![](file-hierarchy.png)

{{< details title="static/index.html file" open=false >}}
{{< highlight html >}}
<html>
  <body>
    <h1>Hello from the static folder!</h1>
  </body>
</html>
{{< /highlight >}}
{{< /details >}}

Running the application and opening [http://127.0.0.1:5000/static/index.html](http://127.0.0.1:5000/static/index.html) we can see the example website.
![](chrome1.png)

## Change the root path

Personally, I prefer to host my website directly on the root path. To achieve this, *static_url_path* is set to ''.

{{< details title="Overwrite the default parameter" open=true >}}
{{< highlight python3 >}}
app = Flask(__name__, static_url_path='')
{{< /highlight >}}
{{< /details >}}

Restart the application and we can see the improved paths at [http://127.0.0.1:5000/index.html](http://127.0.0.1:5000/index.html).
![](chrome2.png)


# Further Reading
* Check the regarding website to deploy the application as a function
  [GCP](https://cloud.google.com/functions/docs/deploying/console),
  [AWS](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html) and
  [Azure](https://docs.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-python#publish-the-project-to-azure)
* There are many alternatives to this approach. In my experience, hosting a website as a function is rarely the best 
  approach. However, I had a customer with rigorous compliance and security requirements, which obstructed other 
  approaches. 
* Often it is much easier to host the website in a storage bucket. 
  [GCP](https://cloud.google.com/storage/docs/hosting-static-website), 
  [AWS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html) and
  [Azure](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website) provide guidelines for this.
* Containerized solutions with NGINX or Apache HTTP Server are much more common.
* One advantage of this approach is the creation of one deployment artefact that can include frontend and backend. 
* The costs can be quite high for a website with high traffic. Please check your cloud billing estimations beforehand.
* To reduce costs you can use CDNs and other caching mechanisms.  
