## Actualizando el deployment

Vamos actualizar el deployment para que despliegue la imagen nginx 1.8. Para ello usaremos el siguiente yaml:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.8 # Actualiza la versión de nginx de 1.7.9 a 1.8
        ports:
        - containerPort: 80

```

Para aplicar el update:

`kubectl apply -f kubernetes_101_lab/deplyment/lab1/deployment-update.yaml`{{execute}}

Podemos comprobar como el deployment crea unos nuevos Pods con la nueva imagen mientras va eliminando los Pods con especificación antigua.

`kubectl get pods -l app=nginx`{{execute}}