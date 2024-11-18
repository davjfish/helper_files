# Kubernetes: 
 - List nodes:  `kubectl get nodes`
 - Kubeconfig file can be copied from ~/.kube/config
 - Get command for adding a new node: `kubeadm token create --print-join-command`
 - Changes can be made by applying .yaml files: `kubectl apply -f $PATHTOYAML`

### Namespaces
 - Most kubectl commands need to be run with a `-n $NAMESPACE` flag in order to filter to relevant results
 - List namespaces: `kubectl get namespaces`

### Pods
 - List pods:  `kubectl get pods -n $NAMESPACE`
 - Get pod details / errors: ` kubectl describe pod $PODNAME -n $NAMESPACE`  Pod name comes from get pods and will contain a unique hash
 - Get pod logs: `kubectl logs $PODNAME -n $NAMESPACE`
 - Enter shell in pod: `kubectl exec -n $NAMESPACE -it $PODNAME -- bash`
 - Delete/restart: `kubectl delete pod $PODNAME -n $NAMESPACE`

### Storage
 - Two parts are necessary a volume and a claim. There is a one-to-one relationship between volumes and claims. The exists as two entities because sometimes the volume can be on a cloud service and you would only need the claim.   
 - For local storage, it makes sense to create them at the same time
 - 
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: {{ .Release.Name }}-pv # unique name for the volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany  # lets many pods read the volume at once, important for serving media files from nginx 
  hostPath:
    path: /data/{{ .Values.mission }}/  # where the files will get saved on host nodes. Note that this is specific to the node the pod is running on, it won't be on all nodes

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  namespace: andes-{{ .Values.mission }} # namespace the claim can be accessed from
  name: {{ .Values.mission }}-pvc # unique name
  labels:
    type: local
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany # many pods can use this claim
  resources:
    requests:
      storage: 1Gi
  volumeName: {{ .Release.Name }}-pv # must match the unique name of the volume


```

### Services
- Mainly use loadbalancers
- These work by listening on a port and routing traffic to specific pods. The pods are selected using the `selector` 
- The port being listened on can be an externalIP, this is how we can make pods accessible to the network
- If no externalIP is specified, the listened port will be on the k8s network and can be accessed from other pods/services with http://SERVICENAME:TARGETPORT;

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-{{ .Values.mission }}-service  # unique name of the service
  namespace: {{ .Values.mission }}
  labels:
    app: nginx-svc  # use this for selecting the service
spec:
  selector:  # select the apps/pods that this service is going to route trafic too
    app: nginx
    mission: {{ .Values.mission }}
  ports:
    - name: http
      protocol: TCP
      port: 123 # port to listen on, on the host/pod 
      targetPort: 80 # port on apps/pods selected above
  externalIPs:
    - 142.#.#.#  # IP of host, this lets the service listen directly on the host instead of the internal k8s network
```



## Secrets:
To create the db password secret, kubectl apply this file:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: andes-secret
type: Opaque
stringData:
  DB_PASSWORD: BASE64PASSWORD # get this with $ echo -n PASSWORD | base64  
```

For the container registry run this file/command:
```bash
#!/bin/bash
kubectl create secret docker-registry dmappsdevtestacr \
    --docker-server=dmappsdevtestacr.azurecr.io \
    --docker-username=dmappsdevtestacr \
    --docker-password=THE_PASSWORD
```

### Helm
 - Helm allows for the use of variables in the .yaml templates and provides a nice way of deploying everything all at once
 - To perform an update on a mission:
1. Make sure that the new docker image exits on Azure
2. Update the image tag in `mission.yamls` (unless using `latest`)
3. Run `helm upgrade $RELEASE_NAME ~/$PATH_TO_HELM_CHART/ -f ~/$PATH_TO_MISSION.yaml`


Useful sites:
https://dev.to/mkalioby/django-on-k8s-part-iii-running-on-kubernetes-2m91
https://medium.com/@tech_with_mike/how-to-deploy-a-django-app-over-a-kubernetes-cluster-with-video-bc5c807d80e2