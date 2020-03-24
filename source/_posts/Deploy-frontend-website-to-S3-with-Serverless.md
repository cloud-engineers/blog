---
title: Deploy frontend website to S3 with Serverless
date: 2020-03-24 16:51:18
tags:
---
Surprisingly, it can be difficult to easily deploy a website to S3 without using the amazon cli, sdk, or amplify.

Thankfully there is a plugin for serverless that now allows this to be easily accomplished.

##### Quick Start
Install serverless globally if you have not done so already:
```
sudo npm install serverless -g
```
After this you will want to setup serverless in the project that you are going to deploy your frontend files. You can do this simply by running:
```
serverless
```
Serverless will then ask you a few questions about your environment and generate the `serverless.yml` file. 

Take this file (you can discard the rest) and place the files you want to deploy to S3 in a folder structure of `client/dist` with the `serverless.yml` file alongside. 

Modify the `serverless.yml` file like so in your project:
```
plugins:
  - serverless-finch

custom:
  client:
    bucketName: mywebsite.com
# Welcome to Serverless!
#
# This file is the main config file for your service.
# It's very minimal at this point and uses default values.
# You can always add more config options for more control.
# We've included some commented out config examples here.
# Just uncomment any of them to get that config option.
#
# For full config options, check the docs:
#    docs.serverless.com
#
# Happy Coding!

service: S3Frontend
# app and org for use with dashboard.serverless.com
#app: your-app-name
#org: your-org-name

# You can pin your service to only deploy with a specific Serverless version
# Check out our docs for more details
# frameworkVersion: "=X.X.X"

provider:
  name: aws
  runtime: nodejs12.x
```
As you can see, you need to make sure the plugin called `serverless-finch` is listed in your serverless file. Serverless generates comments and other services but for now, simply comment out everything else except what is shown above.

Additionally, you will want to install the `serverless-finch` npm package:
```
npm install --save serverless-finch
```

Modify the S3 bucket name to the name of your website and then run:
```
serverless client deploy
```
Wam! You should now see something like this once your website has been deployed:
```
Serverless: This deployment will:
Serverless: - Upload all files from 'client/dist' to bucket 'mywebsite.com'
Serverless: - Set (and overwrite) bucket 'mywebsite.com' configuration
Serverless: - Set (and overwrite) bucket 'mywebsite.com' bucket policy
Serverless: - Set (and overwrite) bucket 'mywebsite.com' CORS policy
? Do you want to proceed? true
Serverless: Looking for bucket...
Serverless: Bucket does not exist. Creating bucket...
Serverless: Configuring bucket...
Serverless: Configuring policy for bucket...
Serverless: Configuring CORS for bucket...
Serverless: Uploading client files to bucket...
Serverless: Success! Your site should be available at http://mywebsite.com.s3-website-us-east-1.amazonaws.com/
```
And then check out your deployed website at http://mywebsite.com.s3-website-us-east-1.amazonaws.com/