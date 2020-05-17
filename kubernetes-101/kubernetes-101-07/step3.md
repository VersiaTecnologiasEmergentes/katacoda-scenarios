# Creando objetos en un namespace

Primero vamos a crear un namespace denominado **desarrollo**:

 `kubectl create namespace desarrollo`{{execute}}

`kubectl get namespaces`{{execute}}

En este caso vamos a crear un deployment basado en *Elastic* dentro del nuevo namespace que hemos creado. El fichero de deployment sera:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: elastic
  labels:
    tipo: "desarrollo"
spec:
  selector:   
    matchLabels:
      app: elastic
  replicas: 1
  strategy:
     type: RollingUpdate
  minReadySeconds: 2
  template:   # Plantilla que define los containers
    metadata:
      labels:
        app: elastic
    spec:
      containers:
      - name: elastic
        image: elasticsearch:7.6.0
        ports:
        - containerPort: 9200
```

Para desplegar ejecutamos:

`kubectl apply -f kubernetes_101_lab/namespace/lab2/elastic-deployment.yaml  --namespace=desarrollo`{{execute}}

Podemos ver como lo ha creado en nuestro namespace:

`kubectl get pods -n desarrollo`{{execute}}

`kubectl describe pods elastic -n desarrollo`{{execute}}



