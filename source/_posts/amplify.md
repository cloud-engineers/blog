---
title: Using Amplify from AWS
date: 2019-10-24 14:12:35
---
Amplify is useful tool from AWS which is helps with deploying to Amazon services. If you are wanting to get websites up fast via s3 buckets, it should be on your radar.

##### Quick Start
Download Amplify by following the instructions at https://aws-amplify.github.io/docs/. Once you have it installed and you have your AWS profile set up, you should be good to start using Amplify.

Cd into your project you want to deploy and run 

```
amplify configure
```

It will ask you a series of questions about your project stack. Once you complete the questions, now it is time to add s3 bucket hosting by running

```
amplify add hosting
```
Again, you will be asked about your environment preferences. After the questions, you can run

```
amplify publish
```
and your code will be deployed to your s3 bucket!

