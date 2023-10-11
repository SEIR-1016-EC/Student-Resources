# Mongo Account Creation

![](https://i.imgur.com/B42NavR.jpg)

## Intro

While developing an application that uses MongoDB, you can only connect to a local MongoDB engine for so long because the application, once deployed, will have to connect to a MongoDB engine accessible via the Internet.

So it makes sense to set up and connect to a hosted MongoDB sooner, rather than later.

The most popular service for hosting MongoDB databases, not surprisingly, is MongoDB's own [Atlas](https://www.mongodb.com/atlas/database).

## Create an Atlas Account

First you will need to sign up for a free account [here](https://www.mongodb.com/cloud/atlas/register):

![](https://i.imgur.com/597Ppil.png)

Accept the Terms of Service:

![](https://i.imgur.com/8FtKqrf.png)

<img src="">

### Answer the Marketing-Related Questions

![](https://i.imgur.com/IK7pg5W.png)

After clicking **Finish**, select **Shared** (FREE), then click **Create**:

![](https://i.imgur.com/Ga9ySXX.png)

### Select a Cloud Provider and Region

There are a limited number of regions that offer the free shared option.

Be sure to browse AWS, Google Cloud & Azure and select the region that's nearest you, e.g., Azure is the only cloud provider that offers a region in California:

![](https://i.imgur.com/BfbYneu.png)

### Security Quickstart

There will be a message at the bottom left stating that the database cluster is being provisioned along with the approximate amount of time it will take to complete.

While the cluster is being provisioned, create a **database user** (this is different from your account user).

The database user's credentials will need to be embedded in the "connection string" used to connect to the database.

Enter a username and password, then click **Create User**:

![](https://i.imgur.com/Ou4wZHf.png)

Skip the *Where would you like to connect from?* section and click **Network Access** in the sidebar menu instead:

![](https://i.imgur.com/MexQC5v.png)

Now click **Add IP Address**:

![](https://i.imgur.com/wFAMhVR.png)

In the resulting modal, click **ALLOW ACCESS FROM ANYWHERE** then **Confirm**:

![](https://i.imgur.com/3Wge37M.png)

After a few moments, the Status will change from "Pending" to "Active"

## Obtain the Connection String

> IMPORTANT:  Database connection strings contain the username and password for connecting to the database.  Never include the connection string in the project's source code - use a .env file instead.
> 

To obtain the connection string that will be added to a project's `.env` file, first click **Database** under DEPLOYMENT in the sidebar then click the **Connect** button:

![](https://i.imgur.com/W7cHXuw.png)

Select the **Connect your application** option:

![](https://i.imgur.com/qMOAxVV.png)

Next, ensure that the **Node.js** driver and latest version is selected.  Then click the "Copy" button to add the connection string to your clipboard and close the dialog:

![](https://i.imgur.com/2oTK6nP.png)

❗️Save the copied database connection string in a safe place for future reference!  You will be using it in several projects.

## Use the Connection String in Your App

You can now paste the connection string in the app's `.env` file, assigning it to a `DATABASE_URL` environment variable:

```
DATABASE_URL=mongodb+srv://sei:<password>@sei-w0kys.azure.mongodb.net/myFirstDatabase?retryWrites=true
```

You're almost done, but you **need to update** the connection string as follows:

1. Replace `<password>` with the password of the database user you created earlier.
2. **IMPORTANT** The connection string by default does not include a database name. For each project, you'll add an appropriate database name between the `/?` part of the connection string. For example, if you have a project named "mongoose-movies" you could give the database the same name: `...mongodb.net/mongoose-movies?retryWrites=true...`.

You're good to go!

> NOTE: For future projects, continue to use the same connection string - just be sure to change the database name part of the connection string as mentioned above.
> 

## Connecting with Mongoose

Connecting to your Atlas database is as easy as...

```jsx
mongoose.connect(process.env.DATABASE_URL);
```

We will be covering the above syntax and how to use environmental variables in a future lesson.

## Viewing & Editing Data

FYI, you can use the Atlas app to view and edit data by clicking on the `COLLECTIONS` button.

## Test Drive in the Mongo Shell

Click the **Connect** button again and this time select **Connect with the MongoDB Shell**:

![](https://i.imgur.com/YlBGC8q.png)

1. Install `mongosh` (MongoDB's Shell) for your operating system.
2. Copy and paste the command shown in Terminal. You will be prompted to enter your **database user's** password

Congrats, you are now ready to test the database in the **Intro to MongoDB** lesson!