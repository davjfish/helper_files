# DJANGO NOTES

# Clean up migrations
1. make sure all migrations have been made and have been applied `python manage.py makemigrations` and `python manage.py migrate`
1. fake undoing migrations for the app you want to reset `python manage.py migrate --fake my_app zero`
1. delete everything in migration folder except __init__.py
1. make migrations `python manage.py makemigrations my_app`
1. fake initial migration `python manage.py migrate --fake-initial` 

https://simpleisbetterthancomplex.com/tutorial/2016/07/26/how-to-reset-migrations.html

# Create an empty migration
```
python manage.py makemigrations --empty <appname>
```
This can be used to make an empty migration in sequence. For example you could load data in this way using or create a custom migration:
```
def load_date(apps, schema_editor):
    XY = apps.get_model("appname", "modelname")
    
    XY(field1=thing, field2=123,).save()
    
operations = [
    migrations.RunPython(load_data)
    ]
```
However see cautions here: https://realpython.com/data-migrations/#comment-1843026722

# Name a migration
```
python manage.py makemigrations mmutools --name "fix_items_table_field_names_add_size_fk"
```

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
In settings.py under INSTALLED_APPS = [ ] add the name of your app

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
Using <b> app_name </b> and <b> name= </b> later allows you in templates to refer to this url using <b> {% url 'projects:project_list' %}</b>

# Remove an App
1) Clean the database
```
python manage.py migrate my_app_name zero
```
2) Remove the app from your project
```
Delete it from INSTALLED_APPS in your project's settings.py
```
3) Delete app files and references to app anywhere else in project


# Manage models from Admin
1) From the Terminal type the following and follow prompts:
```
python manage.py createsuperuser
```
2) Go in to the app admin.py file and register the models in the admin interface
```
from myproject.myapp.models import Author
admin.site.register(Author)
```

# DJANGO Templating Language
```
{% code logic %}
{{ variables }}
```
