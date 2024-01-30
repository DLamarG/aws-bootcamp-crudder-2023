# Week 2 — Distributed Tracing

### Setting Up X-Ray Service from AWS

## Importing and utilizing X-Ray In App.py File
```from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask.middleware import XRayMiddleware

xray_url = os.getenv("AWS_XRAY_URL")
xray_recorder.configure(service='Crudder', dynamic_naming=xray_url)
XRayMiddleware(app, xray_recorder)``` /

## Command To Create Group
```aws xray create-group \
--group-name "Cruddur" \
--filter-expression "service(\"backend-flask\")"``` /

## Added to Docker.yml file
```xray-daemon:
    image: "amazon/aws-xray-daemon"
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "us-east-2"
    command:
      - "xray -o -b xray-daemon:2000"
    ports:
      - 2000:2000/udp```
