# DJANGO NOTES

# Clean up migrations
1. fake undoing migrations for the app you want to reset `python manage.py migrate --fake my_app zero`
1. delete everything in migration folder except __init__.py
1. make migrations `python manage.py makemigrations my_app`
1. fake initial migration `python manage.py migrate --fake-initial` 