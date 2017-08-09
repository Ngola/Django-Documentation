## Views, Regex, and URL Patterns
- [Understanding views inside an application](#understanding-views-inside-an-application)
- [Returning HTML pages from views](#returning-html-pages-from-views)
- [Links and Regular Expressions](#links-and-regular-expressions)
- [Django HTML Syntax](#django-html-syntax)
- [Review Chapter 4](#review-chapter-4)


## 4. Views, Regex, and URL Patterns
### Understanding views inside an application
In short, a **view** is a **Python function** that handles what html page templates are rendered. Views can also handle redirects and other types of HTTP responses. Further reading about HTTP Requests and Responses is advised for better understanding of how views work.

We can define our ```views``` in the file ```views.py``` inside our **App**.

```
├── Blog_App
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py <---------- this file
```

Let's create our first view with the following code:


```
# import this module if not already present in the file...
from django.http import HttpResponse

# this is our view. It receives 'request' as an argument
# this view simply returns the word 'Homepage' as an H1 HTML element.
def home(request):
    return HttpResponse('<h1>Homepage</h1>')
```

Our view is defined to take `request` as an argument. This is a non-optional argument. This `request` that we must pass to our view is an `HTTP request`. It has information such as user authentication, HTTP method, session cookie, page (or path) requested... and more. You can read more about the HTTP request and response objects [in the django documentation](#https://docs.djangoproject.com/en/1.11/ref/request-response/).

We now need to link the view we just created with a url so that any requests to that url will trigger our view. We will link our `home` view with our base url so that when we visit our site at `localhost:8000` or `127.0.0.1:8000` (with or without a slash appended at the end) we may see the message `Homepage` that we defined as the return of our view.


Inside PROJECT/PROJECT, open urls.py. Note the path to the file:

```
PROJECT/
...
├── PROJECT
│   ├── __init__.py
│   ├── __init__.pyc
│   ├── settings.py
│   ├── settings.pyc
│   ├── urls.py <--- this file
│   ├── urls.pyc
│   ├── wsgi.py
│   └── wsgi.pyc
├── db.sqlite3
└── manage.py
```


Inside `urls.py`, on line 18, import our `Blog_App` views. Like so:

```
# importing our Blog_App views as calling those views Blog
from Blog_App import views as Blog
```
If you have multiple Apps in your project, you can import their views in a similar way. Just remember to assign each imported view a different name to avoid confusion and unexpected behavior.

After importing the views from `Blog_App`, let's define the url pattern that should call our `home` view. Add this line to the `urlpatterns` tuple: `url(r'^$', Blog.home, name='homepage'),`. Like so:


```
urlpatterns = [
    url(r'^$', Blog.home, name='homepage'),
    url(r'^admin/', admin.site.urls),
]
```

Let's take a look at the website. Go to `localhost:8000` or `127.0.0.1:8000`, where the Django server is running your site to see if the view is outputting the message specified in the `home` view.

You should now see the word `Homepage` on the page. This means the configuration was successful.

---

### Returning HTML pages from views

Now let's update our view in `Blog_App/views.py` to return the page `index.html`. Note the changes to the `home` view. We will start by importing our models from the `Blog_App`:


```
from django.shortcuts import render
from django.http import HttpResponse

# importing the models from our app...
from Blog_App.models import *
```

Now let's modify the `home` view:

```
def home(request):
    articles_from_database = Article.objects.all()
    return render(request, 'index.html', {'articles': articles_from_database,})
````

Let's now create an `index.html` file inside a new `Templates` folder. The `Templates` folder will live in the main level of our PROJECT directory. Like so:


```
├── Blog_App
│   ...
├── PROJECT
│   ...
├── Templates
│   └── index.html
├── db.sqlite3
└── manage.py
```

From the main PROJECT directory, run the following commands to create the `Templates` folder, as well a the `index.html` file:

```
mkdir Templates
touch Templates/index.html
```

We'll now need to register our `Templates` directory in `settings.py`. The directory should be added to `DIRS: []`:

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'Templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

Inside `index.html`, type the following HTML:


```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Django PROJECT</title>
  </head>
  <body>

    <h1>Homepage</h1>

    {% for article in articles %}
      <p>{{article.title}}</p>
    {% endfor %}

  </body>
</html>
```

The HTML inside `index.html` also includes a `for-loop` that will output the title of our articles:


```
{% for article in articles %}
  <p>{{article.title}}</p>
{% enfor %}
```

The variable `article` inside the `for-loop` is a new variable that changes value as the `for-loop` iterates through all the `articles`. The variable `articles`, however, is the one being returned from our `home` view.

Recall:

```
def home(request):
    articles_from_database = Article.objects.all()
    return render(request, 'index.html', {'articles': articles_from_database,})
```

With this in place, we can access the homepage of our website and see all the articles we decide to create through the admin dashboard. If you haven't created any articles yet, your homepage will only show the word **Homepage**, but you can go ahead and create a new article from the admin dashboard as then refresh the page. The title of the newly added article should now be displayed according to the sintax in `index.html`.

---

### Links and Regular Expressions

Now that our articles are being displayed on the homepage, let's link the titles of these articles to a page where we can see more information about the same articles. Basically, we want to be able to click on one article and go to a page where we only see information about that article.

Let's create a new file. Let's call it 'article.html'. Let's place it inside of our 'Templates' folder, next to our `index.html` page.

```
├── Templates
│   └── index.html
│   └── article.html

```

Write the following in `article.html`:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Article: {{article.title}}</title>
  </head>
  <body>

    <h1>{{article}}</h1>

    <p>{{article.date}}<p>

    <p>{{article.content}}</p>

  </body>
</html>
```

Note that this time we don't have a `for-loop` to determine the information being output on the page. This is because we are formatting this page to only output information about one specific article, the one that the viewer of our site requests through a specific URL. For this, we are going to need to add a new URL pattern to `urls.py`. We are going to use RegEx, or regular expressions to match a specific type of pattern in our URL requests. Please [read more about regular expressions](#https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference) as they can be very useful.

Inside `urls.py` let's add the new patterns to our tuple:

```
url(r'^(?P<id>\d+)', Blog.article, name='article_link'),
```

All three URL patterns should look like this:

```
urlpatterns = [
    url(r'^$', Blog.home, name='homepage'),
    url(r'^(?P<id>\d+)', Blog.article, name='article_link'),
    url(r'^admin/', admin.site.urls),
]
```

Now, let's add a new view to our `views.py` file. The view should be called article, just like in our URL pattern.

The view will be responsible for returning one article at a time. `request` and `id` are the two arguments that we will pass to the function. `id` is the variable defined in the URL pattern that calls this view.

Since the article we will be requesting through a URL with a given `id` may or may not exist, we must use a  `try` and `except` block (similar to Java's `try` and `catch`) to catch errors. If the article does not exist, Django provides us with the handy module called '`Http404`, which we can use to display a `404` error on the page. A `404` errors are typically used when a requested resource does not exist or cannot be found.

```
def article(request, id):
    try:
        article_from_database = Article.objects.get(id=id)
    except Article.DoesNotExist:
        raise Http404('Article does not exist.')
    return render(request, 'article.html', {
    'article': article_from_database,
    })
```

Since we made use of `Http404`, we must also import it to our `views.py` file. Import it with the `HttpResponse` module, which we have already imported:

```
from django.http import HttpResponse, Http404
```

Now go to `localhost:8000/1` or `127.0.0.1:8000/1` to see the first article you created.

This is now a good time to add links to the `index.html` page so that we can access our articles by clicking on their title's. We will only change the `for-loop` section. Like so:

```
{% for article in articles %}
  <a href="{% url 'article_link' article.id %}">
    <p>{{article.title}}</p>
  </a>
{% endfor %}
```

Let's break this sintax down:

`{% url 'article_link' article.id %}`

We are making use of the built-in `url` rounting capabilities in Django, which allows us to dynamically add links to our page.

The name `article_link` refers to the name of the url pattern in `urls.py`:

`url(r'^(?P<id>\d+)', Blog.article, name='article_link')`.

`article.id` refers to the article in the `for-loop` and it's own id in the database.

If we change the RegEx that takes us to our articles, our links on the homepage will still work, so long as the name of the url pattern remains consistent in our HTML pages.

---

### Django HTML Syntax

The Django syntax inside of our HTML pages is quite simple to understand.

Let's start by understanding when and how to use the lovely pairs of curly braces `{{ }}`. These are used when we want to output the value of a variable.

For example, when we wanted the output the title of a post inside `index.html` we used `{{ article.title }}`. [Read more](https://docs.djangoproject.com/en/1.11/ref/templates/language/#variables).

When we want to write some logical statements along with our HTML, we can use a pair of percent signs inside a pair of curly braces, like so: `{% %}`.

For example, when we wanted to loop through our articles being returned from one of our views we used `{% for article in articles %}`. And because this is a `for-loop`, we need to determine where the code block must end with another statement `{% endfor %}`. The same applies to `if-statements`, which start with, for example, `{% if x == 0 %}`, and must end with `{% endif %}`.


Django also has some built-in filters that we can apply to our variables through the use of the `|` (pipe) operator. [Read more](https://docs.djangoproject.com/en/1.11/ref/templates/builtins/).

For example, we can try `{{ article.title | capfirst }}`. This will output the title of the article with the first letter of each word capitatized.

TIP: a handy built-in feature with Django is the function `now`. It returns an aware or naive datetime.datetime, depending on settings.USE_TZ. [Read more](https://docs.djangoproject.com/en/1.11/_modules/django/utils/timezone/#now).
My favorite use of `now` is to display the current year next to a copyright notice in the footer of my website pages. As with anything, your own mileage may vary.

Just as an example, this is how I output the year using `now`:

```
{% now 'Y' %}
```

Simple as that.

---
### Review Chapter 4
- What is a view?
- What should a view return?
- How do our HTML pages and our views relate in the context of our application?
- Create a url pattern (using RegEx) that instead of matching the id of an article, matches the title of the article.
- Create a view to go along with the url pattern specified above.
- Review the view and url pattern we created for for articles, create a similar view for your authors.
- Add a for-loop to the homepage to display the names of the authors.
