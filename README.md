# I Think Therefore I Blog

## Agile UX
[Agile Project](https://github.com/FlashDrag/agile-project)

### Project Planning
- #### Design Thinking
    - **Empathize**.
    Put yourself in the shoes of the user. What are their needs? What are their pain points? What are their goals?
    - **Questions**.
    Why do you want to build this app? What problem does it solve? What value does it provide? What will make users return to your app?
    - **Examine**.
    What do I want to seee when I visit a blog? What features do I want to see? What features do I not want to see?

### User Stories
- #### GitHub's Kanban board
    _GitHub's Kanban board was used to track user stories_

    [Creating Boards with Github Instructions Sheet](https://docs.google.com/document/d/1VSJtnhh_8djRyncotRzMJG9g_ATfiBHasUVZ1xTwybE/edit#heading=h.hvy9tw74f1o0)

- Create new project board
- Open **Workflows** from the <**...**> menu at the top-right of the board.
- Enable the **Item added to project** workflow by toggling it on. _This enables the automation of moving issues to the **To Do** column when they are added to the project board._
- Link the project board to the repository.
- Create a user stories template:
    - Open the repository's settings.
    - Scroll down to the **Features** section.
    - Click **Set up templates**.
    - Click **Add custom template**:

        | name | about | title | labels | assignees |
        | :------: | :------: | :------: | :------: | :------: |
        | User Story | Issue template for user stories | USER STORY: TITLE | story | @FlashDrag |

        **Description**:

        ```As a **role** I can **capability** so that **received benefit**```

    - Commit the changes (**Propose changes**):
    `Add a new user story template`

- #### User Stories/GitHub Issues
    _Create user stories as GitHub issues using the user stories template and add the issue to the project board._

    - View post list: As a Site User I can view a paginated list of posts so that I can easily select a post to view
    - Open a post: As a Site User I can click on a post so that I can read the full text
    - View likes: As a Site User / Admin I can view the number of likes on each post so that I can see which is the most popular or viral
    - View comments: As a Site User / Admin I can view comments on an individual post so that I can read the conversation
    - Account registration: As a Site User I can register an account so that I can comment and like
    - Comment on a post: As a Site User I can leave comments on a post so that I can be involved in the conversation
    - Like / Unlike: As a Site User I can like or unlike a post so that I can interact with the content
    - Manage posts: As a Site Admin I can create, read, update and delete posts so that I can manage my blog content
    - Create drafts: As a Site Admin I can create draft posts so that I can finish writing the content later
    - Approve comments: As a Site Admin I can approve or disapprove comments so that I can filter out objectionable comments

### Entity Relationship Diagram(ERD)
- #### POST MODEL
    **Name**: Post

    **Fields**:
    - id: Primary Key, Auto Increment
    - title: CharField, unique, max length 200
    - [slug](https://nemecek.be/blog/96/django-admin-tip-auto-generated-slug-content): SlugField, Unique, max length 200
    - author: One to many relationship(Foreign Key) with User Model
    - created date: DateTimeField
    - updated date: DateTimeField
    - content: TextField
    - image: CloudinaryField
    - excerpt: TextField
    - likes: Many to many relationship with User Model
    - status: Integer, Choices (Draft, Published)

    **Options**: descending order by created date

- #### COMMENT MODEL
    **Name**: Comment

    **Fields**:
    - post: One to many relationship(Foreign Key) with Post Model
    - name: CharField, max length 80
    - email: EmailField
    - body: TextField
    - created_on: DateTimeField
    - approved: BooleanField

    **Options**: ascending order by created date




## Technologies Used
- [Django](https://www.djangoproject.com/)
- [Bootstrap](https://getbootstrap.com/)
- [PostgreSQL (ElephantSQL)](https://www.elephantsql.com/)
- [Cloudinary](https://cloudinary.com/)
- [Heroku](https://www.heroku.com/)

### Django Packages
- [django-summernote](https://github.com/summernote/django-summernote) - WYSIWYG (full featured) editor for Django posts.

- [django-crispy-forms](https://django-crispy-forms.readthedocs.io/en/latest/)

## Project Setup
- ### Virtual Environment
#### Set Up Virtualenv with Virtualenvwrapper on Ubuntu 22.04
1. Create a virtual environment:
    ```
    $ cd <path to project>
    $ mkvirtualenv -a <path to project> <name of virtual environment>
    # e.g.: $ mkvirtualenv -a . django-blog-env
    ```

    (flag `-a` means to associate the virtual environment with a project)

2. Activate the virtual environment:
    ```
    $ workon <name of virtual environment>
    ```

- ### Django Setup
[Hello Django Instructions Sheet](https://docs.google.com/document/d/113P2BPOkrG6rMxsrm8GCbO6CVG2b1R9htD4tRdkSltQ/edit#heading=h.hvy9tw74f1o0)
1. Install Django:
    ```
    $ pip install 'django<4'
    ```
2. Create a Django project:
    ```
    $ django-admin startproject <name of project> .
    ```

    _Note: The dot (.) at the end of the command means to create the project in the current directory._

3. Configuring Environment Variables:
    - Install `python-dotenv`:
        ```
        $ pip install python-dotenv
        ```
    - Import the `load_dotenv()` function in the `<settings.py>` file:
        ```
        from dotenv import load_dotenv
        load_dotenv()
        ```
    - Create environment variable `<.env>` for sensitive information and add it to the `<.gitignore>` file:
        ```
        $ touch .env .gitignore
        $ echo .env >> .gitignore
        ```
    - Set the environment variables in the `<.env>` file:
        See the `<.env_example>` file.
    - Import the environment variables in the `<settings.py>` file:
        ```
        import os

        development = os.getenv('DEVELOPMENT', False)
        DEBUG = os.getenv('DEBUG', False)
        SECRET_KEY = os.getenv('SECRET_KEY')
        ```
4. Run the Django server:
    - Install `livereload` extension for the browser to refresh the browser automatically:

        LiveReload extension for Chrome: https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=en
        ```
        $ pip install livereload
        ```
    - Run servers and enable LiveReload extension in the browser:
        ```
        $ python manage.py runserver
        $ livereload
        ```

5. Create a Django app:

    _Allows Django to recognize the app and look for a template folder in the app_

    ```
    $ python manage.py startapp <name of app>
    ```

    - Add the app to the `INSTALLED_APPS` list in `settings.py`:

        ```
        TEMPLATES = [
            {
            ...
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            ...
            },
        ]

        INSTALLED_APPS = [
            ...
            '<name of app>',
        ]
        ```

    - Add the app to the `urls.py` file of the project:

    _This will allow the app to have its own `urls.py` file and registers any URLs defined there. And the app's `urls.py` file will be used to define the all routes for the certain app_

        ```
        from django.urls import path, include

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('', include('<name of app>.urls')),
        ]
        ```

    - Create a `urls.py` file in the app:

        ```
        from django.urls import path
        from . import views

        urlpatterns = [
            path('url_path/', views.<name of view function>, name='<name of view function>'),
        ]
        ```

6. Create a Django superuser (_only after the db is set up and migrations are run_):

    ```
    $ python manage.py createsuperuser
    ```

    _Allows you to access the admin UI panel and manage the database_

- ### Database Setup

- Install `psycopg2` database adapter to use PostgreSQL with Django.
- Install `dj-database-url` to use PostgreSQL with Django (_dj-database-url parses the database URL from the environment variable DATABASE_URL and configures the DATABASES setting accordingly_).

    ```
    $ pip install psycopg2 dj_database_url
    ```

- Update the DATABASES setting in `settings.py`:

        ```
        development = os.getenv('DEVELOPMENT', False)
        DEBUG = os.getenv('DEBUG', False)

        if development:
            DATABASES = {
                'default': {
                    'ENGINE': 'django.db.backends.sqlite3',
                    'NAME': BASE_DIR / 'db.sqlite3',
                }
            }
        else:
            # Parse database configuration from $DATABASE_URL
            import dj_database_url
            DATABASES = {
                'default': dj_database_url.config(
                    default=os.getenv('DATABASE_URL')
                )
            }
        ```

#### Local Migrations
1. Make migrations:

The `makemigrations` command looks at the models you have defined in your apps and creates a set of migration files for those changes.

    ```
    $ python manage.py makemigrations
    ```

2. Migrate:

The `migrate` command takes all of the migration files and runs them against your database - synchronizing the changes you made to your models with the schema in the database.

    ```
    $ python manage.py migrate
    ```

- Add db.sqlite3 to the .gitignore file:
    ```
    $ echo db.sqlite3 >> .gitignore
    ```

- Show migrations:

The `showmigrations` command shows all migrations that have been applied or unapplied.

    ```
    $ python manage.py showmigrations
    ```

### Django Packages
- #### Django Summernote
It's a simple WYSIWYG editor for Django. It allows you to edit the content in a text area and format text content as you like.

    [**Integrating Summernote In Django**:](https://djangocentral.com/integrating-summernote-in-django/)

    ```
    $ pip install django-summernote
    ```

    - Add `django_summernote` to the `INSTALLED_APPS` list in `settings.py`:

        ```
        INSTALLED_APPS = [
            ...
            'django_summernote',
        ]

        SUMMERNOTE_THEME = 'bs4'  # Show summernote with Bootstrap4
        ```

    - Add the following to the `urls.py` file of the project:

        ```
        from django.urls import path, include

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('summernote/', include('django_summernote.urls')),
        ]
        ```
    - Run migrations:

        ```
        $ python manage.py migrate
        ```
    - Add the following to the `models.py` file of the app:

        ```
        from django.db import models
        from django_summernote.fields import SummernoteTextField

        class Post(models.Model):
            ...
            content = SummernoteTextField()
        ```
        _This will allow you to use the Summernote editor in the app_

    - Add the following to the `admin.py` file of the app:

        ```
        from django.contrib import admin
        from .models import Post
        from django_summernote.admin import SummernoteModelAdmin

        class PostAdmin(SummernoteModelAdmin):
            summernote_fields = ('content',)
        admin.site.register(Post, PostAdmin)
        ```
        _This will allow you to use the Summernote editor in the admin panel_

    [**USAGE**:](https://github.com/summernote/django-summernote#usage)

    - To use the Summernote editor in the admin panel, you need to add the following to `admin.py` file of the app:

        ```
        from django.contrib import admin
        from .models import Post
        from django_summernote.admin import SummernoteModelAdmin

        @admin.register(Post)
        class PostAdmin(SummernoteModelAdmin):
            summernote_fields = ('content',)
        ```
    - To use the Summernote editor in the app, you need to add the following to `forms.py` file of the app:

        ```
        from django import forms
        from .models import Post
        from django_summernote.widgets import SummernoteWidget

        class PostForm(forms.ModelForm):
            class Meta:
                model = Post
                fields = '__all__'
                widgets = {
                    'content': SummernoteWidget(),
                }
        ```
        _This will allow you to use the Summernote editor in the app_

- #### Django Crispy Forms


## Deployment
1. Database: [ElephantSQL](https://www.elephantsql.com/)
2. Static File Hosting: [Cloudinary](https://cloudinary.com/)
3. Deployment: [Heroku](https://www.heroku.com/)

### ElephantSQL Setup
- Create an ElephantSQL account.
- Create a new database instance.
- Copy the database URL from the ElephantSQL dashboard.
- Add the database URL to the `.env` file.

### Cloudinary Setup
Storage backend for Django static files and media uploads.
https://pypi.org/project/django-cloudinary-storage/

- Create a Cloudinary account.
- For Primary interest, you can choose Programmable Media for image and video APIs.
- Copy the API Environment Variable from the Cloudinary dashboard.
- Add the API Environment Variable to the `.env` file.
- Install cloudinary to upload images to the cloud:
    ```
    $ pip install dj3-cloudinary-storage
    ```

    _`dj3-cloudinary-storage` package is a fork of the django-cloudinary-storage package, specifically adapted for Django 3.x compatibility._

- **Configure `staticfiles` and `media` storage with Cloudinary**:
    - Add the following to the `settings.py` file:
        ```
        # Cloudinary settings
        INSTALLED_APPS = [
            # ...
            'cloudinary_storage',  # Must be above `django.contrib.staticfiles`, to override default static storage
            'django.contrib.staticfiles',
            'cloudinary',
            # ...
        ]

        # STATIC FILES(CSS, JS, IMAGES)

        # Credentials for the cloudinary_storage. Can be found in the Cloudinary dashboard
        CLOUDINARY_STORAGE = {
            'CLOUDINARY_URL': os.getenv('CLOUDINARY_URL'),
        }
        # OR
        CLOUDINARY_STORAGE = {
            'CLOUD_NAME': os.getenv('CLOUDINARY_CLOUD_NAME'),
            'API_KEY': os.getenv('CLOUDINARY_API_KEY'),
            'API_SECRET': os.getenv('CLOUDINARY_API_SECRET'),
        }

        # URL path for your static files.
        # This is the URL path where your static files will be served from.
        # Example path for cloudinary css file: `https://res.cloudinary.com/<cloud_name>/raw/upload/v1/static/django_blog/css/style.css'
        # Dir where your static files are stored locally during development. This is where collectstatic will collect static files from.
        STATICFILES_DIRS = [os.path.join(BASE_DIR, "static"), ]

        # Dir where static files are stored during production, after running collectstatic
        STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

        # Cloudinary's storage for static files with hashed names
        STATICFILES_STORAGE = 'cloudinary_storage.storage.StaticHashedCloudinaryStorage'

        # URL path for media files
        MEDIA_URL = '/media/django_blog/'
        # Media cloudinary storage
        DEFAULT_FILE_STORAGE = 'cloudinary_storage.storage.MediaCloudinaryStorage'
        ```
    - Create a `media` and `static` and in the root directory of the project or in the app directories. The `media` folder will contain all media files uploaded by users, and the `static` folder will contain all static files such as CSS, JS, and images.
    - Create a `base.html` file in the `templates` folder and add the following:
        ```
        {% load static %}
        ```
    - Deploy the app on HEROKU or run the following command to collect static files if the command didn't run automatically:

        ```
        $ python manage.py collectstatic
        ```

    Usually the `$ python manage.py collectstatic` command runs automatically when deploying on HEROKU.

    If you don't use static files, you can prevent this command from running automatically during deployment on Heroku, by setting `DISABLE_COLLECTSTATIC` to `1` in the Heroku config vars. If you already run this command manually, you don't have to disable it on HEROKU, as it won't upload the files again if they didn't change.
        ```
        $ heroku config:set DISABLE_COLLECTSTATIC=1
        ```

    **Notes:**
    - In production, you must set `DEBUG` to `False` to fetch static files from Cloudinary. With `DEBUG` equal to `True`, Django `staticfiles` app will use your local files for easier and faster development (unless you use `cloudinary_static` template tag).


### Heroku CLI deployment instructions
[Django-Heroku settings.py example](https://github.com/heroku/python-getting-started/blob/main/gettingstarted/settings.py)

- Install gunicorn to replace the Django development server:
    ```
    $ pip install gunicorn
    ```
- Create requirements file:
    ```
    $ pip freeze > requirements.txt
    ```
- Create a Heroku Procfile:
    ```
    $ touch Procfile
    ```
- Create a Heroku Procfile:
    ```
    $ echo web: gunicorn <name_of_project>.wsgi:application > Procfile
    # e.g: `web: gunicorn django_todo.wsgi:application`
    ```
- Login to Heroku:
    ```
    $ heroku login
    ```
- Create a Heroku app:
    ```
    $ heroku create <name of app> --region eu
    ```
- Set the environment variables in Heroku:
    ```
    $ heroku config:set <name of variable>=<value of variable>
    ```
- Add the Heroku app URL to the ALLOWED_HOSTS list in `settings.py`:
    ```
    ALLOWED_HOSTS = ['<name of app>.herokuapp.com', '127.0.0.1']
    ```
- Commit and push the code to Heroku or **GitHub and deploy from there**:
    ```
    $ git add .
    $ git commit -m "Setup Heroku files for deployment"
    $ git push heroku master
    ```
- Run the migrations on Heroku:
    ```
    $ heroku run python manage.py migrate
    ```