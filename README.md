# mms-drone

Dockerfile for an MMS / Ops Manager image

## Why
You want to quickly create host instances which will be used to run a MongoDB cluster managed by MongoDB MMS Cloud / Ops Manager.

## What
The dockerfile in this repo will create an Ubuntu image that runs the *automation agent*. Once the automation agent runs, you can use the Ops Manager UI to  add monitoring and backup agents, and you can use the host as (part of) a deployment.

## How

Collect your Ops Manager info for the automation agent. In your project, you should find:

* Api Key
* GroupId
* Base Url

The API key can be generated anew via the MMS portal for this host specifically or for the deployment as per your organization practice guidlines.

You can then build a container based on this image:
```bash
cd ./drone

docker build --rm -t nurih/mms-mongo-drone:alpha .
```

Push or publish the image as you see fit.

```bash
docker push nurih/mms-mongo-drone:alpha
```

You can run a container based on this image. You will need to supply the values for the URL, group id, and API key.

```bash
docker run --name drone1 -h drone1 -p 27017:30001 nurih/mms-mongo-drone:alpha \
    --mmsBaseUrl=http://my.opsmanager.example.com \
    --mmsGroupId=aaaaaabbbbccccddddeeeeee \ 
    --mmsApiKey=0000111122223333444455556666777788889999aaaabbbbccccdddd
```
Note: the URL should not have a training `/` at the end, but likely you would have a specific port number appended to the host. Typical test installations use port 8080 for http, or 8443 for https when installing a test OpsManager.

> This docker image is **not for production use** of any sort, and assumes you are trying things out using a test OpsManager as per [instructions here](https://docs.opsmanager.mongodb.com/current/tutorial/install-simple-test-deployment/).  
