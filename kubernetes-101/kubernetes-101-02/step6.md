# Borrar Pods.

Para borrar los Pods usaremos el modificador **delete** al que le tendremos que pasar el tipo de recurso y el nombre del mismo. Vamos a ver un ejemplo:

Inicialmente mostramos los Pods desplegados:

`kubectl get pods`{{execute}}

Borramos los Pods:

`kubectl delete pod apache`{{execute}}

`kubectl delete pod webserver2`{{execute}}

`kubectl delete deploy webserver`{{execute}}

Comprobamos que se ha borrado correctamente:

`kubectl get pods`{{execute}}

