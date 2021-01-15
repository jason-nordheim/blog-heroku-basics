# Getting Started: Heroku and Node

It doesn't matter how cool your application is, how many features it has, or how useful it is if no one can use it. In order to get people using it, you need to deploy it. [Heroku](https://www.heroku.com) is a development platform designed to help developers get their applications out to their end-users.

> There are a ton of providers that offer deployment services, including [AWS](https://aws.amazon.com/), [Google Cloud Platform](https://cloud.google.com/)/[Firebase](https://firebase.google.com/), and many more. Each vendor will vary in both the features offered, as well as pricing and ease-of-use.

# Deployment Checklist

In a dream world, we might have a magic button that deploys our applications in a single step. However, in the real world - there is some pre-work required in order to get the application ready for deployment.

In the case of deploying a Node application to Heroku, that pre-work includes the following:

1. Dynamic Port Binding & Environment Configuration
   - most providers host multiple application on a single physical system
   - ports are specific to applications (can only be used by one application at a time)
   - we need to get the port that our provider (Heroku) has specified, and use that for our Node application
   - this will be injected into the project (by Heroku) as an environment variable
2. Define Start Script ( in `package.json`)
   - provide `"engine"` configuration
   - provide instruction from running applications
     - define start script
3. Create .gitignore
   - remove development dependencies
   - make sure to exclude `node_modules`

> The steps for competing platforms is very similar

## A simple Express application

Node and Express themselves are not really the focus of the article, so we won't be reviewing those particular technologies here. However a simply Node.js application can be scaffolded out pretty simply:

1. In terminal (or command prompt) create a project folder
2. Navigate inside the project directory initialize NPM using the command: `npm init -y`
3. Still inside the root directory: install the Express package using `npm install express`
4. Inside the same folder, create a file that will hold the Node/Express code. By convention, this is typically named `index.js`
5. Inside `index.js`, we can define a simple Express application as:

   ```js
   const express = require("express"); // import express library
   const app = express(); // initalize new express application

   // create route handler for the index route ('/')
   app.get("/", (req, res) => {
     res.send({ message: "hello from express" }); // reply to GET requests with a JSON object
   });

   // start listening to port 5000
   app.listen(5000);
   ```

Now that we have an Express application, we can need to start the pre-work to get it ready for deployment.

## Environment Configuration and Port Binding

The first part of the pre-work for preparing an Express application for deployment to a cloud provider begins with changing the port which express uses.

In the simple express application we just outlined we can see that we provided the instruction to Express to listen for incoming HTTP requests on port 5000 with `app.listen(5000)`. This is fine while building the application, but cloud providers run many applications per system, so we need to use whatever port Heroku tells us to for our application to work.

Heroku exposes this port to our application through an _environment variable_. We can dynamically figuring out the what port Heroku wants us to use by accessing a variable from the `process.env.PORT`.

Making this change, will result in an `index.js` file that looks like:

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send({ message: "hello from express" });
});

// retrieve the environment variable defined as PORT
// if one is defined, otherwise use 5000
const PORT = process.env.PORT || 5000;

// tell the app to listen on the retrieved port
// and log out the PORT to the console so we know what port to
// send our HTTP request to
app.listen(PORT, () => Console.log(`Server running on: ${PORT}`));
```

Next we need to add some configuration to our `package.json` so that Heroku has all the information it needs to run the code. Right now you `package.json` should look similar to:

```json
{
  "name": "Server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "4.17.1"
  }
}
```

> As Node an express change, pieces of this file may be different. I hard-coded the version number of express so that regardless of new releases, you should be able to follow along using the code above.

## Engine Declaration

Now we need to add a new attribute to the JSON object defined here called `"engines"`. The engines attribute allows us to define what technologies will be required (along with their versions). In order to run the very basic Express application above, we are using two primary technologies: `"node"` and `"express"`.

Updating the `package.json` with our `"engines"` should result in a `package.json` file that should be similar to the example below:

```json
{
  "name": "Server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "engines": {
    "node": "14.0.0",
    "npm": "6.14.10"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

With the `"engines"` defined, Heroku can now inspect the `package.json` to see if any specific versions of technologies should be used to run our application.

## Start Script

There are numerous configurations that modern JavaScript applications can have and in order to deploy our application to Heroku, we will need to tell Heroku how to do run our specific application. Just like informing Heroku of which `"engine"` to use, we define our start-up script. Since Heroku always will try to run Node applications by run the `npm start` command, we will need to define that particular command.

We do this by modifying our `package.json`. All of the command for our application are associated with the `"scripts"` object within the `package.json`. We can define our `"start"` script with `"start" : "node index.js"`.

```json
{
  "name": "Server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "engines": {
    "node": "14.0.0",
    "npm": "6.14.10"
  },
  "scripts": {
    "start": "node index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "4.17.1"
  }
}
```

## Development Dependencies and Git Setup

Since we will be deploying our Express app to Heroku using git, we will need to tell git to ignore files that it doesn't need. As with anything we want git to ignore, we define which files should be ignored within a `.gitignore` file (place in the root of the project's directory).

> By using git to deploy our Express application, we will be able to easily deploy updates.

Creating a `.gitignore` file from the command line can be done simply with `echo "**/node_modules/**" >> .gitignore` to tell git to ignore any subdirectory named `node_modules`. Alternatively, we can specify just the root directory of `node_modules` with `echo "node_modules" >> .gitignore`.

And that's it. That's all you have to do, to get a basic Express application prepped for deployment to Heroku.
