Hypercube
=========

Commercial Underwriting Hypercube

![Built with Cookiecutter Django](https://img.shields.io/badge/built%20with-Cookiecutter%20Django-ff69b4.svg)
[Built with Cookiecutter Django](https://github.com/pydanny/cookiecutter-django/)

![Black code style](https://img.shields.io/badge/code%20style-black-000000.svg)
[Black code style](https://github.com/ambv/black)


Settings
--------

* To run the app locally, set up your local config by creating a .env file (don't commit this).
* A template .env.default file is included in this repo, you will need to add the DB password which is stored elsewhere (ask!).

Other settings for cookiecutter have moved to [settings](http://cookiecutter-django.readthedocs.io/en/latest/settings.html)

Basic Commands
--------------

### Setting Up Your Users

* To create an **superuser account**, use this command:

    $ python manage.py createsuperuser

For convenience, you can keep your normal user logged in on Chrome and your superuser logged in on Firefox (or similar), so that you can see how the site behaves for both kinds of users.

### Type checks

Running type checks with mypy:

  $ mypy hypercube

### Test coverage

To run the tests, check your test coverage, and generate an HTML coverage report::

    $ coverage run -m pytest
    $ coverage html
    $ open htmlcov/index.html

#### Running tests with py.test

  $ pytest

### Live reloading and Sass CSS compilation

Moved to [Live reloading and SASS compilation](http://cookiecutter-django.readthedocs.io/en/latest/live-reloading-and-sass-compilation.html)


Deployment
----------

See the [Deployment confluence page](https://confluence.int.corp.sun/confluence/display/INSRPA/Promote+Image+to+Non-Prod+Hypercube) for details.

The `dev` branch is automatically deployed to the Dev Hypercube cluster. Please develop on a feature branch and raise PRs to make changes to `dev`.

# Running a local Redis Server for Celery

Hi! I'm your first Markdown file in **StackEdit**. If you want to learn about StackEdit, you can read me. If you want to play with Markdown, you can edit me. Once you have finished with me, you can create new files by opening the **file explorer** on the left corner of the navigation bar.


# Download and install Redis
You can download and install redis from the following link
[https://github.com/tporadowski/redis/releases/tag/v5.0.10](https://github.com/tporadowski/redis/releases/tag/v5.0.10)

set the environment variable **CELERY_USE_LOCAL_REDIS=True**  
    Note: This is default set to True

Ensure you have installed celery and redis in your virtual environment by installing them from requirements.txt

## Run the local server

You can run the redis server with the following command in the directory where it is installed

    redis-server

## Start the celery worker

you can start a new celery worker with the following command 
**(from inside the hypercube virtual env)**

    celery -A hypercube worker -l info -P solo -E

## Start the celery beat service

To run periodic tasks, a beat worker is required to pass tasks to the celery worker on a given schedule

    celery -A hypercube beat -l info --scheduler celery.beat.PersistentScheduler --pidfile=


## Run all as a script
To make running all of these services simpler, You can place them in a batch script to be run automatically... (Or add them to your current script if you have one)

For example, Here is an example run.bat

    start cmd /c C:\Users\U226620\Documents\Redis-x64-5.0.10\redis-server
    start cmd /c celery -A hypercube worker -l info -P solo -E
    start cmd /c celery -A hypercube beat -l info --scheduler celery.beat.PersistentScheduler --pidfile=
    python manage.py runserver 0.0.0.0:8000
