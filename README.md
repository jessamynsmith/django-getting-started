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
> Open the command line. For OSX, this means opening Terminal app (I recommend adding it to your dock, as you will need it often)
>
> At the prompt, enter "gcc":
>
>     $ gcc
>
> If you don't already have the command line tools, you will be prompted to install them.

1. Install homebrew. This gives you easy access to useful system packages. Up-to-date install
instructions can be found on the homebrew site: http://brew.sh/
> On the command line:
>
> $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

1. Familiarize yourself with a command line editor. Emacs and Vim are popular choices. If you find
this too daunting, you can use the open command.
> Emacs basics: http://mally.stanford.edu/~sr/computing/emacs.html
>
> Vim basics: http://vim.wikia.com/wiki/Tutorial

1. Ensure that commands from homebrew packages are found before system commands. You can do this by
editing your .bashrc file. On the command line, enter the following:
> $ cd ~
>
> $ open .bashrc  # Could also use vim or emacs
>
> If /usr/local/sbin and /usr/local/bin are not already on your path, add them near the top of the file:
>
>     export PATH=/usr/local/sbin:/usr/local/bin:$PATH
>
> Save and exit

1. Source your changes to .bashrc into the current shell. On the command line, enter the following:

    $ . ~/.bashrc

1. Verify that homebrew is installed correctly. On the command line, enter the following:

    $ brew doctor

1. Install git using homebrew. On the command line, enter the following:

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
code is still written in python 2, for new projects it is best to use python 3. On the command line, enter
the following:
    $ brew install python3

1. Verify the correct python3 is now on your path. On the command line, enter the following:
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
editing your .bashrc file. On the command line, enter the following:
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

1. Clone your project. On the command line, do the following:

    $ cd ~/Development
    
    $ git clone ```<VALUE_FROM_GITHUB>```  # Paste in the value you copied above

1. On the command line, go into the cloned project directory:

    $ cd my_project

1. Make a virtualenv using python3:

    $ mkvirtualenv my_project --python=python3  # Create a python3 virtualenv

1. Install django-toolbelt, which includes Django and useful packages for
deploying a Django project to Heroku. On the command line:

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

1. The standard way of tracking package requirements for a Django project is a requirements.txt file. Whenever a fresh checkout of this project is made, all necessary dependencies can be installed by typing `pip install -r requirements.txt` on the command line. This is how Heroku will install your project dependencies. Use pip freeze to add the installed packages to requirements.txt

		$ pip freeze > requirements.txt

1. Create a new Django project in the top-level git project directory:

		$ django-admin startproject my_project . # Note the trailing '.' which means 'current directory'

1. Edit your .gitignore file to include an entry for .sql files. By default, Django will use sqlite3 (a basic database that is bundled with Python) and you don't want to commit your database file to your repository! Open .gitignore and add the following at the end of the file:

		*.sql*

1. This is a good time to verify that your project runs (always a good idea before committing code!) and if it does, commit the changes.

### Process for Committing Code

Commit as often as you can, but only commit working code. Every time you have added some piece of functionality and your project runs successfully, it's a good idea to commit. Outlined here is a good process to follow every time you commit code.

1. On the command line, run your Django app:

		$ python manage.py runserver
		
1. In a browser, open [http://127.0.0.1:8000](http://127.0.0.1:8000) where you should see "It worked!"

1. Check that the list of changed files is as expected:

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
		
1. Check your diff to see what is in it and make sure it all makes sense:

		$ git diff

1. Commit and push your changes.

		$ git add -A  # Add all modified files to staging
		$ git commit -m "Initialized Django project"  # Commit staged files
		$ git push origin master  # Push commit to github

1. You can go to your GitHub project in the browser and you should see your changes.

1. Summary of essential commit process:

		$ python manage.py runserver  # And check in browser [http://127.0.0.1:8000](http://127.0.0.1:8000)
		$ git status -u  # Check what files have been modified
		$ git diff  # Look over your changes (always a good idea to ensure it is as expected)
		$ git add -A  # Add all modified files to staging
		$ git commit -m "Added first view"  # Commit staged files
		$ git push origin master  # Push commit to github

## Add Functionality to Django App

Now comes the fun part: making your Django app do something interesting!

### Starting a new work session

1. Ensure you are set up to start working. You will want to do this each time you start a working. On the command line, do the following:

		$ cd ~/Development/my_project
		$ workon my_project

### Creating a New App

In Django, an app is a cohesive collection of functionality inside a project. An example of an app might be user account management. Ideally, pps should be self-contained so that an app from one Django project can be reused in another Django project. You will need to follow these instructions each time you want to add a new aspect to your project.

1. To create an app:

		$ python manage.py startapp app1
		
1. You will need to add your new app to my_project/settings.py. This lets Django know about your app, e.g. for creating/running migrations.

		# settings.py
		INSTALLED_APPS = (
				...
    		'app1',
		)

1. Create a urls.py file for your new app. Inside the app1 directory, create urls.py and enter the following:		
		
		# app1/urls.py
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

1. Edit my_project/urls.py and include the urls for your new app. Also, add a redirect so that anyone going to the base URL for your project will be redirected to app1. Once you are done, urls.py should look like this:

		# urls.py
		from django.conf.urls import include, url
		from django.contrib import admin
		from django.views.generic import RedirectView
		
		from app1 import urls as app1_urls  # Import for app1 urls
		
		
		urlpatterns = [
				url(r'^admin/', include(admin.site.urls)),
				url(r'^$', RedirectView.as_view(url='app1', permanent=True)), # Redirect base URL to app1
				url(r'^app1/', include(app1_urls), name='app1'),
		]
		
1. On the command line, run your Django app:

		$ python manage.py runserver
		
1. In a browser, open [http://127.0.0.1:8000](http://127.0.0.1:8000). This should redirect you to [http://127.0.0.1:8000/app1](http://127.0.0.1:8000/app1), which then displays the standard Django 404 page. Not to worry! This is because we've set up an automatic redirect from the root to app1, but the urls.py file for app1 doesn't have any routes in urlpatterns yet. Whenever you get this error, you can check the address in the url bar in the browser and make sure it matches a pattern in urls.py.
		
### Creating a new view

You will need to follow these instructions for every new view you create. Apps typically have multiple views.

1. Create a template for your new view, called index.html, inside the app1/templates/app1 directory. Django templates can do variable substitution, looping, and all sorts of other useful operations, but for now, we will make a very simple "Hello World" template containing plain HTML. Edit index.html to contain the following:

		# app1/templates/app1/index.html
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

		# app1/views.py
		from django.shortcuts import render
		
		
		def index(request):
				context = {}  # So far, we are not specifying any custom context
				return render(request, 'app1/index.html', context)

1. Add your new view to the app urls. Edit app1/urls.py to add a url for your new view. Once you are done, the file should look like the following

		# app1/urls.py
		from django.conf.urls import url
		
		from . import views  # Import the views for this app
		
		
		urlpatterns = [
				url(r'^$', views.index, name='index'),  # Url for index view
		]
		
1. It's going to get tedious appending the /app1 to your url every time, so let's add a redirect. This means that anyone going to the base URL for your project will be redirected to app1. You would typically only do this once per project, so that the first time someone goes to your domain, they are taken to the appropriate landing page. Once you are done, urls.py should look like this:

		# urls.py
		from django.conf.urls import include, url
		from django.contrib import admin
		from django.views.generic import RedirectView
		
		from app1 import urls as app1_urls  # Import for app1 urls
		
		
		urlpatterns = [
				url(r'^admin/', include(admin.site.urls)),
				url(r'^$', RedirectView.as_view(url='app1', permanent=True)), # Redirect base URL to app1
				url(r'^app1/', include(app1_urls), name='app1'),
		]
		
1. Verify that your new view is available by navigating to the new app in the browser: [http://127.0.0.1:8000/app1/](http://127.0.0.1:8000/app1/) Note that you need the /app1 to go directly to app1, because the top-level urls.py links app1 urls to the /app1 path. Because we added a redirect, if you go to [http://127.0.0.1:8000/](http://127.0.0.1:8000/) you will be redirected to /app1.

1. [Verify and commit your code](#process-for-committing-code).
		
### Creating a new library

External libraries can be installed in your virtualenv using pip, but you may also want to create libraries within your application. A library is a related set of functions, e.g. for accessing an external API like Amazon S3. This code isn't necessarily related to any specific Django app, but it is needed for your project.

1. Create a directory for your libraries, called "libs" in the root of your project, next to app1 and my_project. You only need to do this once per project.

1. Inside the "libs" directory, create a new python file for the library you want to create. You need to do this once for every library you add. In this case, we will be creating a library to access the [Open Weather Map API](http://openweathermap.org/api), so we will call the file open_weather_map.py

1. The [requests](http://www.python-requests.org/en/latest/) library is the easiest way to make API calls from Python, so install requests and add it to the project requirements. Ensure that you are in the project root directory and the virtualenv is active first!

		$ workon my_project
		$ pip install requests
		$ pip freeze > requirements.txt
		
1. Open libs/open_weather_map.py, and make a class for accessing the Open Weather Map API. In general, when accessing an external API, it's a good idea to make a class that has at least the base API URL as a member.

		# libs/open_weather_map.py
		import requests
		
		
		class OpenWeatherMap():
		
				def __init__(self):
						# This is the base URL for the API and will be common to all requests
						self.api_url = 'http://api.openweathermap.org/data/2.5/'
						
1. Now that we have an Open Weather Map class, let's use it to get some data! There are many options on the API we've chosen; for now, let's do the [5 day/3 hour forecast](http://openweathermap.org/forecast5). This will be a GET request, since we are retrieving (not modifying) data on the server. The documentation indicates that we must pass in some form of location information to indicate which forecast we want. The requests library makes it easy:

		# libs/open_weather_map.py
		import requests
		
		
		class OpenWeatherMap(object):
				
				def __init__(self):
						self.api_url = 'http://api.openweathermap.org/data/2.5/'
		
				def get_forecast(self):
						# Append the "forecast" path to the base API URL
						url = '%sforecast' % self.api_url
		
						# URL parameters
						params = {
								'lat': -33.8650,
								'lon': 151.2094
						}
		
						# Do a get request on the forecast url, with the lat/lon params
						response = requests.get(url, params=params)
		
						# It's always a good idea to initialize results to empty, and update if data received
						results = {}
						# If the request was successful
						if response.status_code == requests.codes.ok:
								# Get the response body in JSON format
								results = response.json()
		
						return results

1. In order to display the retrieved information on the index page, we need to first call the API from our view code. This means we must import our library, instantiate the class, call our forecast method, and put the resulting data into the view context so the template can access it. Your app1 view code should look something like this when you are done:

		# app1/views.py
		from django.shortcuts import render
		
		from libs.open_weather_map import OpenWeatherMap
		
		
		def index(request):
				open_weather_map = OpenWeatherMap()
		
				context = {
						'forecast': open_weather_map.get_forecast()
				}
				return render(request, 'app1/index.html', context)
				
1. At last, we can add the weather data to our page! You don't need the "Hello World" or "first Django app" placeholder text anymore, so you can delete that. You can check the [API documentation](http://openweathermap.org/forecast5#JSON) to see the format of the returned JSON data. We will use that format information to display data in our page. You can start to see the power of django templates, as we reference members of the forecast data structure and loop over the items in the forecast list.

		# app1/templates/app1/index.html
		<!DOCTYPE html>
		<html lang="en">
		<head>
		<title>Local Forecast</title>
		</head>
		<body>
				<h3>Local Forecast</h3>
		
				<div>Location: {{ forecast.city.name }} ({{forecast.city.coord.lat}}, {{forecast.city.coord.lon}})</div>
				<br>
				<hr>
		
				{% for item in forecast.list %}
						<div>Date: {{ item.dt_txt }}</div>
						<div>Description: {{ item.weather.0.main }}</div>
						<div>Temperature (K): {{ item.main.temp }}</div>
						<hr>
				{% endfor %}
		</body>
		</html>

1. Congratulations, you just retrieved data from an external API and displayed it within a Django site! Probably a good time to [review your changes and commit your code](#process-for-committing-code). :)

## Deploy Django App to Heroku

Now that you have a basic functional weather project, you may want to share with friends or family. Unfortunately, as long as the project is running only on your own computer, you cannot share it with anyone remotely. Luckily, there are free hosting services available that make this easy. We are going to use Heroku, but there are many options.

1. Sign up for a free [Heroku](https://signup.heroku.com/login) account, if you don't already have one. You only need to do this once.

### Create Heroku App
1. In Heroku, navigate to Account -> Dashboard.
1. Click the '+' in the upper right to create a new app for your project.
1. You can enter a name or leave it blank and Heroku will create one for you.
1. Click the "Create App" button.

### Configure the Heroku CLI

1. On the Heroku page for your newly created app, ensure you are on the "Deploy" tab and the "Heroku git" sub-tab.
1. Follow the instructions for installing the [Heroku toolbelt](https://toolbelt.heroku.com).
1. On the command line, log in to Heroku using the username and password you set up on the Heroku website:

		$ heroku login
		
1. Connect your Heroku app to your local git project, where <HEROKU_APP_NAME> is the name of the app on Heroku (visible in the center top of the app page on the Heroku site):

		$ heroku git:remote -a <HEROKU_APP_NAME>


### Set up GitHub -> Heroku Deployment

1. On the Heroku page for your newly created app, click the "Deploy" tab, then click the "GitHub"/"Connect to GitHub" sub-tab.
1. Click the "Connect to GitHub" button. This will bring up an authorization popup.
1. In the popup, click "Authorize application" to connect your Heroku account to your GitHub account. This will require you to log into GitHub if you aren't already logged in.
1. Enter the GitHub repository name for your source code into the text input and click the "Search" button.
1. Once you have the correct repository displayed, click the "connect" button.
1. With the "master" branch selected, click the "Enable Automatic Deploys" button.
1. To deploy the code currently on GitHub, click "Deploy Branch".
1. Once deployment is complete, click the "View" button to check if the app is correctly deployed.
1. We haven't finished setting up Heroku yet, so you should get an error page in the browser. To find out what the error is, go to the command line, and in the project directory, check the logs:

		$ heroku logs
		
1. You should see an error something like "No web processes running". This is because we haven't started any web processes, and to do that, we need to define what a web process is for this app. Heroku relies on a Procfile to run your app, so let's create one now. Usually, Django web processes are run with gunicorn (installed with the django-toolbelt), using the wsgi file generated when the Django project was created. NOTE! The Procfile MUST be in the root of your project. Under the top level my_project directory, create a file named Procfile with the following content:

		# Procfile
		web: gunicorn my_project.wsgi --log-file -
		
1. Heroku defaults to Python 2.x, and we developed our app using Python 3, so we need to tell Heroku what version to use. You can do that by adding a runtime.txt file in the root of the project, with the following contents:

		# runtime.txt
		python-3.4.3
		
1. Let's [review your changes and commit your code](#process-for-committing-code) so the Procfile and runtime.txt are available to Heroku. This new commit will be automatically deployed.

1. You can check deployment status on the command line:

		$ heroku releases
		
1. Once the new release is complete, you can tell Heroku to create a web process:

		$ heroku ps:scale web=1

1. Refresh the browser window where you were viewing your heroku app. You should now see your app up and running with weather data!


# TODO

- Take location input
- static files
- database
- pyflakes
- html validator
- unit tests


# Resources

http://tutorial.djangogirls.org/en/index.html

https://docs.djangoproject.com/en/1.8/intro/tutorial01/

https://devcenter.heroku.com/articles/getting-started-with-django#prerequisites
