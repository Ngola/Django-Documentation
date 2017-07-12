## The Admin dashboard
- [Understanding the admin dashboard](#understanding-the-admin-dashboard)
- [Creating an admin superuser](#creating-an-admin-supersuser)
- [Configuring the admin dashboard](#configuring-the-admin-dashboard)
- [Recent actions](#recent-actions)
- [Customizing The Admin Dashboard](#customizing-the-admin-dashboard)
- [Review Chapter 3](#review-chapter-3)


## 3. The Admin Dashboard
### Understanding the admin dashboard
In chapter 2, I mentioned the **admin dashboard** several times, but I did not explain what it is and how we can use it in the context of our App. So, what is the **admin dashboard**?

If you have used a Content Management System (CMS), like Wordpress, for example, you may have benefited from a somewhat automated admin dashboard experience. From the dashboard you should be able to, say, create blog posts, add images to a gallery, manage store inventory, or add new users to the site. In a way, the dashboard allows you to generate new content for your website without having to write more code. You could simply interface with buttons, drag and drop things into place, and save and deploy changes with the click of a button.

With Django, we are provided with an admin dashboard. By defaults, the admin dashboard can be accessed via `localhost:8000/admin` (if you are running your server on a different port, just be sure to change the port number to the one you have chosen).

When navigating to the admin url, you'll be prompted to provide a username and a password. We'll be creating those credentials in the next section.

### Creating an admin superuser
In order to create a username and a password to use with our admin dashboard, we'll need to run another command with `manage.py`. This time it will be:

> ```python manage.py createsuperuser```

Provide the following information:
- username (required)
- email address (optional)
- password (required)

If you had stopped the server, you can restart it now.

> ```python manage.py runserver```

Let's return to the browser and navigate to the site.

> ```localhost:8000/admin or 127.0.0.1:8000/admin```

Now you can log in with the username and password you used to create the new user.


### Configuring the admin dashboard

Now we'll turn to the file called `admin.py` inside of our **App** called `Blog_App`. The file should have the following two lines already added to it:

```
from django.contrib import admin

# Register your models here.
```

Right above the comment on line 3, let's import our models from the `Blog_App`. Like so:

```
from django.contrib import admin
from Blog_App.models import *
# Register your models here.
```

Now we can simply register our models to the admin dashboard using the a built-in method of the admin site class. Like so:

```
admin.site.register(Article)
admin.site.register(Author)
```

So far, the file `admin.py` should look as follows (I added some comments):

```
# importing the admin module
# importing all the modules
from django.contrib import admin
from Blog_App.models import *

# Register your models here.    
admin.site.register(Article)
admin.site.register(Author)
```


`Blog_App` should now be displayed on our admin dashboard.
You should also see `Articles` and `Authors` on the admin dashboard. Note that our models show in their plural form. This is done automatically by Django. We can change that later if needed.

Next to `Articles` and `Authors` you'll be presented with the option to `add` or `modify` either. Let's first `add` an `Author`. Click the corresponding option and you'll be presented with a page with the fields specified in the class declaration of your model. In the bottom right corner you'll see multiple buttons to save your new instance of `Author`. Follow the same steps to add another author. You call also add another `Article` using a similar approach.

**NOTE**: If you are familiar with the concept of instantiating objects, then what we do with the help of the admin dashboard is essentially that.


### 3.4. Recent Actions
Once a new instance of an `Article` or `Author` is added, it should be referred to as **Article object** or **Author object**. This is not very helpful if you want to refer to your objects by name. The admin dashboard has a sidebar section (to the right of the screen) which features the actions recently taken. Similar to this:

```
  Recent actions
  ---------------------
  My actions

  Article object
  Article
  Author object
  Author
```

Since you may want to know the exact name of the object and not a generic name, we can change the `Article` and the `Author` classes to output the desired information. This is how our models are defined so far:

**Author:**
```
class Author(models.Model):
    name = models.CharField(max_length=100)
    job  = models.CharField(max_length=100)
```

**Article:**
```
class Article(models.Model):
    title   = models.CharField(max_length=100)
    content = models.TextField()
    date    = models.DateField(auto_now=True)
```

Add the following function to the `Author` model, after **job**, which is the last property/attribute:

```
def __str__(self):
    return self.name
```

Add the following function to the `Article` model, after **date**, which is the last property/attribute:

```
def __str__(self):
    return self.title
```

Let's take `Article`, for example. Let's suppose one of the articles we create has '**Hiking The Kilimanjaro**' as it's title. Now we'll be able to see the title of the article under **Recent Actions**. i.e.

```
  Recent actions
  ---------------------
  My actions

  Hiking The Kilimanjaro
  Article
  Leonardo Ribeiro
  Author
```

**NOTE:** The function we just added to our models will also be helpful later when we deal with our HTML pages.


### Customizing The Admin Dashboard

Let's configure our admin dashboard a little further. On the main admin dashboard page, the one you get to from `localhost:8000/admin` (or something similar if you're using a different domain) you will see `Blog_App` and immediately under the App you will see both models: `Article` and `Author`. Click on `Article` to get a list of articles in a nicely formatted table. Click on `Author` to get a list of authors.
The tables you just saw can be customized to some extent. We will customize the `Article` table to only display the **title** and the **date**. To do so, let's open `admin.py`. Inside this file let's add the following code:

```
# Import all(*) models from the Blog_App
# Ignore this import statement if the models have already been added...
from Blog_App.models import *

class ArticleAdmin(admin.ModelAdmin):
  # Sort the articles by title.
  ordering = ['title']

  # Properties to be displayed on the table.
  list_display = ['title', 'date']

  # Number of articles per page.
  list_per_page = 20

# Register the model class and the admin model to the admin dashboard.
admin.site.register(Article, ArticleAdmin)
```

### Review Chapter 3
- Add another class to the `admin.py` to manage the `Author` model.
- Sort the articles in reverse order. If in ascending order, switch the order to descending.
- In your own words, what is the admin dashboard?


