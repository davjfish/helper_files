# Azure NOTES

- log into azure: `az login`
- log in with MSI `az login --identity` (system-based)
- log in with MSI `az login --identity -u [MSI_CLIENT_ID]` (user-based)

- display configuration for web app:
```az webapp config appsettings list --name MyWebapp --resource-group MyResourceGroup```
- add a new configuration on a web app:
```az webapp config appsettings set -g "$AZURE_RESOURCE_GROUP" -n "$WEB_APP_NAME" --settings [KEY]="[VALUE]"```
- create an ACR Task to build image upon commit:
```az acr task create --registry "$ACR_SERVICE_NAME" --name "$NEW_TASK_NAME" --image "DOCKER_IMAGE:TAG" --context "https://MY_REPO#MY_BRANCH" --file "Dockerfile" --git-access-token "MY_PAT" -g "DMAPPS-DEV-RG"```
- display the details of an ACR task:
```az acr task show -r "$ACR_SERVICE_NAME" -n "$TASK_NAME" -o table```
- display the status of recent ACR tasks that have been run:
``` az acr task list-runs -r "$ACR_SERVICE_NAME" -n "$TASK_NAME" -o table```