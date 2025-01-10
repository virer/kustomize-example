Generate Helm Charts structure out of kustomize output
```console
$ kustomize build ../development/ | helmify myapps
```

Generate 
```console
$ helm template myapps
---
# Source: myapps/templates/myapp.yaml
apiVersion: v1
kind: Service
metadata:
  name: release-name-myapps-myapp
  labels:
    app: myapp
    helm.sh/chart: myapps-0.1.0
    app.kubernetes.io/name: myapps
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "0.1.0"
    app.kubernetes.io/managed-by: Helm
spec:
  type: ClusterIP
  selector:
    app: myapp
    app.kubernetes.io/name: myapps
    app.kubernetes.io/instance: release-name
  ports:
  - name: 80-80
    port: 80
    protocol: TCP
    targetPort: 80
---
# Source: myapps/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: release-name-myapps-web-deployment
  labels:
    helm.sh/chart: myapps-0.1.0
    app.kubernetes.io/name: myapps
    app.kubernetes.io/instance: release-name
    app.kubernetes.io/version: "0.1.0"
    app.kubernetes.io/managed-by: Helm
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
      app.kubernetes.io/name: myapps
      app.kubernetes.io/instance: release-name
  template:
    metadata:
      labels:
        app: web
        app.kubernetes.io/name: myapps
        app.kubernetes.io/instance: release-name
    spec:
      containers:
      - env:
        - name: KUBERNETES_CLUSTER_DOMAIN
          value: "cluster.local"
        image: registry.redhat.io/rhel9/nginx-120:latest
        name: nginx
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 700m
          requests:
            cpu: 500m
```
