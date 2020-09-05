---
layout: post
title: "Guide to Deploying a Flask app on Heroku"
date: 2020-3-30
categories:
- Guide
- Full-Stack
tags:
- Flask
cover: images/flask_heroku.jpeg
thumbnail: images/flask_heroku.jpeg
---

Heroku is a popular application deployment platform with a functional free tier of services, and Flask is populalar application development micro-framework in Python. The Heroku-Flask environment is one of the quickest ways to deploy a small application for testing, yet sadly, I did not find a single tutorial that covered all aspects of the deployment process without leaving room for a whole bunch of errors.

Hence, this guide.

<!--more-->

Note that this is not a guide to Flask and Flask based development and is focused specifically on deployment of your flask application. There are excellent resources online for learning development Flask, which would get you started in a day, so refer to that if you need to get going in Flask, before learning to deploy your projects online - here.

## Setup Environment and Git
Your deployment requires a root folder, we'll let it be : flask-deployment. Create this folder, cd into it and install flask and gunicorn in a new virtual environment:

```bash
mkdir flask-deployment
cd flask-deployment
pipenv install flask gunicorn
```

Run the following in root (flask-deployment) to setup git:

```bash
git init
git add .
git commit -m "Initial Commit"
```

## Setup Heroku

You need to have heroku-cli installed on your system, and have a Heroku account configured to proceed with this step. If you don't have these requirements fulfilled, do the following first:

### [Sign Up for Heroku](https://signup.heroku.com/)


### [Install heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)

Once you're done with these installations and sign-up, run the following commands to setup Heroku in the created folder flask-deployment:

*Code to login and create a new app*
```bash
heroku login
heroku create my-first-app
```
*Code to push initial commit to heroku*
```bash
git push heroku master
```

Once you're done with this, move forward to working on your app.

## The App

Inside this folder, create an app folder which would contain all files related to your Flask application. For more information on how to create a Flask application, refer to the tutorial [here.](https://medium.com/bhavaniravi/build-your-1st-python-web-app-with-flask-b039d11f101c)

## Nomenclature

Ensure the following specification in your flask app, for the sake of nomenclature used in this guide:
- your applicaiton is inside the app folder, i.e., it's path is flask-deployment/app/main.py
- your app is named main.py
- your app is named app, as in app = Flask(__name__)  -- default naming.

## Additional Files

Now that you've created your app and tested it locally for functionality (refer to Flask tutorial for any help), let's move on to deployment.

Create the following files in the root folder (flask-deployment):

### wsgi.py

Create a wsgi.py file with the following content:

```python
from app.main import app

if __name__ == "__main__":
	app.run()
```
**NOTE:** *Ensure that the nomenclature specified earlier is followed.*

### runtime.txt

Create a runtime.txt file with your Python version. For me, as of now, its 3.7.5:

```bash
python-3.7.5
```

### Procfile

By far the easiest file to mess up, and hardest to debug, your Procfile should contain the following, *(given that you've followed the nomenclature mentioned)*:

```bash
web: gunicorn wsgi:app
```

### requirements.txt

Once you're done with development on your app and have included all necessary modules, generate a requirements file using:

```bash
pip freeze > requirements.txt
```

**NOTE:** *Ensure that this requirement file also contains gunicorn, if it doesn't, add gunicorn to the last line of requirements.txt manually.*

## Deployment and Managing Dynos

Run the following code blocks to deploy this project:

### Pushing code files to Heroku

```bash
git add .
git commit -m "Code files"
git push heroku master
```

### Ensuring that a web dyno is running for deployment:

```bash
heroku ps:scale web=1
```

Additionally, you can check if your app is working correctly by running the following in your project's root folder:

```heroku local web```

### *Tip on Database*

Do not use an sqlite3 database *(commonly used for basic Flask applications)* with Heroku deployment. Heroku does not support sqlite3 due to the manner in which sqlite accesses memory. Use POSTGRES instead.

And that's it!  

Your application will be live on Heroku after this. If you face any issues or have any doubts, feel free to reach out to me at [vivek.kaushal@outlook.com](mailto:vivek.kaushal@outlook.com).  

Cheers!