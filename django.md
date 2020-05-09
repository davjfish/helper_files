# DJANGO NOTES

# Clean up migrations
1. make sure all migrations have been made and have been applied `python manage.py makemigrations` and `python manage.py migrate`
1. fake undoing migrations for the app you want to reset `python manage.py migrate --fake my_app zero`
1. delete everything in migration folder except __init__.py
1. make migrations `python manage.py makemigrations my_app`
1. fake initial migration `python manage.py migrate --fake-initial` 

# Start a new project
```
django-admin startproject <name> .

#the . means start it in the current folder
```
