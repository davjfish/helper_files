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
# Start a new app
```
python manage.py startapp <name>
```

# Add App to Project
In settings.py under INSTALLED_APS = [ ] add the name of your app

# Add App URLs to Project
1) In urls.py of the project folder under urlpatterns = [] add your app
```
#ie
path('<appname>/', include('<appname>.urls')),
```
2) Create a urls.py file in your app and add your urls and app name
```
#ie
from django.urls import path
from projects import views

app_name = "projects"

urlpatterns = [
    path('', views.project_list, name="project_list"),
]
```
Using <b> app_name </b> and <b> name= </b> later allows you in templates to refer to this url using <b> {% url 'projects:project_list' %}



# DJANGO Templating Language
```
{% code logic %}
{{ variables }}
```
