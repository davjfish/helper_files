# DJANGO NOTES

# Clean up migrations
1. fake undoing migrations
```
python manage.py migrate --fake my_app zero
```
1. delete everything in migration folder except __init__.py
1. make migrations...
```
python manage.py makemigratations my_app
```
1. fake initial migration
```
python manage.py migrate --fake-initial
``` 