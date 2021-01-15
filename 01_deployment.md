# Deploying an Express Application

Pre-requisites:

- Directory Containing
  - initialized with NPM (e.g. contains a `package.json`)
  - defined `"engines"` variables
  - defined `"start"` script script
  - defined Express application, launching express using an `process.env.PORT` (if defined)
  - `.gitignore` file excluding `node_modules` directory
- Heroku Personal Account
- Github Account

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
