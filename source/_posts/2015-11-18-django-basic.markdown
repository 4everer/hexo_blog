---
layout: post
title:  "Django Basic"
tags: [python, django, web]
categories: programming
date: 2015-11-18
---


<!-- Django Basics
===== -->

## preparation

``` bash
mkdir project
cd project

virtualenv venv
source venv/bin/activate
```

## create project
```bash
django-admin.py startproject mysite

tree mysite
```

project structure:

```
mysite/
├── mysite
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

## basic commands

`python manage.py <command> [options]`

### create django application (app)

`python manage.py startapp appname`

```
appname
├── __init__.py
├── admin.py
├── migrations
├── models.py
├── tests.py
└── views.py
```

Then edit mysite/settings.py, add entry in **INSTALLED_APPS**

### views and URLconfs

![About MVC](/images/djangoBasic_url-dispatch.png)
