# Viendo los logs de un Pod.

En este momento deberíamos tener desplegados dos pods y lo que vamos a hacer es desplegar otro más. En este caso, optaremos por desplegar un Apache para los que no les guste nginx :), lo haremos de la siguiente forma:

`kubectl run apache --image=httpd --port=8080 --generator=run-pod/v1`{{execute}}

`kubectl get pods`{{execute}}

Para ver los logs del nuevo Pod creado:

`kubectl logs apache`{{execute}}

`kubectl logs apache --tail=10`{{execute}}

Todavía no tenemos configurada la parte de red, pero podemos podemos hacer una prueba usando lo que hemos aprendido hasta ahora. Primero nos vamos a conectar al Pod de Apache:

`kubectl exec apache -it bash`{{execute}}

Ahora vamos a instalar wget:

`apt update -y`{{execute}}

`apt install wget -y`{{execute}}

Ahora lanzamos la prueba del Apache:

`for i in $(seq 1 10);do wget localhost;done`{{execute}}

`exit`{{execute}}

`kubectl logs apache`{{execute}}