django-getting-started
======================

Step by step guide to getting started with Django development on OS X

This is my preferred setup, developed over my years as a programmer. There are many options, which 
I will omit here for the sake of simplicity, but feel free to modify to suit your needs.

Notes:

1. You will need to use the command line for parts of this guide. If you are using OSX, you can use Terminal. On Linux, you can open a shell of your choosing. Windows development is harder than OSX/Linux, but you can probably get by with the cmd prompt.
1. Any line that is preceded by a '$' means that is a command to be typed on the command line.
1. If you've completed a given step in the past, you can skip that step and go to the next.
1. Any value that is in angle brackets and capitalized (e.g. <SOME_VALUE>) is a placeholder and must
be replaced by an actual value.


# One-Time Setup

You will need to do this once on each computer you want to set up for Django development.


## Development Environment Setup

1. Install XCode from the Apple website.
> XCode download: https://developer.apple.com/xcode/downloads/
>
> Detailed steps available here: http://railsapps.github.io/xcode-command-line-tools.html

1. Install the command line tools. This allows you to compile packages from source.
> Open Terminal app (I recommend adding it to your dock, as you will need it often)
>
> At the prompt, enter "gcc":
>
>     $ gcc
>
> If you don't already have the command line tools, you will be prompted to install them.

1. Install homebrew. This gives you easy access to useful system packages. Up-to-date install
instructions can be found on the homebrew site: http://brew.sh/
> In Terminal:
>
> $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

1. Familiarize yourself with a command line editor. Emacs and Vim are popular choices. If you find
this too daunting, you can use the open command.
> Emacs basics: http://mally.stanford.edu/~sr/computing/emacs.html
>
> Vim basics: http://vim.wikia.com/wiki/Tutorial

1. Ensure that commands from homebrew packages are found before system commands. You can do this by
editing your .bashrc file. In Terminal, enter the following:
> $ cd ~
>
> $ open .bashrc  # Could also use vim or emacs
>
> If /usr/local/sbin and /usr/local/bin are not already on your path, add them near the top of the file:
>
>     export PATH=/usr/local/sbin:/usr/local/bin:$PATH
>
> Save and exit

1. Source your changes to .bashrc into the current shell. In Terminal, enter the following:

    $ . ~/.bashrc

1. Verify that homebrew is installed correctly. In Terminal, enter the following:

    $ brew doctor

1. Install git using homebrew. In Terminal, enter the following:

    $ brew install git

1. Make a directory to hold all your programming projects. I call mine "Development" and I put it
in my home directory.
    
1. Sign up for a GitHub account: https://github.com/join

1. Set up your GitHub SSH key so you don't have to type username and password on the command line every time: https://help.github.com/articles/generating-ssh-keys/

1. Select a code editor. I like PyCharm but it does require a paid license. Other popular 
options are [Sublime text](http://www.sublimetext.com/2), [textmate](https://macromates.com/),
 [Emacs](http://mally.stanford.edu/~sr/computing/emacs.html), and [Vim](http://vim.wikia.com/wiki/Tutorial).
 
 1. Configure your text editor to use spaces instead of tabs. The standard in the Python community is to replace each tab character with 4 spaces.


## Python Environment Setup

1. Install python 3 using homebrew. This is the latest version of python, and while much existing
code is still written in python 2, for new projects it is best to use python 3. In Terminal, enter
the following:
    $ brew install python3

1. Verify the correct python3 is now on your path. In Terminal, enter the following:
    $ which python3  # Should be "/usr/local/bin/python3"

1. Pip in the standard package manager for Python. Packages provide useful functionality that is not available in Python itself. Ensure that you have the latest pip version installed:
    $ pip install -U pip

1. Install virtualenv and virtualenvwrapper for python3. The virtualenv package allows you to have individual
environments for every python project, separate from the system package installs. The 
virtualenvwrapper package is a utility to manage your virtualenvs. It keeps the virtualenvs separate
from your code and also provides useful operations on virtualenvs.
(To be investigated: Do virtualenv and virtualenvwrapper have to be installed globally?)
    $ sudo pip install virtualenv virtualenvwrapper

1. Add virtualenvwrapper configuration to your .bashrc file. You can do this by
editing your .bashrc file. In Terminal, enter the following:
    $ cd ~

    $ open .bashrc  # Could also use vim or emacs

    If they are not already present, add the following 3 lines:

        export WORKON_HOME=$HOME/.virtualenvs
        export PROJECT_HOME=$HOME/Development
        source /usr/local/bin/virtualenvwrapper.sh

    Save and exit


# Django Project Setup

You will need to do this once for each new Django project you create.

## Create Django Project and add to GitHub

1. Log in to GitHub and create a new repository.

    1. Click the '+' in the upper right and select "New repository"
    1. Enter repository name ("my_project") and description ("My Django project")
    1. Select "Initialize this repository with a README"
    1. Under .gitignore select "Python"
    1. Under license select "MIT License"
    1. Click the "Create repository" button
    1. On the right-hand side, select the value in the HTTPS clone URL box and copy it

1. Clone your project. Open Terminal, and do the following:

    $ cd ~/Development
    
    $ git clone ```<VALUE_FROM_GITHUB>```  # Paste in the value you copied above

1. In Terminal, go into the cloned project directory:

    $ cd my_project

1. Make a virtualenv using python3:

    $ mkvirtualenv my_project --python=python3  # Create a python3 virtualenv

1. Install django-toolbelt, which includes Django and useful packages for
deploying a Django project to Heroku. In terminal:

    $ pip install django-toolbelt

1. Check the list of installed packages using pip freeze. It should look something like the following. The first part of the line is the package name, the '==' is the version specifier (in this case, identically equal to), and the number to the right is the version that is installed.

    $ pip freeze
    
		dj-database-url==0.3.0
		dj-static==0.0.6
		Django==1.8.4
		django-toolbelt==0.0.1
		gunicorn==19.3.0
		psycopg2==2.6.1
		static3==0.6.1
		wheel==0.24.0

1. Explanation of installed packages:

		dj-database-url - Required by Heroku to access the postgres database addon
		dj-static - Wrapper around static3 to allow Heroku to serve static files (e.g. CSS, JavaScript)
		Django - The main Django package
		django-toolbelt - The top-level package we installed to get all these packages
		gunicorn - Webserver recommended by Heroku for serving Django apps (similar to 'manage.py runserver', which is used for testing)
		psycopg2 - Python package for accessing postgres databases
		static3 - Python 3 version of static, which allows your static files to be served by WSGI
		wheel - Used by pip, installed by default

1. The standard way of tracking package requirements for a Django project is a requirements.txt file. Whenever a fresh checkout of this project is made, all necessary dependencies can be installed by typing `pip install -r requirements.txt` in the Terminal. This is how Heroku will install your project dependencies. Use pip freeze to add the installed packages to requirements.txt

		$ pip freeze > requirements.txt

1. Create a new Django project in the top-level git project directory:

		$ django-admin startproject my_project . # Note the trailing '.' which means 'current directory'

1. Check that you have expected changes:

		$ git status -u
		On branch master
		Your branch is up-to-date with 'origin/master'.
		Untracked files:
			(use "git add <file>..." to include in what will be committed)
		
			manage.py
			my_project/__init__.py
			my_project/settings.py
			my_project/urls.py
			my_project/wsgi.py
			requirements.txt
		
		nothing added to commit but untracked files present (use "git add" to track)
		
1. Verify that your project runs (always a good idea before committing code!).

    1. In Terminal, run your Django app:
        
        $ python manage.py runserver
    
    1. In a browser, open http://127.0.0.1:8000 where you should see "It worked!"
    
1. Edit your .gitignore file to include an entry for .sql files. By default, Django will use sqlite3 (a basic database that is bundled with Python) and you don't want to commit your database file to your repository! Open .gitignore and add the following at the end of the file:

		*.sql*
    
1. Commit and push your changes.

		$ git add -A  # Add all modified files to staging
		$ git commit -m "Initialized Django project"  # Commit staged filed
		$ git push origin master  # Push commit to github

1. Go to your GitHub project in the browser and you should see your changes.


## Add Functionality to Django App

Now comes the fun part: making your Django app do something interesting!

1. Ensure you are set up to start working. You will want to do this each time you start a working. Open Terminal, and do the following:

		$ cd ~/Development/my_project
		$ workon my_project

### Creating a New App

In Django, an app is a cohesive collection of functionality inside a project. An example of an app might be user account management. Ideally, pps should be self-contained so that an app from one Django project can be reused in another Django project. You will need to follow these instructions each time you want to add a new aspect to your project.

1. To create an app:

		$ python manage.py startapp app1
		
1. You will need to add your new app to my_project/settings.py. This lets Django know about your app, e.g. for creating/running migrations.

		INSTALLED_APPS = (
				...
    		'app1',
		)

1. Create a urls.py file for your new app. Inside the app1 directory, create urls.py and enter the following:		
		
		from django.conf.urls import url
		
		from . import views  # Import the views for this app
		
		
		urlpatterns = [
		]
		
1. Create a templates directory with app-specific subdirectory. Inside the app1 directory, create a new directory named templates, and within that directory, create a directory named app1. It may seem strange to have app1 with the app1/templates directory, but it is important so that templates from different apps do not conflict with each other. Your directory structure should now look like the following:

		my_project/
		    app1/
		        migrations/
		        templates/
		            app1/
		        admin.py
		        ....
		        urls.py
		        views.py
		    my_project/
		        settings.py
		        ...

1. Edit my_project/urls.py and include the urls for your new app. Once you are done, urls.py should look like this:

		from django.conf.urls import include, url
		from django.contrib import admin
		
		from app1 import views as app1_views  # Import for app1 views
		
		urlpatterns = [
				url(r'^admin/', include(admin.site.urls)),
				url(r'^app1/', include('app1.urls')),  # For all requests to /app1, look at urls in app1.urls
		]
		
### Creating a new view

You will need to follow these instructions for every new view you create. Apps typically have multiple views.

1. Create a template for your new view, called index.html, inside the app1/templates/app1 directory. Django templates can do variable substitution, looping, and all sorts of other useful operations, but for now, we will make a very simple "Hello World" template containing plain HTML. Edit index.html to contain the following:

		<!DOCTYPE html>
		<html lang="en">
		<head>
		</head>
		<body>
			<h3>Hello World!</h3>
			<p>I just made the index page for my first Django app. :-)</p>
		</body>
		</html>

1. Create the view code for the "Hello World" view. This is an extremely simple view that takes a request and renders the app1/index.html template. (Note that we do not need to say prepend "templates" to the path -- Django knows to look at templates.) Edit app1/views.py to contain the following:

		from django.shortcuts import render
		
		
		def index(request):
				context = {}  # So far, we are not specifying any custom context
				return render(request, 'app1/index.html', context)

1. Add your new view to the app urls. Edit app1/urls.py to add a url for your new view. Once you are done, the file should look like the following

		from django.conf.urls import url
		
		from . import views  # Import the views for this app
		
		
		urlpatterns = [
				url(r'^$', views.index, name='index'),  # Url for index view
		]
		
1. Verify that your new view is available by navigating to the new app in the browser: http://127.0.0.1:8000/app1/ (Note that you need the /app1 because your specified in the top-level urls.py that all urls for app1 would be available at /app1)

1. If your code is working, commit and push your changes.

		$ git status -u  # Check what files have been modified
		$ git diff  # Look over your changes (always a good idea to ensure nothing odd snuck in there)
		$ git add -A  # Add all modified files to staging
		$ git commit -m "Added first view"  # Commit staged filed
		$ git push origin master  # Push commit to github


# Resources

http://tutorial.djangogirls.org/en/index.html

https://docs.djangoproject.com/en/1.8/intro/tutorial01/

https://devcenter.heroku.com/articles/getting-started-with-django#prerequisites
