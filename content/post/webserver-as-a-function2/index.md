+++
author = "Torben"
title = "Webserver as a function - Part 2"
date = "2021-10-14"
description = "Run Flask on GCP"
categories = ["Cookbook",]
tags = ["Python", "Flask", "Serverless", "FaaS", "GCP"]
+++

# TL;DR
Python functions on GCP need an entrypoint for every HTTP request.
You can define one entrypoint that routes traffic to normal Flask methods.
This makes hosting web services with Cloud Function a breeze.

# Motivation
* Very fast setup of a local web server
* Same behaviour locally as on GCP
* One deployment artefact can contain static and dynamic parts

# Prerequisites
1. Installed python
2. Flask library is available

# Approach
## The Flask App
The Flask app has a simple api endpoint that return "Hello from the api!".
It also hosts a static file called index.html from the static folder.
The local web server is reachable at [http://127.0.0.1:5000/index.html](http://127.0.0.1:5000/index.html).
For more information see the [previous post](https://pluvial.dev/post/webserver-as-a-function/).

{{< highlight python >}}
from flask import Flask, Request

# This serves static content
app = Flask(__name__, static_folder='static', static_url_path='/')

# Backend code can be written normally
@app.route('/api')
def api():
    return "Hello from the api!"
{{< /highlight >}}

## Preparing for Google Cloud Function
When a Cloud Function is triggered it loads the runtime environment and calls a method of the provided code.
The called method is named entry point and is specified in the deployment command.
The name of the method can be chosen freely.
In the code example the method is named gcp_entry_point, containing the boilerplate code
to integrate with the Flask methods.

Google Cloud Function passes a Request object to the entry point method. The Request class is imported from Flask.
The Request object implements access to headers, mime-type and of course data that might have been passed.
The following code takes the Request from the entry point and routes it through the processing methods of Flask.

Roughly, the code
* sets the received Request object as RequestContext
* preprocesses the request
* dispatches the request, meaning it calls the Flask methods that are provided by the user code
* handles the exception, if necessary
* builds the response object and returns it

{{< highlight python >}}
from flask import Request
def gcp_entry_point(request: Request):
    with app.request_context(request.environ):
        try:
            rv = app.preprocess_request()
            if rv is None:
                rv = app.dispatch_request()
        except Exception as e:
            rv = app.handle_user_exception(e)
        response = app.make_response(rv)
        return app.process_response(response)
{{< /highlight >}}

The code comes from this
[Stackoverflow answer](https://stackoverflow.com/a/66026762/1499913)
that was provided by [Martin](https://stackoverflow.com/users/4443309/martin).

## Deploy on GCP
The code is now ready to run as Cloud Function. You can use this gcloud command start the deployment.
Important is the parameter --entry-point gcp_entry_point, which marks the method from the previous code example 
as the entry point.
After the deployment the function is reachable by the trigger URL.
It looks like
[https://[REGION]-[PROJECT-NAME].cloudfunctions.net/waaf](https://[REGION]-[PROJECT-NAME].cloudfunctions.net/waaf).

{{< highlight powershell >}}
# Deploy the function
gcloud functions deploy waaf --runtime python39 --trigger-http --allow-unauthenticated --entry-point gcp_entry_point
# Clean up (Needs additional approval)
gcloud functions delete waaf
{{< /highlight >}}

{{< hint >}}
⚠ ⚠ ⚠ The cloud function is reachable from the internet by default!
This can lead to security issues and high costs!
Check the documentation on [Securing Cloud Functions](https://cloud.google.com/functions/docs/securing)!
{{< /hint >}}

# Further Reading
* Code can be found in [this GitHub project](https://github.com/torbenmoeller/pluvial-waaf).
* The gcp_entry_point is only used by the Google Cloud Function. Locally, the requests are directly routed to the Flask methods.
* THINK ABOUT SECURITY! Check the documentation on [Securing Cloud Functions](https://cloud.google.com/functions/docs/securing).
* For more information see the [previous post](https://pluvial.dev/post/webserver-as-a-function/).

