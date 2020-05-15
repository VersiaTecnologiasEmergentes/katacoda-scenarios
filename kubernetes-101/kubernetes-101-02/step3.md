# Ejecutar comandos dentro del Pod.

Igual que en los entornos Docker, en Kubernetes podemos lanzar comandos contra los pods. Por ejemplo, en este caso vamos a sacar la configuración que tiene el nginx del webserver2:

`kubectl exec webserver2 cat /etc/nginx/nginx.conf`{{execute}}

Por su puesto, también podemos entrar en modo interactivo para inspeccionar el contenedor que se esta ejecutando:

`kubectl exec webserver2 -it bash`{{execute}}

`hostname`{{execute}}

`cat /etc/nginx/nginx.conf`{{execute}}

Para salir de la consola:

`exit`{{execute}}