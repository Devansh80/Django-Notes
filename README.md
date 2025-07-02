# Django Framework - Beginner's Guide

## What is Django?
Django is a Python web framework that helps you build web applications quickly. It follows the "Don't Repeat Yourself" (DRY) principle and handles many common web development tasks automatically.

## Key Concept: MVC vs MVT
- **Traditional MVC**: Model-View-Controller
- **Django MVT**: Model-View-Template
  - **Model**: Database structure (data)
  - **View**: Business logic (what happens)
  - **Template**: HTML presentation (what users see)

## Installation & Setup
```bash
pip install django
django-admin startproject mysite
cd mysite
python manage.py runserver
```

## Django Project Structure
```
mysite/                    # Project root
├── manage.py              # Command-line utility
├── mysite/               # Project package
│   ├── __init__.py
│   ├── settings.py       # Project configuration
│   ├── urls.py          # Main URL routing
│   ├── wsgi.py          # Web server gateway
│   └── asgi.py          # Async server gateway
└── db.sqlite3           # Default database (created after first migration)
```

## Creating an App
```bash
python manage.py startapp blog
```

### App Structure
```
blog/                     # App directory
├── __init__.py
├── admin.py             # Admin interface config
├── apps.py              # App configuration
├── models.py            # Database models
├── views.py             # View functions
├── urls.py              # App URL patterns (create this)
├── tests.py             # Test cases
└── migrations/          # Database migration files
    └── __init__.py
```

## Core Components

### 1. Models (models.py)
Define your database structure using Python classes.

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return self.title
```

**Common Field Types:**
- `CharField()` - Short text
- `TextField()` - Long text
- `IntegerField()` - Numbers
- `DateTimeField()` - Date and time
- `BooleanField()` - True/False
- `ForeignKey()` - Relationship to another model

### 2. Views (views.py)
Handle requests and return responses.

```python
from django.shortcuts import render
from django.http import HttpResponse
from .models import Post

# Function-based view
def home(request):
    posts = Post.objects.all()
    return render(request, 'blog/home.html', {'posts': posts})

# Simple response
def about(request):
    return HttpResponse("About page")
```

### 3. Templates
HTML files with Django template language for dynamic content.

```html
<!-- blog/templates/blog/home.html -->
<!DOCTYPE html>
<html>
<head>
    <title>My Blog</title>
</head>
<body>
    <h1>Blog Posts</h1>
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
        <small>{{ post.created_at }}</small>
    {% endfor %}
</body>
</html>
```

### 4. URLs (urls.py)
Map URLs to views.

```python
# blog/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('about/', views.about, name='about'),
]

# mysite/urls.py (main)
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

## Essential Django Commands

### Database Operations
```bash
python manage.py makemigrations    # Create migration files
python manage.py migrate           # Apply migrations to database
python manage.py sqlmigrate blog 0001  # View SQL for migration
```

### Development
```bash
python manage.py runserver         # Start development server
python manage.py runserver 8080    # Run on specific port
python manage.py shell            # Interactive Python shell
```

### Admin & Users
```bash
python manage.py createsuperuser   # Create admin user
python manage.py collectstatic     # Collect static files
```

## Settings.py Key Configurations

```python
# Database configuration
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Installed apps (add your apps here)
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',  # Your app
]

# Static files (CSS, JavaScript, Images)
STATIC_URL = '/static/'
```

## Django Admin
Register models to manage them via web interface.

```python
# blog/admin.py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

## Template System Basics

### Template Tags
- `{% %}` - Logic (loops, conditions)
- `{{ }}` - Variables
- `{# #}` - Comments

### Common Template Tags
```html
{% if user.is_authenticated %}
    <p>Welcome, {{ user.username }}!</p>
{% else %}
    <p>Please log in</p>
{% endif %}

{% for item in items %}
    <p>{{ item.name }}</p>
{% empty %}
    <p>No items found</p>
{% endfor %}
```

## Request-Response Flow
1. User requests a URL
2. Django checks `urls.py` patterns
3. Matches URL to a view function
4. View processes request, interacts with models
5. View renders template with data
6. Django returns HTTP response to user

## Best Practices for Beginners
1. **One app per functionality** - Keep apps focused and small
2. **Use meaningful names** - For models, views, and templates
3. **Follow Django conventions** - File naming and structure
4. **Always migrate after model changes** - Keep database in sync
5. **Use Django admin for testing** - Quick way to add/edit data

## Quick Start Checklist
1. ✅ Install Django
2. ✅ Create project (`startproject`)
3. ✅ Create app (`startapp`)
4. ✅ Add app to `INSTALLED_APPS`
5. ✅ Create models
6. ✅ Make and run migrations
7. ✅ Create views
8. ✅ Set up URLs
9. ✅ Create templates
10. ✅ Test with `runserver`

## Common File Locations
- **Templates**: `app_name/templates/app_name/`
- **Static files**: `app_name/static/app_name/`
- **Media files**: Configured in `settings.py`

This guide covers the fundamental concepts you need to start building with Django. Focus on understanding the MVT pattern and how URLs, views, models, and templates work together.
