## Django App Models
- [Understanding Models inside an application](#understanding-models-inside-an-application)
- [Understanding Model Fields](#Understanding-model-fields)
- [Understanding Model Migrations](#understanding-model-migrations)
- [Review Chapter 2](#review-chapter-2)




## 2. Django App Models
### Understanding Models inside an application
In section 1.4. we created our Blog_App. Now we are going to create our models inside our application. Models are essentially python classes that inherit from Django's models object. These models can be defined as blueprints of the objects you will want tostore in the database.

So, in our Blog_App, we are going to create two models: `Author` and `Article`. We will later use the admin dashboard to create our objects as needed.

Inside Blog_App, look for `models.py`. The file should have one line of code and a line with comments. Like so:

```
from django.db import models
# Create your models here.
```

We will declare our `Author` class under the comment:

```
class Author(models.Model):
    name = models.CharField(max_length=100)
    job  = models.CharField(max_length=100)
```

Now we'll also declare our `Article` class:

```
class Article(models.Model):
    title   = models.CharField(max_length=100)
    content = models.TextField()
    date    = models.DateField(auto_now=True)
```

Now that we have our two classes, `Author` and `Article`, we can discuss how a models is stored in our sqlite3 database.

When we migrate our changes to the database (we'll see migrations later), each model will be stored as a table. Each new instance of a given model will be a new row in the table. The fields will be rows in that table.

```
Author:
id       |name            |job
1         Nunes Ribeiro    Journalist
2         Paula Oliver     Engineer
```

### Understanding Model Fields
Now we'll talk a little about the fields in our models. The type of a field is similar in concept to the type of a variable. So, an `IntegerField` is should only contain integer values. The same way, a `BooleanField` can only contain either **True** or **False** for values.

In our App we will mostly use the following fields:
- `models.CharField`: holds any string and requires a max_length attribute.
- `models.TextField`: also holds a string, but the length is unbounded.
- `models.EmailField`: used for emails. This field has input validation.
- `models.URLField`: used for URLs. This field has input validation.
- `models.FileField`: used for storing files. Validation occurs.
- `models.ImageField`: inherits from FileField and used validation to check for image file.
- `models.BooleanField`: regular Python field for storing True or False values (works well in conditional statements).
- `models.DateField`: uses Python's date object.
- `models.DateTimeField`: stores date, like DateField, but also stores time.

Declaring the properties of a model as fields will allow us to input the desired value types later when we create new instance objects through the admin dashboard. In the admin dashboard, our HTML page will present us with **input fields** for our fields. For BooleanFields, the HTML page will present us with simple and intuitive **checkboxes** (checked=True).

As we've already seen with `models.CharField`, fields can be configured with the use of some provided options. Here's just a few commonly used field options:
- `blank`: allows for a string to be left empty in the corresponding field in the database.
- `default`: sets a default value. This value will be used if the field is not provided with any other value.
- `choices`: limits values stored in a field to those specified.

Here's an example:

```
SIZE_LIST = (
              ('S', 'Small'),
              ('M', 'Medium'),
              ('L', 'Large'),
            )
shirt_size = models.CharField(max_length=1, choices=SIZE_LIST, default='M')
```

### Understanding Model Migrations
Migrations are instructions to the database concerning your models. Every time a new property is added or a field changed, a migration should be made and applied to the database. This ensures that the information in our database reflects that of our abstract classes. So, in short, migrations should be made so that changes to a model are applied to the database.

Let's revisit our table for the Author model from the example in section 2.1,

```
Author:
id       |name            |job
1         Nunes Ribeiro    Journalist
2         Paula Oliver     Engineer
```

As we can see, the names of the columns match those of the properties of the `Author` class. Let's suppose I'd like to change the variable `name` to `Author_Name`. This change, at first, may not seem very significant, but conflicts with the database may arise when we decide to create a new object of type `Author` in our **admin dashboard**. The admin dashboard is aware of the change that happened to the name of the variable called `name`. However, the database is not aware of the change. So, when we try to write to the database the column called `Author_Name`, Django will throw an error.

To mitigate that, we need to make migrations and then apply them to the database.

NOTE: To ensure that everything works as expected, stop your server, if it is running, and then perform the desired migrations. Stop the server by using the key-combination `CONTROL-C` (control, NOT command).

Like so:

> `python manage.py makemigrations`

Here's a brief look at the migration instructions:

```
Migrations for 'Blog_App':
  Blog_App/migrations/0001_initial.py
    - Create model Article
    - Create model Author
```

Now we apply the migrations like so:

> `python manage.py migrate`

Here's a brief look at the instructions being sent to the database. These are our migrations:

```
Operations to perform:
  Apply all migrations: Blog_App, admin, auth, contenttypes, sessions
Running migrations:
  Applying Blog_App.0001_initial... OK
```

Now, our database contains our models, `Author` and `Article`, and it also contains information regarding the fields that our models require.

### Review Chapter 2
- Why should we care about migrations?
- How do our models relate to our database?
- Create a new model inside models.py with fields we have not used in either model.
