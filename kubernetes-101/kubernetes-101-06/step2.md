# Servicios. CLUSTERIP / NodePort/LoadBalancer



## CLUSTERIP



vamos a lanzar un deployment:

`cat kubernetes_101_lab/services/lab1/deploy.yaml`{{execute}}

`kubectl apply -f kubernetes_101_lab/services/lab1/deploy.yaml`{{execute}}

`kubectl get po,deploy`{{execute}}

vamos a crear el servicio de modo imperativo

`kubectl expose deployment webserver --name=webserver-svc --target-port=80 --type=ClusterIP`{{execute}}

Para comprobarlo:

`kubectl get svc`{{execute}}

`kubectl describe service webserver-svc`{{execute}}

un servicio del tipo *ClusterIP* no podemos acceder desde el exterior. Cualquier pod si podría acceder a ese servicio.

Probaremos con un curl http://CLUSTERIP:PORT



Podemos hacer la prueba de escalar:

`kubectl scale --replicas=3 deployment/webserver`{{execute}}

`kubectl describe service webserver-svc`{{execute}}

Ahora borramos el servicio:

`kubectl delete service webserver-svc`{{execute}}

`kubectl get services`{{execute}}



##  NodePort

Para levantar el servicio con NodePort, bastará con:

`kubectl expose deployment webserver --name=webserver-svc --target-port=80 --type=NodePort`{{execute}}

`kubectl get svc`{{execute}}



Podemos comprobar que funciona usando el + de la parte superior para redirigir al puerto que hemos obtenido anteriormente. 

`kubectl delete service webserver-svc`{{execute}}

`kubectl get services`{{execute}}



## LoadBalancer

Para levantar el servicio con LoadBalancer, bastará con:

`kubectl expose deployment webserver --name=webserver-svc --target-port=80 --type=LoadBalancer`{{execute}}

`kubectl get svc`{{execute}}