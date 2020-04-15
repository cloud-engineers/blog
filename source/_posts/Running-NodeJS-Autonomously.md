---
title: Running NodeJS Autonomously
date: 2020-04-15 09:45:52
tags:
---
In setting up an automatic deployment (Continuous Deployment) and automatic tests (Continuous Integration) as part of your deployment,
you may need to run NodeJS projects as jobs that run on the server without manually being started by running `npm start` via an ssh session.

Crontab can be a solution, perhaps even nohup (f.e. `nohup ping google.com &`) but there is an npm package called [forever](https://www.npmjs.com/package/forever) that could be more familiar for Javascript developers.

Install forever by running: 

`npm install forever` 

You can start and run "forever" a typical NodeJS project by running:

`forever start -c "npm start" ./`

You can also point to an `app.js`, `index.js`, or `bin/www` file (the typical starting file of a NodeJs project) to run your project with forever.

NOTE: You can also pass in environment variables to your script with:

`PORT=3000 NODE_ENV=test forever start -c "npm start" ./`

Once you've started your project with forever, look at your node project/script running:

`forever list`

There are also individual logs for each of the jobs your run which may be very helpful when debugging issues. F.e. `username/.forever/yivz.log`

For cleanup, you can run the following commands for your jobs running with forever:

To stop your jobs, run `forever stopall`

If there are still remaining node processes run:

```
kill -9 $(lsof -t -i:$PORT)
killall node
```