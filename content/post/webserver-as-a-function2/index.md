+++
author = "Torben"
title = "Webserver as a function - Part 2"
date = "2021-10-11"
description = "Run Flask on GCP"
categories = ["Cookbook",]
tags = ["Python", "Flask", "Serverless", "FaaS", "GCP"]
+++

# TL;DR
Python functions on GCP need an entrypoint for every HTTP request.
You can define entrypoint that routes traffic to normal Flask methods.
This makes hosting a web service with Cloud Function a breeze.

# Motivation 
* Very fast setup of a web server locally and same behaviour on GCP
* One deployment artefact can contain static and dynamic parts

# Prerequisites
1. Installed python
2. Flask library is available

# Approach
## Default setup
[http://127.0.0.1:5000/static/](http://127.0.0.1:5000/static/).

This method is only used as the entrypoint for Google Cloud Functions
The code comes from this Stackoverflow question:
https://stackoverflow.com/questions/53488766/using-flask-routing-in-gcp-function
The answer was provides by Martin: https://stackoverflow.com/users/4443309/martin

{{< highlight python >}}
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


## Deploy on GCP

{{< highlight powershell >}}
gcloud functions deploy waaf --runtime python39 --trigger-http --allow-unauthenticated --entry-point gcp_entry_point
{{< /highlight >}}

# Further Reading
* Code can be found in [this GitHub project](https://github.com/torbenmoeller/pluvial-waaf).

