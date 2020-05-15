## Generando el yaml de un Pod en ejecución.

Algunas veces no disponemos de los yaml que han usado para desplegar el Pod que se encuentra en ejecución. Aunque no es una práctica que deberías permitir en nuestros entornos, podemos obtener de forma sencilla la configuración del Pod. Para ello:

`kubectl get pod webserver -o yaml`{{execute}}

Lo mas interesante es redireccionar esta salida a un fichero:

`kubectl get pod webserver -o yaml >> webserver.yml`{{execute}}

Vamos a hacer una pequeña prueba a ver si podemos recuperar el Pod desde el fichero que acabamos de crear. Primero borraremos el Pod y pasaremos a crearlo desde el fichero *"webserver.yaml":

`kubectl delete pod webserver`{{execute}}

`kubectl get pods`{{execute}}

`kubectl create -f webserver.yml`{{execute}}

`kubectl get pods`{{execute}}

Para terminar borramos el Pod:

`kubectl delete pod webserver`{{execute}}