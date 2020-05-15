# Escalando la aplicación aumentando el número de replicas.

Podemos aumentar el número de pods en nuestro deployment aplicando un nuevo fichero yaml como este:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4 # Actualiza el número de réplicas de 2 a 4
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.8
        ports:
        - containerPort: 80
```

Aplicamos los cambios de escalado:

`kubectl apply -f kubernetes_101_lab/deplyment/lab1/deployment-scale.yaml`{{execute}}

`kubectl get pods -l app=nginx`{{execute}}



