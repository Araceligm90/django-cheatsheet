# DJANGO COMMANDS CHEATSHEET 


### 1. << django-admin startproject django_things_project . >>


    a. 'djando-admin': this command-line utility allows you to perform various administrative tasks related to Django projects.
    b.'django_things_project': is the name of the project that will be created.
    c. '.': at the end of the command represents the destination directory where the project will be created. In this case, the dot (.) indicates the current directory. The project's files and directories will be created within the current directory.

Putting it all together, the command django-admin startproject django_things_project . creates a new Django project 
with the name "django_things_project" in the current directory. The project will be set up with the necessary files and 
directory structure to start developing a Django-based web application.

If you run the command 'tree', you'll be able to see what's inside your directory, which should include a few files, 
including:

-------------
django_things

    a. asgi.py
    b. __init__.py
    c. settings.py --> Represents all of the project's settings.
    d. urls.py --> Contain how the different routes are configured for the endpoints.
    e. wsgi.py

manage.py

-------------


### 2. << python manage.py runserver >>

This command checks if you have unapplied migrations and also will start the development server. 

### 3. << python manage.py migrate >>

To migrate all the unapplied migrations. 

### 4. << python manage.py startapp things >>

Creates a new app. Note that the project is called 'django_things', but then we can name the app whatever we want, in 
this case we are going to call it 'things'. Think about the project as a blueprint and the app are the rooms.

Once we create the application, we need to follow a few more steps to connect it to our project.

    a. We need to manually add the name of our app to our settings.py - INSTALLED_APPS

### 5. Next steps to create your app

**5.1. In views.py --> import and declare the below:**


-------------
        from django.views.generic import TemplateView


        class HomePageView(TemplateView):
            template_name = 'home.html'

-------------

**5.2. And then create a 'urls.py' file within the app directory (things). We will use this file to define our routes. 
Within this file we are going to write the below:**

-------------
        from django.urls import path

        from .views import HomePageView


        urlpatterns = [
            path('', HomePageView.as_view(), name='home'),
        ]

-------------

**5.3. Create a new folder called 'templates' and within it a file called 'base.html'.**

In base.html we are going to create the boilerplate code for an HTML file. Refer to the file for more info.
We are also creating any other html files we would like to be included in our app and extending the 'base.html' file, as
shown in 'home.html'.


**5.4. On settings.py -> TEMPLATES -> 'DIRS' add:**

-------------
         "DIRS": [
            BASE_DIR / 'templates',
        ],

-------------



**5.5. Add the below to 'urls.py'**

-------------
        from django.contrib import admin
        from django.urls import include, path

        urlpatterns = [
            path("admin/", admin.site.urls),
            path('', include('things.urls'))
        ]

-------------


### 6. << python -m pip install django-compressor >>


Django Compressor is a tool that helps optimize and compress the static files in a Django web application, such as CSS 
and JavaScript files. It combines multiple files into a single file, minifies the code, and applies various 
optimizations to improve the performance of the application.


**6.1. Add the compressor to your apps in settings.py.**


**6.2. Add the below lines to the end of your settings.py file.**

-------------
    COMPRESS_ROOT = BASE_DIR / 'static'

    COMPRESS_ENABLED = True

    STATICFILES_FINDERS = ('compressor.finders.CompressorFinder',)

-------------


**6.3. Within your project, create two new folders and one file.** 


    a. Folder called 'static'
    b. Within static, another folder called 'src'
    c. Within src, a file called 'input.css'


**6.4. In your 'base.html' file, add the below tags at the top of the document.**

-------------
    {% load compress %}
    {% load static %}

-------------


**And then add the below within the <head> tag, as shown in 'base.html'.**

-------------
    {% compress css %}
    <link rel="stylesheet" href="{% static 'src/output.css' %}">
    {% endcompress %}
-------------


### 7. Install Tailwind CSS and initialize it.

Run the following command: << npm install -D tailwindcss >>
And then run: << npx tailwindcss init >>

*Make sure that your 'node-modules' folder is included in your gitignore!!*

 
**7.1. In your 'tailwind.config.js' add the below string to 'content':**

-------------
    './templates/**/*.html'
-------------

**7.2. In your 'src' folder, add the below:**

-------------
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
-------------

**7.3. Run the following command: << npx tailwindcss -i ./static/src/input.css -o ./static/src/output.css --watch >>**

The above command creates the output file to the static -> src folders


### 8. Install Flowbite

Run the following command to install it: << npm install flowbite >>


**8.1. And then in your 'tailwind.config.js' file, we need to include the below strings.**

-------------
    content: [
      './templates/**/*.html' --> this was already added before
      './node_modules/flowbite/**/*.js'
    ],

    plugins: [
        require('flowbite/plugin')
    ],
-------------

**8.2. To finish, paste the below script at the end of the <body> tag as shown in 'base.html'.**

-------------
    <script src="https://cdnjs.cloudflare.com/ajax/libs/flowbite/1.6.5/flowbite.min.js"></script>
-------------

**8.3. And don't forget to link the below stylesheet as shown in line 11 in 'base.html'.**

-------------
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flowbite/1.6.5/flowbite.min.css" rel="stylesheet" />
-------------