# Build and Deploy a Full Stack MERN Project - React + Redux, Node, Express, MongoDB

![Memories](https://i.ibb.co/Z8Y0CJv/Screenshot-2020-10-30-at-11-10-04.png)
Video Part1: https://www.youtube.com/watch?v=ngc9gnGgUdA
Video Part2: https://www.youtube.com/watch?v=aibtHnbeuio

## Materials/References:
HTTP status codes: https://www.restapitutorial.com/https...
MongoDB Atlas: https://www.mongodb.com/cloud/atlas
MemDev: https://mem.dev/
Background: https://www.svgbackgrounds.com/
Redux-Provider-Reducer: https://tsh.io/blog/react-state-management-react-hooks-vs-redux/

## Introduction
This is a code repository for the corresponding video tutorial. 

Using React, Node.js, Express & MongoDB you'll learn how to build a Full Stack MERN Application - from start to finish. The App is called "Memories" and it is a simple social media app that allows users to post interesting events that happened in their lives.

By the end of this video, you will have a strong understanding of how the MERN Stack works.

Setup:
- run ```npm i && npm start``` for both client and server side to start the app

## client

Initialize:
npx create-react-app
npm install axios moment react-file-base64 react-redux redux redux-thunk @material-ui/core
remove all files in src
create src/index.js
package.json > add "proxy": "http://localhost:5000" - make sure to use app.use(cors()) before specifying the routes in server/index.js  --> these fix cors issue from client request

## server

Initialize:
npm init -y
npm install body-parser cors express mongoose nodemon
package.json > add "type":"module" --> allow using import instead of require  (SINCE nodejs 12.x) -- this is not needed with create-react-app as it is already there

mongodb/cloud/atlas
- create Project
  - ReactMernMemories
- ReactMernMemories > Build a cluster > shared cluster > AWS > create cluster
- database access > new database user  -read & write DB
    - reactMemUser / cfOQBeWsJdH6JTLq
- network access > IP Whitelist > add IP adress > add current ip adress
- cluster > CONNECT > Connect your application > driver = node.js > copy connection detail and use it in noder/server

# Deploy app

## PART01 & PART02: deploy server to heroku

dashboard.heroku.com > signup > new app > rlu-memories-project
-> download heroku-cli

Before deploying:
- update .env : remove PORT : 'process.env.PORT' will be populated automatically by heroku (used in index.js)
- add Procfile : 
  web: npm run start
  --> enable heroku to start the app as soon as the application is deployed

heroku-cli:
- heroku -v
- heroku login > follow the prompts to connect through the browser (SSH key)
- cd ../server

If git repo does not exist yet
- git init
- heroku git:remote -a rlu-memories-project
- git add .
- git commit -am "c"
- git push heroku master

Existing git repo with already committed code
- heroku git:remote -a rlu-memories-project

dashboard.heroku.com > open app
- we should see Hello to Memories API (see index.js)
https://rlu-memories-project.herokuapp.com/
- we should see all available posts:
https://rlu-memories-project.herokuapp.com/posts


--> server app available from deployed location: xxxxx.herokuapp.com/posts

## PART01 & PART02: deploy client to heroku

### connect to heroku
update index.js
- const url = 'https://rlu-memories-project.herokuapp.com/posts'

### deploy

app.netlify.com > signup > sites > "drag and drop your site folder here"
npm run build  (generate the build folder)
from explorer, drad and drop the 'build' folder (select the build folder) into above netlify

--> client app available from deployed location: xxxxx.netlify.com 
--> see netlify app settings : possible to update the domain name

APP IS LIVE FROM THE WEB
