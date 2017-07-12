#### 1. Getting Started
- [Installing pip](#installing-pip)
- [Installing Django](#installing-django)
- [Creating a Django project](#creating-a-django-project)
- [Understanding your project](#understanding-your-project)
- [Creating an application inside our project](#creating-an-application-inside-our-project)
- [Understanding the parts of an application](#understanding-the-parts-of-an-application)
- [Review Chapter 1](#review-chapter-1)


## 1. Getting Started
### Installing Pip
NOTE: This instructions are for users running some version of MacOS (or OSX).\
In order to install Django on your system we first need to check if we already have `pip` (Python's Package Installer) present in our system.\
To check if `pip` is in your system run the following command in the terminal:

> ```pip --version```

If you have `pip` in your system, you will be able to see a message similar to the following:

> ```pip 9.0.1 from /Library/Python/2.7/site-packages (python 2.7)```

If this is your case, you're all set. You can move on to [the next section](#installing-django)
If this is not your case, you'll need to install `pip` by downloading the `get-pip.py` source code from the following URL (you can do this from your preferred browser):

> ```https://bootstrap.pypa.io/get-pip.py```

After you've downloaded the source code, on your terminal, navigate to the folder where `get-pip.py` was downloaded. Now run the following command:

> ```python get-pip.py```


### Installing Django
Once the installation of `pip` is complete (or after you've certified that you have `pip` in your system), on your terminal, run the following command to install Django, version 1.11:

> ```pip install django==1.11```

### Creating a Django project
In order to create our first Django project we need to determine where it is going to be located. Once that has been determined, you should also think about what name you will give to your project. In this example our project will be called, you guessed it, PROJECT.

So, to create our project called PROJECT we will go back to our terminal and run the following command:

> ```django-admin startproject PROJECT```

Once your project has been created, you should have a folder in your system called PROJECT. Take your time and get acquainted with the contents of this folder.

Here's a rough view of your project's directory:

```
PROJECT
  manage.py
  PROJECT
    __init__.py
    settings.py
    urls.py
    wsgi.py
```
As you can see, a file called `manage.py`was created inside project directory. We are going to use this file a lot, as it allows us to manage our project and applications inside it.

If you're interested in knowing what `manage.py` can do, run the following command to list all the available options (run the command from within your project's directory):

> ```python manage.py```

One of the options for `manage.py` is `runserver`. This allows us to run a local server from where we can access our project using a web browser. Start a server with the following command:

> ```python manage.py runserver```

The output on my terminal is the following:


```
Performing system checks...

System check identified no issues (0 silenced).

You have 13 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

June 28, 2017 - 16:52:30
Django version 1.11, using settings 'PROJECT.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```


By default, the project server will run your website on `localhost`, port `8000`. To access the site just navigate to `localhost:8000` on a web browser. Some systems have `127.0.0.1` as an alias for localhost. You can use that too. And if you do, you can also use that followed by the port number (i.e `127.0.0.1:8000`).

Now go to your browser at one of the above addresses and check your welcome page. It should say something like this:

```
It worked!
Congratulations on your first Django-powered page.

Next, start your first app by running python manage.py startapp [app_label].

You're seeing this message because you have DEBUG = True in your Django settings file and you haven't configured any URLs. Get to work!
```

The `debug` language for your Django project is `en-us` (English US), by default.
 This language can be changed in `settings.py`. Go to `settings.py` and look for the variable called `LANGUAGE_CODE`. Change its value to one that is relevant to you. For example, `it` (Italiano), `de-AT` (Deutsche), `es` (Español), `pt-BR` (Português).

Note that on your welcome message, you have been instructed to create a new django app. We will be creating our first app soon. But we first need to understand our project a bit better.

### Understanding your project
Remember when we ran the command: `django-admin startproject PROJECT`. This created our Django project.

Think of our project as a container. The Django project should contain our applications, our configurations (settings), our templates, our css, our javascript..., etc. As you see, the project directory is where your website will live. Here's a rough look at how a project directory may look like (folders are marked with a forward `slash /`):

```
/PROJECT
  db.sqlite3
  manage.py
  /PROJECT_Static
    /css
      master.css
    /js
      master.js
    /img
      favicon.ico
      logo.jpg
  /PROJECT_Media
  /PROJECT
    __init__.py
    settings.py
    urls.py
    wsgi.py
  /Blog_App
    /migrations
    __init__.py
    admin.py
    apps.py
    models.py
    tests.py
    views.py
  /Templates
    404.html
    master.html
    index.html
    /Blog_Templates
      blog.html
      article.html
```

We will go over what those folders and files mean soon. It is important that you familiarize yourself with the structure of your project so you know your way around the multiple files you may need to edit.

In the upcoming section we'll be creating our first Django app so we can get more functionality out of our project.

### Creating an application inside our project
An application, from now on referred to simply as **App**, is a specific feature within your project. For example, a `Blog`, a `Forum`, a `Wiki`, a `Store`.

For this example you'll create a blog **App**. To do so, navigate to your main project directory (where `manage.py` is located). Run the command:

> python manage.py startapp Blog_App

Your project directory should now look like this:

```
/PROJECT
  manage.py
  /Blog_App
    /migrations
    __init__.py
    admin.py
    apps.py
    models.py
    tests.py
    views.py
  /PROJECT
    __init__.py
    settings.py
    urls.py
    wsgi.py
```

Now we need to let Django know that we plan to use this new **App** in our project.
To do so, we need to add the name of our **App** to the `INSTALLED_APPS` tuple in `settings.py`. Like so:

```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'Blog_App',
]
```

As you can now see, your App, Blog_App, was created in your project directory and has been populated with the required files for it to work properly. We'll be editing some of those files so we can get our application running soon.

### Understanding the parts of an application
Django uses a `Model-View-Template` architecture. To better understand what MVT means and how it differs from the common MVC pattern, refer to the [official documentation](https://docs.djangoproject.com/en/1.11/faq/general/#django-appears-to-be-a-mvc-framework-but-you-call-the-controller-the-view-and-the-view-the-template-how-come-you-don-t-use-the-standard-names).

An **App** has the following parts:
- `models.py`, class (object) declarations.
- `admin.py`, administrative interface for that app (admin dashboard).
- `views.py`, functions that return HTTP responses and render the HTML pages.
- `tests.py`, (we don't need to worry about this for now).
- `/migrations`, migrations are instructions to the database about changes in the App models. This is a directory, not a file.


### Review Chapter 1
- What is pip?
- How are the project and the application related?
- Change the debug language of your application. Test the results in your browser.
- Create a new application inside your project. Make it relevant to your project. Add at least one model to the admin dashboard from this new application.
________________________
