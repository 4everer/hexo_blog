---
layout: post
title:  "Django Blog Tutorial"
tags: [python, django, web]
categories: programming
date: 2015-11-15
---

Django Blog Tutorial - the Next Generation - Part 1
====

[original link](http://matthewdaly.co.uk/blog/2013/12/28/django-blog-tutorial-the-next-generation-part-1/)

*edited with new django version*
[github](https://github.com/4everer/django_test)

This series will cover exactly the same basic idea of using Django to build a blogging engine, but will expand on what the original series did in many ways. We will cover such additional topics as:

- Using Twitter Bootstrap to make your blog look good without too much hassle
- Using Virtualenv to sandbox your blog application
- ~~Using South to effectively manage changes to your database structure~~
    **use `python manage.py migrate` and `python manage.py makemigrations`**
- Writing some simple unit tests
- Deploying the finished application to Heroku

Ready? Let’s get started!

\*unix system
python 2.7

**prerequests**:

- Virtualenv
- Pip
- Git
- psql

## Beginning work
With all that done, we’re ready to get started. Create a folder in a suitable place on your file system and switch into it. I generally keep a dedicated folder in my home directory called Projects to use for all of my projects, and give each project a folder within it - in this case the project is called django_tutorial_blog_ng.

Now, we’ll use Git to keep track of our source code. If you prefer Mercurial, feel free to use that, but this tutorial will assume use of Git, so you’ll want to adapt the commands used accordingly. Start tracking your project with the following command from the shell, when you’re in the project directory:

``` bash
$ git init
$ virtualenv venv --distribute
$ source venv/bin/activate
$ pip install django-toolbelt
```
Once the installation finish,
`$ pip freeze > requirements.txt`

Next, we’ll commit these changes with Git:

```bash
$ git add requirements.txt
$ git commit -m 'Committed requirements'
```

Next we’ll add a .gitignore file to ignore our virtualenv - we want to keep this out of version control because it’s something specific to that install. We have all we need to recreate it so we don’t want to store it. In addition, we also want to ignore any compiled Python files (identifiable by the .pyc suffix):
add and edit .gitignore file
>
venv/
*.pyc


Let’s commit that too:

``` bash
$ git add .gitignore
$ git commit -m 'Added a gitignore file'
```

Now, let’s generate our project’s basic skeleton:
```bash
$ django-admin.py startproject django_tutorial_blog_ng .
```

This application skeleton includes a basic configuration which will be sufficient for now, but you will also want to add the SQLite database file to your .gitignore:
>
env/
*.pyc
db.sqlite3

Let’s commit what we’ve done:
```bash
$ git add .gitignore django_tutorial_blog_ng/manage.py
$ git commit -m 'Created project skeleton'
```

Now, you can create your database. Run the following command:

```bash
$ python manage.py migrate
$ python manage.py syncdb
```

You’ll be prompted to create a superuser - go ahead and fill in the details. Now, run the following command:

```bash
$ python manage.py runserver
```

This will run Django’s built-in web server on port 8000, and if you click [here](127.0.0.1:8000), you should see a page congratulating you on your first Django-powered page. Once you’re finished with it, you can stop the web server with Ctrl-C.


## Your first app
Django distinguishes between the concepts of projects and apps. A project is a specific project that may consist of one or more apps, such as a web app, whereas an app is a set of functionality within a project. For instance, one website might include some flat pages, an admin interface, and a blogging engine, and these could easily be different apps. By encouraging you to separate different types of functionality into different apps, Django makes it easier for you to reuse existing content elsewhere.

We’re going to create our first app, which is the blogging engine. Run the following command to create a basic skeleton for this app:

```bash
$ python manage.py startapp blogengine
```

## An introduction to MVC
![About MVC](/images/djangoBasic_url-dispatch.png)

MVC is a common pattern used in web development. Many web development frameworks can be loosely described as MVC, including Django, Rails, CodeIgniter, Laravel and Symfony, as well as some client-side frameworks like Backbone.js. The basic concept is that a web app is divided into three basic components:

- Models: the data managed with the application
- Views: the presentation of the data
- Controllers: an intermediary between the models and the views

Now, Django’s interpretation of MVC is slightly different to many other frameworks. While in most frameworks the views are HTML templates for rendering the data, in Django this role is taken by the templates, and the views are functions or objects that render data from the models using a template. Effectively, you can think of Django’s views as being like controllers in other frameworks, and Django templates as being views.

In Django, you create your models as Python classes that represent your data, and you use the Django ORM to query the database. As a result, it’s rare to have to directly query your database using SQL, making it more portable between different databases.

Now, our first model is going to be of a blog post. At least initially, each post will have the following attributes:
>
A title
A publication date and time
Some text

Now, we could just jump straight into creating our first model, but we’re going to make a point of following the practices of test-driven development here. The basic concept of TDD is that you write a failing test before writing any code, then you write the code to pass that test afterwards. It does make things a bit slower, but it’s all too easy to neglect writing tests at all if you leave it till later.

If you take a look in the **blogengine** folder you’ll notice there’s a file called **tests.py**.

It’s worth taking a little time to plan out what we want to test from our post model. Each post object will have the attributes I mentioned above, and what we want to be able to do is test that we can:

- Set the title
- Set the publication date and time
- Set the text
- Save it successfully
- Retrieve it successfully

``` python
# test.py
from django.test import TestCase
from django.utils import timezone
from blogengine.models import Post
# Create your tests here.
class PostTest(TestCase):
    def test_create_post(self):
        # Create the post
        post = Post()
        # Set the attributes
        post.title = 'My first post'
        post.text = 'This is my first blog post'
        post.pub_date = timezone.now()
        # Save it
        post.save()
        # Check we can find it
        all_posts = Post.objects.all()
        self.assertEquals(len(all_posts), 1)
        only_post = all_posts[0]
        self.assertEquals(only_post, post)
        # Check attributes
        self.assertEquals(only_post.title, 'My first post')
        self.assertEquals(only_post.text, 'This is my first blog post')
        self.assertEquals(only_post.pub_date.day, post.pub_date.day)
        self.assertEquals(only_post.pub_date.month, post.pub_date.month)
        self.assertEquals(only_post.pub_date.year, post.pub_date.year)
        self.assertEquals(only_post.pub_date.hour, post.pub_date.hour)
        self.assertEquals(only_post.pub_date.minute, post.pub_date.minute)
        self.assertEquals(only_post.pub_date.second, post.pub_date.second)
```

then edit the **model.py**

```python
from django.db import models
# Create your models here.


class Post(models.Model):
    title = models.CharField(max_length=200)
    pub_date = models.DateTimeField()
    text = models.TextField()
```

first do the migraitons for db
```bash
python manage.py makemigrations
python manage.py migrate
```

run the test
`python managy.py test`

we can use
`python manage.py sqlmigrate blogengine 0001`
to see the migrations

*The `sqlmigrate` command takes migration names and returns their SQL*

run `python manage.py check` to check before making any migrations

## Creating blog posts via the admin

Now, we need a way to be able to create, edit and delete blog posts. Django’s admin interface allows us to do so easily. However, before we do so, we want to create automated acceptance tests for this functionality, in order to test the ability to create posts from an end-user’s perspective. While unit tests are for testing sections of an application’s functionality from the perspective of other sections of the application, acceptance tests are testing from the user’s perspective. In other words, they test what the application needs to do to be acceptable.

First, we will test logging into the admin.

Now, you could put your own credentials in there, but that’s not a good idea because it’s a security risk. Instead, we’ll create a fixture for the test user that will be loaded when the tests are run. Run the following command:
```bash
$ python manage.py createsuperuser
```

Give the username as bobsmith, the email address as bob@example.com, and the password as password. Once that’s done, run these commands to dump the existing users to a fixture:
```bash
$ mkdir blogengine/fixtures
$ python manage.py dumpdata auth.User --indent=2 > blogengine/fixtures/users.json
```

This will dump all of the existing users to blogengine/fixtures/users.json. You may wish to edit this file to remove your own superuser account and leave only the newly created one in there.

First of all, we import two new objects from `django.test`, `LiveServerTestCase` and `Client`. Then we create the first part of our first test for the admin, named `AdminTest`. Eventually, this will test that we can log successfully into the admin interface. For now, we’re just doing the following:

    - Creating a Client object
    - Fetching the /admin/ route
    - Asserting that the status code for this HTTP request is 200, (in other words, that the page was fetched successfully).

``` python
from django.test import TestCase, LiveServerTestCase, Client
from django.utils import timezone
from blogengine.models import Post
# Create your tests here.


class PostTest(TestCase):
    def test_create_post(self):
        # Create the post
        post = Post()
        # Set the attributes
        post.title = 'My first post'
        post.text = 'This is my first blog post'
        post.pub_date = timezone.now()
        # Save it
        post.save()
        # Check we can find it
        all_posts = Post.objects.all()
        self.assertEquals(len(all_posts), 1)
        only_post = all_posts[0]
        self.assertEquals(only_post, post)
        # Check attributes
        self.assertEquals(only_post.title, 'My first post')
        self.assertEquals(only_post.text, 'This is my first blog post')
        self.assertEquals(only_post.pub_date.day, post.pub_date.day)
        self.assertEquals(only_post.pub_date.month, post.pub_date.month)
        self.assertEquals(only_post.pub_date.year, post.pub_date.year)
        self.assertEquals(only_post.pub_date.hour, post.pub_date.hour)
        self.assertEquals(only_post.pub_date.minute, post.pub_date.minute)
        self.assertEquals(only_post.pub_date.second, post.pub_date.second)


class AdminTest(LiveServerTestCase):
    fixtures = ['users.json']
    def test_login(self):
        # Create client
        c = Client()
        # Get login page
        response = c.get('/admin/')
        # Check response code
        self.assertEquals(response.status_code, 200)
        # Check 'Log in' in response
        self.assertTrue('Log in' in response.content)
        # Log the user in
        c.login(username='bobsmith', password="password")
        # Check response code
        response = c.get('/admin/')
        self.assertEquals(response.status_code, 200)
        # Check 'Log out' in response
        self.assertTrue('Log out' in response.content)
```

Now, if you run `python manage.py test`, you should find that the test passes. Next, we’ll test that we can log out:

```python
def test_logout(self):
        # Create client
        c = Client()
        # Log in
        c.login(username='bobsmith', password="password")
        # Check response code
        response = c.get('/admin/')
        self.assertEquals(response.status_code, 200)
        # Check 'Log out' in response
        self.assertTrue('Log out' in response.content)
        # Log out
        c.logout()
        # Check response code
        response = c.get('/admin/')
        self.assertEquals(response.status_code, 200)
        # Check 'Log in' in response
        self.assertTrue('Log in' in response.content)
```

