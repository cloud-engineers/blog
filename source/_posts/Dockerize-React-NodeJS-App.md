---
title: Dockerize React/NodeJS App with Postgres
date: 2019-11-22 15:57:27
---
Sometimes you will want to run both a frontend React app, a backend NodeJS server, and a Postgress database from the same host.

##### Quick Start
Docker compose has been an excellent way to achieve this while working with serveral different applications that will need to work together (See https://docs.docker.com/compose/). Once you've installed Docker, compose should be installed as well.

Cd into your React project and create a Docker file with:
```
touch Dockerfile
```
Add the following contents:
```
FROM node

RUN mkdir -p /srv/app/client
WORKDIR /srv/app/client

COPY ./package.json /srv/app/client
COPY ./package-lock.json /srv/app/client
COPY . /srv/app/client

RUN npm install

EXPOSE 3000

CMD npm start
```
Now cd into your NodeJS project and create another Dockerfile with the following contents:

```
FROM node

WORKDIR /app

COPY ./package.json .
COPY ./package-lock.json .

RUN npm install

COPY . .

EXPOSE 3001

CMD npm start
```
Now that you have set up your Dockerfiles, you are now ready to create your docker-compose file. I usually place this file in a directory above the project directories:
```
version: "3"
services:
  app:
    build: ./node-server
    depends_on:
      - postgres
    environment:
      DATABASE_URL: postgres://user:pass@postgres:5432/db
      NODE_ENV: development
      PORT: 3001
    ports:
      - "3001:3001"
    command: npm run dev
    volumes:
      - .:/app/
      - /app/node_modules

  postgres:
    image: postgres
    ports:
      - "35432:5432"
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: db

  client:
    build: ./react-app
    environment:
      - REACT_APP_PORT=3000
    expose:
      - 3000
    ports:
      - 3000:3000
    volumes:
      - ./react-app/src:/srv/app/client/src
      - ./react-app/public:/srv/app/client/public
    links:
      - app
    command: npm run start
```
You'll need to make some edits to this file regarding the foldernames. This file assumes you have a folder named react-app and node-server.
After this, return to the directory with the compose file and you should be able to execute:
```
docker-compose up
```
And Boom! You are now a docker ninja!