# Creando nuestro primer servicio.

vamos a lanzar un deployment:

`kubectl apply -f kubernetes_101_lab/services/lab1/deploy.yaml`{{execute}}

`kubectl get po,deploy`{{execute}}

vamos a crear el servicio de modo imperativo

`kubectl expose deployment webserver --name=webserver-svc --target-port=80 --type=NodePort`{{execute}}

Para comprobarlo:

`kubectl get svc`{{execute}}

`kubectl describe service webserver-svc`{{execute}}

Podemos hacer la prueba de escalar:

`kubectl scale --replicas=3 deployment/webserver`{{execute}}

`kubectl describe service webserver-svc`{{execute}}

`minikube ip`{{execute}}


