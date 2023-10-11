# Deploying Heroku + Node

# Deployment for Node-Mongo-Express: Heroku & Atlas

## Learning Objectives

- Describe the difference between development, test, and production environments
- Use environmental variables to keep sensitive data out of code
- Deploy a Node/Express API using Heroku
- Identify deployment debugging strategies

## About Deployment

### What is Deployment?

Deployment is the act of putting an app up on one or more Internet-connected
servers that allow users to access and use the app.

![](https://media.giphy.com/media/mi6DsSSNKDbUY/giphy.gif)

### Review Questions

**❓ How have we deployed our apps so far in this class?**

**❓ What changed in our apps when we deployed them?**

## Environments and Environment Variables

### Environments

Application environments are an important part of the context in which an
application runs. Each environment is configured to support a certain usage of
the application.

Typical application environments include:

- **Development** - environment where an app or new feature(s) are created and
run locally
- **Test** - environment where code and UI is tested for functionality and
performance
- **Production** - environment for complete and tested code to be hosted online
for clients to use

So far, we've been using the `development` environment by default when working
on our backend applications. Today we'll look at deploying to the `production`
environment, a.k.a. the public, published version of our site.

Each environment has a different set of configurations -- things that vary
depending on how we're running or using our app. We could be developing,
testing, or deploying our apps. Configuration settings often include...

- The name of the database
- The username/password to connect to the database
- API authentication keys (e.g. to connect to Twitter API)
- Whether or not to reload code on each request (for debugging vs performance)
- Where to save log information (error logs, etc)

### Environment Variables

We do not want to keep configuration in our codebase (e.g. the code we see when
we push to Github) for several reasons:

- We do not want to expose private information such as passwords and API Keys.
- When we push the same code to different environments, we need a way to
dynamically tell which environment we're in.

Node manages application environments using "environment variables".

Environment variables store data and configuration information that is defined
outside of your codebase and pertain to the phase of the application's
development.

Storing this information separately protects sensitive information like API keys
and passwords because it is not visible from your project directory.

This is accomplished using `process`, a global object that comes with all Node
projects. `process` has a property `env` where we store environment variables.

We can view a project's environment variables using the Node REPL in our
terminal.

```
$ cd <your-node-project>
$ node
> process.env

```

- *Question:* What are some of the listed variables? Why would they be stored
here?

You can create a new environmental variable in the terminal too! (terminal, not
node!)

```
$ export <YOUR_ENVIRONMENTAL_VARIABLE_NAME>=<variableValue>

```

> NOTE:
> 
> - Make sure there are no spaces next to the equals sign!
> - Environment variables tend to be SCREAMING_SNAKE_CASE by convention.

To test, try logging the following code from the node repl.

```
> process.env.<YOUR_ENVIRONMENTAL_VARIABLE_NAME> = // whatever

```

If you'd like, you can also remove the environmental variable by exiting out of
the Node CLI and entering this command in the normal terminal:

```
$ unset <YOUR_ENVIRONMENTAL_VARIABLE_NAME>

```

### Essential Questions

**❓ What are examples of environmental variables we've used?**

**❓ Why do we typically tell Git not to track our environmental variables?**

**❓ When do environmental variables get loaded?**

# Deploying Node-Express-Mongoose Applications

Deploying our Node-Express-Mongoose application consists of several steps.

First, we'll create a Heroku app in the command line.

Next, we'll verify our Node-Express-Mongoose applications are able to connect to
this cloud-hosted database.

Finally, we'll configure and deploy our application to Heroku by using Git to
push our code to their servers.

## You Do: Deploy Book-e JSON

Today, we will be deploying the solution repo of our Book-e JSON exercise. You
can download it from the link
[here](https://git.generalassemb.ly/SEI-Standard-Curriculum/booke-codealong).

1. Clone down the repo into your `sei/sandbox` directory.
2. Change into the project directory with `cd booke-codealong`
3. Install dependencies with `npm i`.
4. Create a .env file locally and add your environmental variable for
`DATABASE_URL`.
5. Verify that your app works locally by running `nodemon index.js`.

### Heroku

We'll be using Heroku to deploy our Express apps because it simplifies and
expedites the process for deployment. Heroku automatically does the following...

- Starts up a new server when we run `heroku create`, and installs all the
necessary services
- Adds a new remote to our Git repo, so we can just run `git push heroku main`
to copy our code over to their servers
- Detects our database
- Detects the language our program is written in and chooses a buildpack
- Automatically installs our app's dependencies, and starts our app
- Easily change configuration information using `heroku config`

Heroku also has a FREE pricing tier! Click [here](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) to install Heroku to your command line.

## Deployment Steps

Deployment is essentially an exercise in following directions. Follow the
step-by-step instructions below to deploy your Book-e JSON App. Pay attention to
the notes following each prompt! Fully read each prompt (including the notes)
before executing each step.

### Step A. Set up Heroku

1. Login to Heroku from the Terminal with `heroku login`.
2. From your project directory, create an app on Heroku

```
heroku create
```

> NOTE:
> 
> - Make sure you are in your Book-e JSON App project directory before you run
> this command
> - The `heroku create` command prepares Heroku to receive your code. Heroku
> will randomly create an app name for you if you don't specify one. You may
> need to login before you can create an app.
> - If you choose to name your application with `heroku create <your-app-name>`,
> you will need to use a unique name. If the name is taken, Heroku will prompt
> you to choose something new.
> - Running into an error that reads:
> `! Unable to connect to Heroku API, please check internet connectivity and try again.`
> See how to debug:
> https://help.heroku.com/GOLWIGSP/why-can-t-i-connect-or-authenticate-with-the-heroku-command-line-cli

### Step B. Heroku & Node Setup

Next we need to let our Node app know *when* to use Mongo Atlas as our database.

A built-in environment variable, `NODE_ENV` available under
`process.env.NODE_ENV`, is used to define the application environment. When a
Node app is deployed to Heroku, Heroku automatically sets the `NODE_ENV`
variable to `'production'`. You can use this variable to set other configuration
variables in your app dynamically, if you'd like to have different settings for
development vs. production.

The code below will determine the `mongoURI` for our application to connect to.

```
const mongoURI = process.env.DATABASE_URL;
```

If we had a different database for production vs development, we might do
something like this:

```
const mongoURI =
  process.env.NODE_ENV === "production"
    ? process.env.PRODUCTION_DATABASE_URL
    : process.env.DEVELOPMENT_DATABASE_URL;
```

Similarly, when Heroku starts your app it will automatically assign a port to
`process.env.PORT` (an environmental variable!) to be used in production.

In our book-e `index.js` file, notice that we use `app.listen` to accommodate
Heroku's production port and our own local development port.

```
app.set("port", process.env.PORT || 8000);

app.listen(app.get("port"), () => {
  console.log(`✅ PORT: ${app.get("port")} 🌟`);
});
```

### One VERY IMPORTANT change to make...

![](https://media.giphy.com/media/lTBEtbCOEpndFyx4Gm/giphy-downsized.gif)

Heroku also looks for instruction when starting your app. In this case, that
instruction is to run `node index.js`. In `package.json` under `scripts`, note
that we need to add the following:

```
"scripts": {
    "start": "node index.js"
}
```

Add and commit those changes. If this were your real project, you'd want to push
those changes to your remote repository as well.

### Essential Questions

**❓ What have we done so far for deployment?**

**❓ What do you think would happen if you forgot to set your `start` script?**

### Step C. Heroku & Atlas Configuration

1. In your terminal, add the MongoDB Atlas database URL to your deployed Heroku
app using `heroku config:set` in your terminal where you created your Heroku
app. Use the following commands as an example (make sure to run the
config:set command in your project's directory and replace `<password>` with
your own password):

```
heroku config:set DATABASE_URL="mongodb+srv://yourusername:<password>@cluster0-zoapi.mongodb.net/test?retryWrites=true&w=majority"
# CONFIRM
heroku config
```

![](https://i.imgur.com/2HeSXMd.png)

Note: You can also set environmental variables for your Heroku app from within
Heroku. Log into your Heroku account, go to your app's dashboard, click on
settings, and click "Reveal Config Vars". Use the pencil icon to edit existing
ones or add new key-value pairs manually.

## Step D. Deploying to Heroku with Git

1. Run `git status` to make sure that there are no changes to add or commit and
then push your code to Heroku remote with `git push heroku main`.
2. Seed your Atlas database by running the command:

```
heroku run node db/seed.js
```

> NOTE:
> 
> - `heroku run` allows you to run js files on the heroku server. We can seed
> our database on heroku using the same seed file we used locally.
1. Open your application! Run the command `$ heroku open` in your terminal. This
will launch your production app in a new browser tab.
2. You just successfully deployed your first app! You should be proud, so pat
yourself on the back, give your neighbor a high five, call your parents, and
share this milestone with someone you love!

![](https://media.giphy.com/media/YJ5OlVLZ2QNl6/giphy.gif)

## Essential Questions

**❓ What does the command `git push heroku main` do?**

**❓ Why did we need to run the following command:
`heroku config:set DATABASE_URL=...`?**

## Solving Deployment Issues

**Not working?** Don't worry! Debugging is a part of your life now, and
deployment issues are normal. Check out these tips on solving deployment issues.

### Google is Your Best Friend

More often that not, solving deployment issues requires a good deal of Googling.
Don't expect to find a silver bullet -- often we must go through many different
issues other users may have encountered to understand our own.

What should you Google?

- If you aren't able to deploy, Google the error that shows up in your terminal
after trying to push your app.
- If you are able to deploy but your app doesn't load/function properly, see
what shows up after running `$ heroku logs --tail` in the terminal.

### Heroku Errors v. Node Errors

You may notice that the errors you receive after running `$ heroku logs --tail`
are not always detailed or particularly helpful.

- Instead of focusing on the Heroku errors, pay attention to the Node errors in
your terminal. They will often provide you with more direction.
- Still stuck? Check out the
[Heroku Error Codes Documentation](https://devcenter.heroku.com/articles/error-codes).

> NOTE:
> 
> - A common error that students come across is file name case sensitivity.
> Check out
> [this documentation](https://stackoverflow.com/questions/10523849/changing-capitalization-of-filenames-in-git)
> on changing the capitalization of filenames in Git.

### Help Each Other Out!

If you notice somebody running into the same problem as you, try working
together on debugging it!

## Additional Resources

- [About Environments](./dev_environments_variables.md)
- [The Heroku CLI | Heroku Dev Center](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)
- [Causes of Heroku H10-App Crashed Error And How To Solve Them](https://dev.to/lawrence_eagles/causes-of-heroku-h10-app-crashed-error-and-how-to-solve-them-3jnl)
