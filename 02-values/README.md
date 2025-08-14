# This section will discuss about how to perfectly write values file.

1. We will start by creating a helm chart.
```bash
helm create httpd-chart
```
2. Now check the files created by helm chart.
```bash
ls -l httpd-chart/
total 8
drwxr-xr-x. 2 root root    6 Aug  5 14:25 charts
-rw-r--r--. 1 root root 1147 Aug  5 14:25 Chart.yaml
drwxr-xr-x. 3 root root  162 Aug  5 14:25 templates
-rw-r--r--. 1 root root 2364 Aug  5 14:25 ¸
```
3. As you can see there is a default values.yaml file lets check the content of this file.
```bash
grep -v # values.yaml
replicaCount: 1
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""
imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""
serviceAccount:
  create: true
  automount: true
  annotations: {}
  name: ""
podAnnotations: {}
podLabels: {}
podSecurityContext: {}
securityContext: {}
service:
  type: ClusterIP
  port: 80
ingress:
  enabled: false
  className: ""
  annotations: {}
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []
resources: {}
livenessProbe:
  httpGet:
    path: /
    port: http
readinessProbe:
  httpGet:
    path: /
    port: http
autoscaling:
  enabled: false
  minReplicas: 1
  maxReplicas: 100
  targetCPUUtilizationPercentage: 80
volumes: []
volumeMounts: []
nodeSelector: {}
tolerations: []
affinity: {}
```
4. We can modify this values.yaml file to as per our requirement.
5. Lets start by modifing the no of replicas and image in the files.
   - Change the no of replicas to 2
   - Change the image to docker.io/nginx:latest
```bash
replicaCount: 2
image:
  repository: docker.io/nginx
  pullPolicy: IfNotPresent
  tag: "latest"
```
6. Now try to deploy this helm chart.
```bash
helm install myapp .
```
7. Check the resources created by helm chart. As we can see it has created a deployment, replicaset and 2 pods.
```bash
kubectl get all
NAME                                     READY   STATUS    RESTARTS   AGE
pod/myapp-httpd-chart-77d6ffd868-mx46r   1/1     Running   0          75s
pod/myapp-httpd-chart-77d6ffd868-wg77f   1/1     Running   0          75s
NAME                        TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/kubernetes          ClusterIP   10.96.0.1      <none>        443/TCP   5d18h
service/myapp-httpd-chart   ClusterIP   10.99.100.70   <none>        80/TCP    75s
NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/myapp-httpd-chart   2/2     2            2           75s
NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/myapp-httpd-chart-77d6ffd868   2         2         2       75s
```
8. Check the image used by myapp-httpd-chart pod.
```bash
kubectl get pod myapp-httpd-chart-77d6ffd868-mx46r -o jsonpath='{.spec.containers[*].image}'
docker.io/nginx:latest
```
9. Using a custom values file.
  - Create a new values file with image and 1 replice
```bash
vim httpd-values.yaml
replicas: 1
image:
  repository: docker.io/httpd
  tag: "latest"
```
  - Now  install helm with this custom values file.
```bash
helm install httpd -f httpd-values.yaml .
```
