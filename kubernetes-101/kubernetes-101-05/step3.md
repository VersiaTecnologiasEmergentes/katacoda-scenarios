## SELECTOR

Para hacer la prueba vamos a usar cuatro despliegues:

- tomcat
- tomcat1
- tomcat2
- tomcat3

Desplegamos todos mediante:

`cd kubernetes_101_lab/labels/lab1/`{{execute}}

`kubectl apply -f .`{{execute}}

Veamos todas las etiquetas:

`kubectl get pods --show-labels`{{execute}}

A continuación algunas pruebas con los selectores:

`kubectl get pods --show-labels -l estado=desarrollo`{{execute}}

`kubectl get pods --show-labels -l estado=testing`{{execute}}

`kubectl get pods --show-labels -l responsable!=sistemas`{{execute}}

`kubectl get pods --show-labels -l 'estado in(desarrollo)'`{{execute}}

`kubectl get pods --show-labels -l 'estado in(desarrollo,testing)'`{{execute}}

`kubectl get pods --show-labels -l 'estado notin(desarrollo)'`{{execute}}

`kubectl delete pods -l estado=desarrollo`{{execute}}

`kubectl get pods`{{execute}}