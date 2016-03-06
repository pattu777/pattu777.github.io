---
layout: post
title: Adding a WYSIWYG Editor to your Django app.
comments: true
---

One of the most useful features of Django is it's admin interface. But the standard plain textarea is really hard to handle. So my first technical blog post will deal with adding a rich HTML editor to an existing django app.

Some of the common django based WYSIWYG editor apps are listed below.

1. django-ckeditor
2. django-summernote
3. django-tinymce
4. django-wysiwyg
5. django-wysiwyg-redactor

In this post I will install and configure django-ckeditor.

Step 1:
Install django-ckeditor app using pip.

```python
pip install django-ckeditor
```

Step 2:
Add ckeditor to <strong>INSTALLED_APPS</strong> in settings.py file.

Step 3:
Add the following lines to your settings file.

```javascript
CKEDITOR_UPLOAD_PATH = "uploads/"
CKEDITOR_JQUERY_URL = '//ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js'
```

Step 4:
Update your project's urls.py file with the below line.

```python
url(r'^ckeditor/', include('ckeditor.urls')),
```

Step 5:
Modify your models.py file and change your model fields.

```python
from ckeditor.fields import RichTextField
class BlogPost(models.Model):
    author = models.CharFIeld(max_length=30)
    title = models.CharField(max_length=50)
    description = RichTextField()

    def __unicode__(self):
        return self.title
```

Step 6:
Now run the following django commands.

```
python manage.py makemigrations
python manage.py migrate
python manage.py collectstatic
python manage.py runserver
```

Done. Now open up your browser and go to the link 127.0.0.1:8000/admin. Log in to the admin account using your superuser name and password.

Finally try to create a new post using the newly added WYSIWYG editor.
