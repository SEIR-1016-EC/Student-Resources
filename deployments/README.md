## What is Deployment?

Deployment is the act of putting an app up on one or more internet-connected
servers that allow users to access and use the app.

### Requirements for Deployment

There are generally a few things we need for an app to be properly deployed:

- **Executable Code** - we must get our code onto the server and be able to run
it
- **Dependencies** - the server(s) where our app will run must have the proper dependencies installed
- **Services** - the server(s) must be running the correct services (web,
database, email, etc.)
- **Configuration** - we must configure our running app with respect to its
deployment environment

### Deployment Approaches

There are lots of ways to do each of these steps. For example, we can get our
code onto a server by...

- Using FTP (File Transfer Protocol) to transfer the files onto the server.
- Adding a git remote and using `git push` to transmit files a hosting platform - ex: GH Pages.
- Linking a GH / Gitlab to a 3rd party storage platform (some popular free / low-cost options include Railway, Fly.io, Netlify, and Heroku).

# 3rd Party Deployment Platforms

### Unit 1

[Deploying Your App to GitHub-Pages](./github_pages.md)

### Unit 2

[MongoDB Atlas Guide](./mongo_atlas.md)

[Heroku with Node & Mongoose!](./heroku_node_mongoose.md)

[Developer **Environments & Environment Variables**](./dev_environments_variables.md)

### Unit 3

[React Deployment to Netlify](./react_deployment.md)

### Unit 4

[Django Deployment](./django_deployment.md)