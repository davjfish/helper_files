[Geodjango with PostgreSQL](#method-1) 

[Geodjango with sqlite3](#method-2)

# Method 1
# Setting up a location-based app with geo-django with PostgreSQL

Software you need:
- PostgreSQL
- Python
- QGIS (for easy install of dependencies: GDAL, GEOS and PROJ.4)
- Django (with GeoDjango enabled)
- Leaflet (optional)

# Basic Setup of Components

### PostgreSQL
- download latest version (or one prior to latest) at https://www.postgresql.org/download/
- install with defaults
- choose password for superuser account
- default port 5432
- launch StackBuilder at the end of install
- choose new install of PostgreSQL
- open 'spatial extensions' and install 'PostGIS xx for PostgreSQL'
- do not select 'upgrade mode' option
- enter the password you created earlier (if required)
- on completion of install, find PostgreSQL folder and launch pgAdmin 4
- enter superuser password
- right click 'Database', and create a new database
- name your database and save
- right click on 'extensions' under new database and then 'create', type 'postgis' and select and save

### Python
- download latest version at https://www.python.org/downloads/
- install and reboot

### QGIS
(If you don't want to install QGIS, you can independently install GDAL, GEOS and PROJ.4)
- download latest version at https://qgis.org/en/site/forusers/download.html
- install 

### Django
- create your virtual environment as usual
- use pip to install django
- create your project 
- go into settings.py in your project and add geo-django to installed apps
```
INSTALLED_APPS = [
'django.contrib.gis',
]
```

# Connect Project to PostgreSQL Database
Python cannot natively connect to a PostgreSQL database, so you need the following:
- ensure pip is updated (pip install -U pip)
- use pip to install psycopg2-binary (binary version includes libs required that psycopg2 does not have)

You will need the following info from your PostgreSQL database to connect it:
- name of database
- user (postgres by default when you create superuser)
- password 
- host
- port 

To be secure, you can create a new user account specific to your new project database.
- launch pgAdmin4 from your PostgreSQL folder
- select your database
- from the top menu select 'Tools' and 'Query Tool'
- use SQL to create account:
```
CREATE USER shopadmin WITH PASSWORD 'shopadmin'; 
GRANT ALL PRIVILEGES ON DATABASE shops TO shopadmin;
```
- click on execute button
- refresh console to see new user under Login/Group Roles
- use new info to update settings.py in your project under Databases, change ENGINE from sqlite to postgis and add other info
```
DATABASES = {
    'default': {
        'ENGINE': 'django.contrib.gis.db.backends.postgis',
        'NAME': 'shops',
        'USER': 'shopadmin',
        'PASSWORD': 'shopadmin',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```
- before you create your app you may have to specify where your GDAL .dlls are in settings.py if you get any of these errors:
```
'django.contrib.gis.db.backends.postgis' isn't an available database backend.
Try using 'django.db.backends.XXX', where XXX is one of:
    'mysql', 'oracle', 'postgresql', 'sqlite3'
```
```
ImportError: Could not find the GEOS library (tried "geos_c", "libgeos_c-1"). Try setting GEOS_LIBRARY_PATH in your settings.
```
Try this:
```
GDAL_LIBRARY_PATH = r'C:\Program Files\QGIS 3.12\bin\gdal204'
GEOS_LIBRARY_PATH = r'C:\Program Files\QGIS 3.12\bin\geos_c.dll'
```
Further troubleshooting with this: https://stackoverflow.com/questions/49139044/geodjango-on-windows-could-not-find-the-gdal-library-oserror-winerror-12

# Create App
- create your app now as usual
- add to INSTALLED_APPS in settings.py

# Leaflet for maps
Learn more at https://leafletjs.com/examples/quick-start/
- add CSS, JS and map height to .html template head -- MUST be in this order
```
<head>
    <meta charset="UTF-8">
    <title>Nearby Shops</title>
    
    <!--    Leaflet CSS-->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
   integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
   crossorigin=""/>
   
     <!-- Leaflet JS - Make sure you put this AFTER Leaflet's CSS -->
    <script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
   integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
   crossorigin=""></script>
   
    <!--    Leaflet - set map height-->
    <style>
        #mapid { height: 180px; }
    </style>
    
</head>
```
- in body add an empty div for the map and then add the javascript to display markers
```
<!-- map div-->
    <div id="mapid"></div>

    <!-- map js -->
    <script>
        var map = L.map('mapid').setView([39.290440, -76.612330], 17);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

        L.marker([39.290440, -76.612330]).addTo(map)
            .bindPopup('I am in Baltimore.<br> Looking for shops!')
            .openPopup();

        {% for shop in shops %}
        var circle = L.circle([{{ shop.location.y }}, {{ shop.location.x }}], {
        color: 'red',
        fillColor: '#f03',
        fillOpacity: 0.5,
        radius: 5
      }).addTo(map);
        {% endfor %}

    </script>
```

# Method 2
# Setting up a location-based app with geo-django with sqlite3 and Spatialite

General steps:
- check Python version and (32 vs 64)
- download cyqlite (R-tree enabled)
- download correct versions of Spatialite files (zips)
- unpack zips somewhere (i.e. C:/Spatialite), sqlite first, then overwrite if prompted with Spatialite files

- if not using venv: add folder to System Path
- if using venv: go to venv folder, make a copy of Scripts folder, copy Spatialite folder files to Scripts folder

- in python DLL folder: rename sqlite3.dll to sqlite3_old.dll and copy sqlite3.dll from Spatialite folder


Reference materials:

App Tutorial: https://www.paulox.net/2020/12/08/maps-with-django-part-1-geodjango-spatialite-and-leaflet/

Setting up Spatialite: 

Spatialite install: https://pythongisandstuff.wordpress.com/2015/11/11/python-and-spatialite-32-bit-on-64-bit-windows/ (note: mod_spatialite no longer exists as such, have to download the three zips and unpack into venv)

Spatialite files: https://www.gaia-gis.it/gaia-sins/ (From red box at bottom)

Inserting in venv: https://stackoverflow.com/questions/39787700/unable-to-locate-the-spatialite-library-django

Error Messages:  
"ERROR 4: Unable to open EPSG support file gcs.csv. Try setting the GDAL_DATA environment variable to point to the directory containing EPSG csv files"  
Means it can't find the GDAL_DATA path. In my case I had deleted the osgeo folder that it originally linked to in my venv variables, so I had to manually go in and change it to the new directory.  
Resolved by: Edit configurations > Environment Variables > add GDAL_DATA path   
See: https://stackoverflow.com/questions/57288856/unable-to-open-epsg-support-file-gcs-csv-setting-the-gdal-data-environment-var



