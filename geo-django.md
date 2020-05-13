# Setting up a location-based app with geo-django

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

### QGIS
(If you don't want to install QGIS you can independently install GDAL, GEOS and PROJ.4)
- 
