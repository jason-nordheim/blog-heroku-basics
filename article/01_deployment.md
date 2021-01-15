# Deploying an Express Application

Pre-requisites:

- Required:
  - Directory Containing
    - initialized with NPM (e.g. contains a `package.json`)
    - defined `"engines"` variables
    - defined `"start"` script script
    - defined Express application, launching express using an `process.env.PORT` (if defined)
    - `.gitignore` file excluding `node_modules` directory
  - Heroku Personal Account
  - Github Account
- Recommended:
  - OS: Mac OS
    - with homebrew installed

Assuming you have all the steps above in place (this was covered in part 1), you should be all set to follow along with deploying to your Express application to Heroku.

## Pushing our Code up to Git

Heroku defaults to a continuous deployment (CD) model in which Heroku retrieves the entire project (e.g. our code) from git/github.

1. Initialize Repository
   - from the project's root directory of the project (same level as `package.json`), run the command `git init`.
   - if executed successfully, the `git init` command should produce a message that says the repository was initialized
2. Add Code to the repository
   - from the project's root directory, run the command `git add .` to add everything in this directory (recursively)
3. Commit the code

- this is how we mark a snapshot of our project's state in git
- we use the command `git commit -m '<COMMIT_MESSAGE'`, and by convention, the first commit in git is typically attached with the message `initial commit`

  ```sh
  git commit -m "initial commit"
  ```

  - successful execution of the commit will print a message confirming exactly what code was committed to be tracked in git. It should look something like:
    ```sh
    [master (root-commit) bd394f9] initial commit
    4 files changed, 245 insertions(+)
    create mode 100644 .gitignore
    create mode 100644 index.js
    create mode 100644 package-lock.json
    create mode 100644 package.json
    ```
  - note: you should not see any files or directories from the `node_modules` directory

## Install Heroku CLI

The git executable is a pre-requisite to install the Heroku CLI, so now that we have git installed, and have successfully initialized our project with git, we can install the Heroku CLI which will help us deploy our application to the web.

> Since this could change at any time, and the instructions differ for downloading and installing Heroku depending on your OS, it is always best to reference the [Heroku Dev Center](https://devcenter.heroku.com/articles/heroku-cli) for the best information on setting this up.

On Mac OS (with the homebrew package manager and xCode CLI tools installed) the install is very simple, all we have to do is run the command:

```sh
brew tap heroku/brew && brew install heroku
```

And let homebrew take care of the rest. We can verify it installed correctly by running `heroku -v` in terminal. If installed correctly, you should see output displaying the version of the Heroku CLI as well as the underlying technologies that the CLI was developed upon.

## Setup Heroku CLI

Since everything you do with Heroku is associated with your Heroku account, it is no surprise that you will need to provide account credentials to use the Heroku CLI.

To login in to your Heroku account with the CLI, simply run the command `heroku login`, then you enter your email and password.

## Creating a Heroku App with the CLI

Now that we've configured the CLI with our credentials, we will need to create scaffold out the resources for our project on Heroku's platform.

In the CLI, we create a new application by running the command: `heroku create`. If executed successfully, you should see output that states `"Creating app...done"` followed by the randomly generated application name and two URLs.

There are three parts to take note of here:

1. After `Creating app... done` is the randomly generated name of the application. This will be how you identify your application from the Heroku console.
2. The first URL is the public URL upon which the application will be accessible.
3. The second URL is the git repository where we will push our production code.

> It is a good idea to save these credentials.

Make sure to save

## Deploying our Code

With our git repository initialized, our code committed, a Heroku account setup, the heroku CLI setup and configured with your Heroku account, and a Heroku app created with a public URL and a git repository - we have all the pieces in place to push our code to Heroku.

Using the second URL provided folloing the `heroku create` command, (e.g. the one that ends in `.git`), you will need to use git to associate a new remote repository with your application. The command will look like:

```sh
git remote add ALIAS PROJECT_URL
```

> The ALIAS is user-defined. So choose a name for the remote repository that makes sense to you. It may make sense to simply denote the remote repository as `heroku`.

> If the `git remote add heroku PROJECT_URL` produces a message saying `fatal: remote heroku already exists` that is totally fine. You are seeing that output because the Heroku CLI already took care of adding it for you.

Now that git knowns where our remote repository is located, we need to use the `git push REMOTE BRANCH` command to send our code. The `REMOTE` is a reference to the alias defined earlier (`heroku`), and the default branch in `master`.

If you've followed along verbatim, the `push` command should be:

```sh
git push heroku master
```

This command should produce several lines of text within the terminal window providing information on what is happening at each step. First it will copy the repository over to the remote repository Heroku just setup for you, then create the runtime environment (Node/NPM), then install the associated binaries, build the dependencies, and perform basic tree shaking to minify the deployed code bundle.

If this was successful, you will see a message near the end saying `Build Successful`. Then `Launching...` followed by the version number (`v1` if you got this working on the first try).

## Verifying Deployment

As with anything, it's good to verify that things are working. Since our express application is very basic; containing a single route handler for just GET requests to `/`, we can simply open a browser and navigate to the public URL (provided by Heroku) for our application.

If you're route-handler looks like mine - opening the page in the browser should show the JSON object we told our express application to respond with anytime a GET request to the `/` (root) directory occurs.

In my case:

```js
{"message":"hello from express"}
```

# Congratulations

Congratulations! You've now successfully deployed an Express application to Heroku.
