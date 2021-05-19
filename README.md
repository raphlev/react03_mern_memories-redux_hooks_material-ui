# Build and Deploy a Full Stack MERN Project - React + Redux, Node, Express, MongoDB

![Memories](https://i.ibb.co/Z8Y0CJv/Screenshot-2020-10-30-at-11-10-04.png)
Video Full MERN Series: https://www.youtube.com/watch?v=ngc9gnGgUdA&list=PL6QREj8te1P7VSwhrMf3D3Xt4V6_SRkhu&t

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

## Init client

Initialize:
npx create-react-app
npm install axios moment react-file-base64 react-redux redux redux-thunk @material-ui/core
remove all files in src
create src/index.js
package.json > add "proxy": "http://localhost:5000" - make sure to use app.use(cors()) before specifying the routes in server/index.js  --> these fix cors issue from client request

Code from https://github.com/adrianhajdin/project_mern_memories/tree/PART_4
update package.json (dependencies)
    "axios": "^0.21.1",
    "dotenv": "^8.6.0",
execute 'npm i' & 'npm audit fix'
rename public/memories.png into public/favicon.ico
remove this line in public/index.html:  <link rel="icon" type="image/png" href="./memories.png">
update api/index.js:
    import dotenv from 'dotenv';
    dotenv.config();
    const SERVER_URL = process.env.SERVER_URL|| "http://localhost:5000";
    const API = axios.create({ baseURL: SERVER_URL });
update Auth/Auth.js with new google client ID (see above google credential for client)

## Init server

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

Code from https://github.com/adrianhajdin/project_mern_memories/tree/PART_4
rename .Procfile into Procfile (not needed)
update package.json (dependencies)
    "axios": "^0.21.1",
execute 'npm i' & 'npm audit fix'
update index.js
    import dotenv from 'dotenv';
    dotenv.config();
    // const CONNECTION_URL ..
    mongoose.connect(process.env.CONNECTION_URL, { useNewUrlParser: true, useUnifiedTopology: true })
add .gitignore
    node_modules
    build
add .env
    PORT=5000
    CONNECTION_URL=mongodb+srv://reactMemUser:Mw1NOyXk19m5ASlL@cluster0.tr43l.mongodb.net/memories?retryWrites=true&w=majority

## google credential for client

react-google-login is used in Auth.js
You can create a clientID by creating a new project on Google developers website.
- https://console.cloud.google.com/apis/dashboard?project=rlu-memories&folder=
- go to OAuth consent screen: https://console.cloud.google.com/apis/credentials/consent?folder=&project=rlu-memories
- create new Oauth Consent screen:
App name : rlu-memories
User support email : raphaellevequeptc@gmail.com
Developer contact information Email addresses : raphaellevequeptc@gmail.com
- go to credentials: https://console.cloud.google.com/apis/credentials?project=rlu-memories
- create credentials > Oauth Client Id
Authorized JavaScript origins
For use with requests from a browser
URIs *
https://localhost:3000
https://rlu-memories.netlify.app
http://localhost:3000

Authorized redirect URIs
For use with requests from a web server
URIs *
http://localhost:3000
http://localhost:3000/auth
https://rlu-memories.netlify.app
https://rlu-memories.netlify.app/auth

--> Copy Your client ID and set it to <GoogleLogin clientId >

## deploy server to heroku

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

- heroku config  
- heroku config:set CONNECTION_URL="mongodb+srv://reactMemUser:Mw1NOyXk19m5ASlL@cluster0.tr43l.mongodb.net/memories?retryWrites=true&w=majority"
- heroku config  

- git push heroku master
- heroku logs --tail  

--> server app available from deployed location: https://rlu-memories-project.herokuapp.com/

## deploy client to netlify

Deploy from GIT  

Update Site Name to rlu-memories: create same client site domain as indicated in 'google credential for client' above: rlu-memories.netlify.app

Build Settings:
- Repository: https://github.com/raphlev/react03_mern_memories-redux_hooks_material-ui
- Build command: npm run build
- Publish directory: client/build
- Env Variable: SERVER_URL=https://rlu-memories-project.herokuapp.com
- Set env variable NODE_ENV=production
- Set env variable CI=false  (to avoid warning treates as error: https://answers.netlify.com/t/new-ci-true-build-configuration-treating-warnings-as-errors-because-process-env-ci-true/14434)
Click Deploy and go to settings/deploys: Build settings
- Base directory: client
- Build command: npm run build
- Publish directory: client/build
- Deploy log visibility: Logs are public
- Builds: Active
Update Site Name to:
- rlu-memories

--> Client running on https://rlu-memories.netlify.app/

## Improvment
- add package.json in backend directory and move server dependencies and script into this new file
- update root package.json > "start": "npm run start --prefix backend",
