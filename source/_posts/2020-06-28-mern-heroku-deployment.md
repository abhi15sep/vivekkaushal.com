---
title: "Deploying a MERN Web-App to Heroku"
date: 2020-6-28
categories:
- Guide
- Full-Stack
tags: 
- MERN
- Heroku
cover: images/mern_heroku.jpg
thumbnail: images/mern_heroku.jpg
---

This post builds upon the guide to developing projects in MERN stack that I posted a while back, you can find it [here](https://kaushalvivek.github.io/2020-04-11-mern/). 

Heroku probably has the best free-tier service out there for hosting MERN stack web applications. Though it requires certain non-intuitive settings before you can go about deploying your app. This article will guide you through the process of deploying your web-app on Heroku.

- We would be deploying the server and the frontend on the same deployment instance.
- The database being used here is MongoDB Atlas.
- This deployment uses npm instead of yarn for deployment.

<!--more-->

### Step 1 - App Directory Structure

Your app comprises of a client (the React app) and a server, the structure should be as follows for deployment on Heroku:

```
/project_root
  |_ index.js (your server file)
  |_ models
  |   |_ model1.js
  |   ...
  |_ routes
  |   |_ route1.js
  |   ...
  |_ package.json
  |_ package-lock.json
  |_ client (react app root)
      |
     ... react app files
```

### Step 2 - Add Handler for Client Build

Add the following code to your *index.js (server)* file. This tells the server to look for a build of the react app.

```js
if (process.env.NODE_ENV === 'production' || process.env.NODE_ENV === 'staging') {
  app.use(express.static('client/build'));
  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname + '/client/build/index.html'));
  });
}
```

### Step 3 - Add Scripts to *package.json (server)*

Add the following to your server's *package.json*. If a *"script"* key is already present, add *"start"* and *"heroku-postbuild"* to it.

```json
"scripts": {
    "start": "node index.js",
    "heroku-postbuild": "cd client && npm install && npm run build"
  },
```

### Step 4 - Procfile

Add a *Procfile* to your server root folder. Use the following commands in your terminal to do so:

```bash
echo "web: node index.js" > Procfile
```

### Step 5 - Creating a path for API

We are using the same URL for our API calls and for serving our app, hence, we'll create a sub-path specifically for APIs. This path needs to be properly defined in both our server file (*index.js*) and our *axios* API calls.

While defining routes, add a parent sub-path (*/api*)to all routes. Make the following changes to your server file *index.js*:

Do this:

```js
const usersRouter = require('./routes/user');
app.use('/api/users', usersRouter);

const friendsRouter = require('./routes/friend');
app.use('/api/friends', friendsRouter);
```

Instead of:

```js
const usersRouter = require('./routes/user');
app.use('/users', usersRouter);

const friendsRouter = require('./routes/friend');
app.use('/friends', friendsRouter);
```

This needs to be done for all the routes in your application.  

When calling the API using *axios* from your react app, ensure that the API is call is directed to the correct route. Don't define the full path or the server port, that is handled automatically in Heroku. For example, calling the */api/friends* route defined about, well call:

```js
componentDidMount() {
    axios.get('/api/friends/')
      .then(response => {
        // do stuff
      });
}
```

### Step 6 - Server Port and Proxy

We saw how we need not mention the server port in Heroku, as that is handled by the platform. We need to get this port and use it to serve our API. This is done by simply using the port defined in the environment. Add the following to the top of your server *index.js*:

```js
const port = process.env.PORT || 5000;
```

Now that we've defined the port, we need to add a proxy to our client's *package.json* file, so that when axios tries to reach our server in a local deployment it can reach our server. Add the following to the bottom of your *package.json* file in your **client** folder. **NOT** the one in your **project_root**. This is ignored by Heroku, but is used by your local deployment.

```json
"proxy": "http://localhost:5000"
```

### Step 7 - Client Build

Build your react app by running the following in your *client* folder:

```bash
npm run build
```

You can also use yarn to do the same:

```bash
yarn build
```

### Step 8 - MongoDB Atlas URI and Access Settings

Make sure that your MongoDB Atlas URI is correctly configured in your server *index.js*. Also ensure that your MongoDB Atlas is setup such that it would accept access requests from your server's IP *(recommended to whitelisting access to all IPs for a Heroku deployment)*. You can find more information on how to do this here:

- [Video on how to whitelist all IPs *(recommended setting for Heroku deployment)*](https://www.youtube.com/watch?v=leNNivaQbDY)
- [More information on Heroku and MongoDB Atlas integration](https://stackoverflow.com/questions/42159175/connecting-heroku-app-to-atlas-mongodb-cloud-service)
- [More information on MongoDB Connection String](https://docs.mongodb.com/manual/reference/connection-string/)

You can check my guide to creating a MERN project [here](https://kaushalvivek.github.io/2020-04-11-mern/), for more information on how to setup your MongoDB URI in your server *index.js*.

### Step 9 - Pushing to GitHub

We'd be using GitHub to deploy our code. Push your code, in the existing directory structure, to GitHub. Create a new repository in GitHub and copy the ***remote_repo_URL***. Then execute the following commands in your *project_root* folder.

```bash
git init
# start a git project
echo node_modules > .gitignore
echo client/node_modules > .gitignore
# add node_modules to git ignore
git add .
# add files to push to repository
git commit -m "First Commit"
# create your first commit
git remote add origin remote_repo_url
# copy remote_repo_url from the target GitHub Project
git push -u origin master
# push your code to the chosen GitHub Project
```

### Step 10 - Connecting Heroku to GitHub

- Go to [heroku.com](https://dashboard.heroku.com/apps)
- Create a new app, choose an app name. Your app url would be *name.herokuapp.com*
- Choose *Connect to GitHub* as the deployment method
- If your GitHub account isn't connected to Heroku, do so
- Search for, and choose the project repository we created in the previous step
- Enable automatic deployments and select the deployment branch
- Click on **Deploy Branch**

And that's it! You're done. Your app would now be deployed. If you face errors, you can debug them using Heroku's logs. Feel free to reach out to me.