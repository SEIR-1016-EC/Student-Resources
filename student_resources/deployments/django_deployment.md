# Django Deployment

![image](https://media.git.generalassemb.ly/user/21089/files/169f93f0-d47d-424f-8223-dac3ee04ce21)

# Contents

# Heroku Setup

If you haven't already, ensure you have completed the [Getting Started with Heroku](https://www.notion.so/Getting-Started-with-Heroku-b374ae9e006442f6ba4a5d9bc5b6940b?pvs=21) guide.

# Preparation

## `cd` Into the Project’s Folder

- `cd` into the Django project’s root folder
- Open the project in VS Code: `code .`
- Open a terminal in VS Code: `ctrl + backtick`
- Make sure you have checked out into the `main` branch

## Create the App on Heroku

After ensuring that you’re logged in, you can create the app on Heroku as follows:

```bash
heroku create <optional-preferred-name-of-app> --buildpack heroku/python
```

Replace `<optional-preferred-name-of-app>` with the name you want (no spaces). 

If you don’t specify the `<optional-preferred-name-of-app>` argument, Heroku will assign a randomly generated app name automatically.

Keep in mind that there are thousands upon thousands of apps deployed on Heroku, so you may have to get creative when giving your app a name because it has to be unique to Heroku. Using hyphens is one way to help get the app name/URL you want.

For example, this is the command used to create an app named *sei-students*:

```bash
heroku create sei-students --buildpack heroku/python
```

Which will output the below if the chosen name is available.

```
Creating ⬢ sei-students... done
https://sei-students.herokuapp.com/ | https://git.heroku.com/sei-students.git
```

Verify that the command was successful:

```bash
git remote -v
```

You should see a remote name `heroku` listed. If it wasn’t, try `heroku create` again with another preferred name.

The name you choose will be the name of the app in your Heroku dashboard and the name used for the subdomain in the URL of your hosted app, e.g., `https://spotify.herokuapp.com`

## Add a Postgresql database to the project

Heroku has a variety of addons that can be added to the project. For us to add a postgresql database to the application we can run the following command.

```
heroku addons:create heroku-postgresql:hobby-dev
```

Now if you head on over to app in heroku you should be able to see it in the addons section. 

![image](https://media.git.generalassemb.ly/user/21089/files/4d45ea19-19f6-4935-8bd3-a500cd909d62)


# Ready the Django Project

Django projects need to be configured to be deployed.

Django has detailed deployment **[docs](https://docs.djangoproject.com/en/3.1/howto/deployment/)** and a **[checklist](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/)**; however, we will use a dedicated package to make deploying to Heroku much easier.

<aside>
 
⚠️ Note: If you ever deploy a Django app used in production, you’ll want to review the checklist mentioned above and implement the additional security precautions.

</aside>

## Install `django-heroku`

First, let’s install `[django-heroku](https://github.com/heroku/django-heroku)`, which is a Python package that will help with the deployment process:

```bash
pipenv install django-heroku
```

This command may not work. Work with your instructor if it does not. You will probably be told to use one of the following commands depending on the error you receive:

```bash
pipenv install django-on-heroku
```

After running this command you will move onto the **Update `settings.py`** section below.

***Or*** if you are on a ***Linux variant*** you might need to run this command:

```bash
sudo apt-get install --reinstall libpq-dev
```

and follow it again by

```bash
pipenv install django-heroku
```

### Update `settings.py`

There are several changes we would have to make to `**settings.py**` to be able to deploy.

However, the `django-heroku` package makes the necessary changes to `**settings.py**` for us. All we need to do is add the following to the **very bottom** of **`settings.py`**:

```python
# Other settings above

# Configure Django App for Heroku.
import django_heroku
django_heroku.settings(locals())
```

<aside>
 
🚨 Note that the import name is django_heroku instead of django-heroku we used when installing.

</aside>

**If you used the `django-on-heroku` package instead, you'll need to use the following code instead of the above code (at the very bottom of `settings.py`):**

```python
# Other settings above

# Configure Django App for Heroku.
import django_on_heroku
django_on_heroku.settings(locals())
```

## Install `gunicorn`

The built-in development server we’ve been running with `python3 manage.py runserver` is not suitable for deployment.

`gunicorn` is a Python HTTP Server designed to work with Linux/Unix servers such as Heroku’s.

Let’s install it:

```bash
pipenv install gunicorn
```

## Create & Configure `Procfile`

Heroku needs a file named **Procfile** to know how to run a Python app.

Let’s create one - be sure to name it exactly as `**Procfile**` (capitalized and without a file extension):

```bash
touch Procfile
```

We only need to add a single line of code in **`Procfile`**. However, it’s important to replace the `<your-project-name-here>` with your actual project name:

```
web: gunicorn <your-project-name-here>.wsgi
```

```
web: gunicorn spotify_project.wsgi
```

The project name should be the same as your project’s folder name, however, you can also verify the project’s name by examining this line in `**settings.py**`:

```python
WSGI_APPLICATION = 'spotify_project.wsgi.application'
# spotify_project is the project name
```

## Creating a `requirements.txt` file

### Note: These instructions only apply if you are not using a `pipenv` virtual environment `Pipfile`.

The `**package.json**` file we used in Node apps informed Heroku which Node modules the app needed to be installed.

The equivalent in a Python app is the **`requirements.txt`** file.

For out requirements file we will be adding the following requirements 

```
asgiref==3.3.4
autopep8==1.5.7
dj-database-url==0.5.0
Django==3.2
django-on-heroku==1.1.2
gunicorn==20.1.0
psycopg2-binary==2.8.6
pycodestyle==2.7.0
pytz==2021.1
sqlparse==0.4.1
toml==0.10.2
whitenoise==5.2.0
```

<aside>
 
🚨 If you added any additional dependencies to your django app you can get the version number and add it to the list by using the following command: 
`conda list package_name`

</aside>

# Commit the Changes

Now let’s commit the changes made to the project (make sure that you’re on the `main` branch):

```bash
git add .
git commit -m "Config deployment"
```

## Deploy to Heroku

The `heroku` remote was added to the repo with the `heroku create` command ran earlier.

So, deploying the first time and re-deploying later is as easy as running this command:

```bash
git push heroku main
```

The first deployment will take considerably longer than subsequent deployments because Heroku will install all of the Python packages. However, during re-deployments, Heroku will only install/uninstall changes made to `**requirements.txt**`.

Read the output during deployment carefully. You’ll need to address the errors if the deployment fails.

In the case of a successful first deployment - **the app is still not quite ready to run**…

# Migrate the Database Migrations

## Checking that Heroku Created a PostgreSQL Database

If a Django project is configured to use a PostgreSQL, Heroku automatically detects and creates a PostgreSQL database for the project.

You can run the following command to verify this:

```bash
heroku pg
```

You should see an output similar to this:

```
=== DATABASE_URL
Plan:                  Hobby-dev
Status:                Available
Connections:           0/20
PG Version:            11.2
Created:               2019-03-19 16:06 UTC
Data Size:             7.9 MB
Tables:                0
Rows:                  0/10000 (In compliance) - refreshing
Fork/Follow:           Unsupported
Rollback:              Unsupported
Continuous Protection: Off
Add-on:                postgresql-parallel-89032
```

### Check and Migrate the Migrations

Run the command that shows us a list and status of the migrations for our local project:

```bash
python3 manage.py showmigrations
```

The output for the Cat Collector app looks like this:

```
admin
 [X] 0001_initial
 [X] 0002_logentry_remove_auto_add
 [X] 0003_logentry_add_action_flag_choices
auth
 [X] 0001_initial
 [X] 0002_alter_permission_name_max_length
 [X] 0003_alter_user_email_max_length
 [X] 0004_alter_user_username_opts
 [X] 0005_alter_user_last_login_null
 [X] 0006_require_contenttypes_0002
 [X] 0007_alter_validators_add_error_messages
 [X] 0008_alter_user_username_max_length
 [X] 0009_alter_user_last_name_max_length
contenttypes
 [X] 0001_initial
 [X] 0002_remove_content_type_name
main_app
 [X] 0001_initial
 [X] 0002_feeding
 [X] 0003_auto_20190303_2329
 [X] 0004_cat_toys
 [X] 0005_photo
 [X] 0006_cat_user
sessions
 [X] 0001_initial
```

We can run most any command we can locally on the Heroku server by prefacing the command with `heroku run` and substituting `python` for `python3` (Python 3 on Heroku’s servers are configured as `python`).

Let’s check out the migrations for the deployed app:

```bash
heroku run python manage.py showmigrations
```

Which generates the following output for Cat Collector:

```
admin
 [ ] 0001_initial
 [ ] 0002_logentry_remove_auto_add
 [ ] 0003_logentry_add_action_flag_choices
auth
 [ ] 0001_initial
 [ ] 0002_alter_permission_name_max_length
 [ ] 0003_alter_user_email_max_length
 [ ] 0004_alter_user_username_opts
 [ ] 0005_alter_user_last_login_null
 [ ] 0006_require_contenttypes_0002
 [ ] 0007_alter_validators_add_error_messages
 [ ] 0008_alter_user_username_max_length
 [ ] 0009_alter_user_last_name_max_length
contenttypes
 [ ] 0001_initial
 [ ] 0002_remove_content_type_name
main_app
 [ ] 0001_initial
 [ ] 0002_feeding
 [ ] 0003_auto_20190303_2329
 [ ] 0004_cat_toys
 [ ] 0005_photo
 [ ] 0006_cat_user
sessions
 [ ] 0001_initial python
```

Yup, the unchecked migrations tells us that they need to be migrated:

```bash
heroku run python3 manage.py migrate
```

Lots of `OK`s is a good sign!

<aside>
🚨 ***ANYTIME*** you make migrations and migrate locally you'll also have to migrate on your Heroku deployment as well after pushing.

</aside>

# Create the superuser

Because the database is “fresh”, there’s no superuser yet.

```bash
heroku run python3 manage.py createsuperuser
```

It’s the same process as locally, just a bit slower.

# Open the Application

Let’s check it out!

```bash
heroku open
```

Since the database is new, there will not be any users or data. You will need to sign up new test users and fill the database with new demo data.

# Test the Admin Portal

Okay, the finale is to browse to:

`**https://<your-app-name>.herokuapp.com/admin**`

to checkout the admin portal.

Replace `<your-app-name>` with the name of your application

# Troubleshooting

The following command shows Heroku’s log for our app and is useful for troubleshooting. The log also contains the output from your app’s `print()` statements:
