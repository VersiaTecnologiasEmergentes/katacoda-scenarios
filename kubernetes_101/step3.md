# PODS 101. Introducción.

## ¿Qué es un Pod?

Los **Pods** son unos de los conceptos más importantes en Kubernetes, ya que son los objetos claves con los que interactúan los desarrolladores.  Un Pod será la unidad de ejecución básica de una aplicación Kubernetes, la unidad más pequeña y simple en el modelo de objetos que crea o implementa Kubernetes. El Pod encapsula el contenedor (ó contenedores) de una aplicación junto con los recursos de almacenamiento, red y las reglas de ejecución.   

El diagrama de arquitectura de un Pod queda reflejado en el siguiente esquema:

<img src="./assets/Pod_architecture.png" alt="image-20200513132919459" style="zoom:50%;" />

Una característica interesante de los Pods es que son efímeros, con una vida útil normalmente limitada. Al reducir el escalado o actualizar a una nueva versión, los Pods mueren. Es una idea que tenemos que tener clara, cuando un Pod falla o cambia, todo lo que contenía desaparece por lo que nos debemos de apoyar en herramientas externas para poder hacer análisis de *"root cause"* .

Existen dos tipos de modelos de Pod que podemos crear:

- **Un contenedor por Pod.**  Es el modelo habitual y nos proporciona un alto desacoplamiento de los diferentes servicios por lo menos a nivel operacional.
- **Multi-contenedor por Pod.** En este modelo, un Pod puede contener múltiples contenedores que suelen estar estrechamente acoplados para compartir recursos. Estos contenedores funcionan como una sola unidad de servicio. Un ejemplo puede ser el uso de sidecars, proxies o registros.



A la hora de desplegar el Pod en el clúster de Kubernetes podemos optar por una vía imperativa en la que lo desplegamos directamente o declarativa mediante un fichero tipo **yaml** en el que le decimos al clúster como queremos que despliegue nuestro Pod. La primera opción suele estar bien para entornos de desarrollo pero no es buena opción para producción. 

![image-20200513144516694](./assets/Deply_aplication_Pod.png)



## Desplegando nuestro primer Pod.

Comenzamos desplegando nuestro primer Pod de forma imperativa, simplemente usando el modificador **"run"** de la siguiente forma:

`kubectl run webserver --image=nginx`{{execute}}

Vemos en la salida, que esta forma de ejecutar un Pod está en *DEPRECATED* ya que la opción que usa por defecto no sólo nos esta creando un Pod sino que lo esta asociando a un Deployment. Una entidad superior a la que le podemos aplicar otro tipo de controllers orientados a especificar el comportamiento sistémico del Pod dentro de nuestro clúster.

Podemos ver el Pod que hemos creado y  el deployment mediante:

`kubectl get pods`{{execute}}

`kubectl get deployments`{{execute}}

`kubectl get po,deploy`{{execute}}

Si queremos ver información más detallada usaremos el modificador *"-o wide":

`kubectl get pods -o wide`{{execute}}

Imaginemos que no queremos que nos genere el Deployment y simplemente queremos que nos despliegue el Pod. Para ello usaríamos la siguiente sintaxis:

`kubectl run --generator=run-pod/v1 webserver2 --image=nginx`{{execute}}

Comprobamos que lo ha hecho correctamente:

`kubectl get pods`{{execute}}

`kubectl get deployments`{{execute}}

Una buena forma de ver la diferencia entre los dos tipos de despliegue es probar a borrar los pods. En el caso del que tiene un deployent asociado, veremos como se reinicia y en el otro simplemente se para. 

```bash
kubectl delete pod $NAME
```



## Propiedades del Pod.

Para obtener información detallada del Pod vamos usar **"describe"**.  Este modificador necesita que le pasemos el tipo de objeto y el nombre del recurso:

`kubectl get pods`{{execute}}

`kubectl describe pod webserver`{{execute}}

En este caso nos debería de mostrar toda la información referente a los dos pods, ya que los dos empiezan por *webserver*.



## Ejecutar comandos dentro del Pod.

Igual que en los entornos Docker, en Kubernetes podemos lanzar comandos contra los pods. Por ejemplo, en este caso vamos a sacar la configuración que tiene el nginx del webserver2:

`kubectl exec webserver2 cat /etc/nginx/nginx.conf`{{execute}}

Por su puesto, también podemos entrar en modo interactivo para inspeccionar el contenedor que se esta ejecutando:

`kubectl exec webserver2 -it bash`{{execute}}

`hostname`{{execute}}

`cat /etc/nginx/nginx.conf`{{execute}}

Para salir de la consola:

`exit`{{execute}}



## Viendo los logs de un Pod.

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



## Kubectl proxy

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

`curl http://localhost:8080/api/v1/namespaces/default/pods/webserver2/proxy`{{execute}} 

Aunque esta es una posibilidad, no es la óptima. Veremos más adelante como los servicios nos ayudarán a hacer esto de forma bastante más sencilla.



## Borrar Pods.

Para borrar los Pods usaremos el modificador **delete** al que le tendremos que pasar el tipo de recurso y el nombre del mismo. Vamos a ver un ejemplo:

Inicialmente mostramos los Pods desplegados:

`kubect get pods`{{execute}}

Borramos los Pods:

`kubectl delete pod apache`{{execute}}

`kubectl delete pod webserver2`{{execute}}

`kubectl delete deploy webserver`{{execute}}