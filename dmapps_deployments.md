# DMApps Deployment Checklist

## All Apps
- Ensure new app is added to dmapps/urls.py and dmapps/views.py
- Ensure new app has functions to create user groups in your_project/utils.py
- Ensure new app is added to default_conf.py APP_DICT
- Ensure any new plugin apps are added to requirements.txt and settings.py INSTALLED_APPS
- Ensure views have been optimized to remove unnecesary imports (ctrl + alt + O) and/or open dialog to choose (ctrl + alt + shift + L)

## Geodjango Apps
- Ensure app is added to "if not GEODJANGO:" in settings.py
