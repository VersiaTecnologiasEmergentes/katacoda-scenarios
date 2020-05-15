# Kubectl proxy

**Kubectl proxy** nos permitirá acceder al API de Kubernetes, lo levantaremos mediante:

`kubectl proxy --port=8080 &`{{execute}}

Podemos ver el contenido del API:

`curl http://localhost:8080/`{{execute}}

Vamos a hacer algunas sencillas pruebas:

- Comprobamos versión:

`curl http://localhost:8080/version`{{execute}}

- Comprobamos estado:

`curl http://localhost:8080/healthz`{{execute}}

- Mostramos todos los recursos de la API v1

`curl http://localhost:8080/api/v1/`{{execute}}

- Mostrar todos los namespaces:

`curl http://localhost:8080/api/v1/namespaces`{{execute}}

- Mostramos la lista de los Pods del namespace default:

`curl http://localhost:8080/api/v1/namespaces/default/pods`{{execute}}

- Accedemos a la información del Pod:

`curl http://localhost:8080/api/v1/namespaces/default/pods/webserver2`{{execute}}

Y ahora vamos a usar una de las funcionalidades del API para ver lo que el contenedor está exponiendo:

`curl http://localhost:8080/api/v1/namespaces/default/pods/webserver2/proxy/`{{execute}} 

Aunque esta es una posibilidad, no es la óptima. Veremos más adelante como los servicios nos ayudarán a hacer esto de forma bastante más sencilla.