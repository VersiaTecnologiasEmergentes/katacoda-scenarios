# Poner CPU y memoria a los namespaces

En este apartado vamos a ver como se le pueden poner límites de recursos a los Pods que se crean en un namespace. Para ello vamos a crear un nuevo namespace:

`kubectl create namespace test`{{execute}}

`kubectl get namespaces`{{execute}}

Ahora tenemos que crear el *yaml* donde se definen los límites:

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: recursos
spec:
  limits:
  - default:
      memory: 512Mi
      cpu: 1
    defaultRequest:
      memory: 256Mi
      cpu: 0.5
    max:
      memory: 1Gi
      cpu: 4
    min:
      memory: 128Mi
      cpu: 0.5
    type: Container
```

Desplegamos:

`kubectl apply -f kubernetes_101_lab/namespace/lab3/limites.yaml -n test`{{execute}}

Comprobamos que los límites se han aplicado:

`kubectl describe namespace test`{{execute}}

Para comprobar que los Pods se nos crean con esas limitaciones de recursos, vamos a desplegar de forma imperativa un nginx:

`kubectl run nginx --image=nginx -n test`{{execute}}

`kubectl get pods -n test`{{execute}}

Comprobamos las características del pod:

`kubectl describe pod nginx -n test`{{execute}}



