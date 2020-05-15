
# Agrupando contenedores: docker-compose

En nuestra operativa diaria, a menudo nos encontramos con arquitecturas complejas e interconectadas que no se solucionan con la creación de un solo entorno.

Son estas situaciones las que nos permiten dar el siguiente paso en el ecosistema Docker: **Docker compose**.


## Docker-compose

Docker compose es una herramienta para la definición y ejecución de aplicaciones multi-contenedor.

A través de un fichero [YAML](https://yaml.org/), podemos definir los servicios que interactúan entre sí para definir nuestra arquitectura, así como las dependencias entre ellos y la configuración para su ejecución.

El primer acercamiento lo podemos realizar mediante un fichero que ejecute la imagen creada en el paso anterior, recuperándola del registro de Docker Hub.

Para ello, crearemos un fichero llamado **docker-compose.yml** (la extensión *.yaml* también es válida) y copiaremos en él el siguiente contenido (es **muy importante** respetar el espaciado, ya que *YAML* es un formato basado en la tabulación con espacios):

```
version: '3'

services:

  django_webapp:
    image: oscarpdr/webapp:0.1
    ports:
      - 8000:8000
    container_name: django_webapp
```

A continuación, ejecutamos el comando `docker-compose up` para crear un contenedor con una instancia de la imagen *webapp*. En caso de que tengamos varios ficheros docker-compose, podemos indicar el deseado con el modificador `docker-compose -f NOMBRE_FICHERO up`. Todos los parámetros pueden ser consultados a través de `docker-compose --help`. También podemos ejecutar el proceso en segundo plano, disponiendo de nuevo de la consola.

`docker ps`{{execute}}
`docker-compose up -d`{{execute}}
`docker ps`{{execute}}

![docker-compose up](./assets/docker-compose_up.png)

Podemos observar que la imagen se ha descargado del registro para instanciarla dentro de un contenedor al que hemos denominado `django_webapp`. Si queremos consultar sus *logs* (al menos aquellos redirigidos a la salida estándar), como si hubiéramos ejecutado el comando sin el modificador `-d`, podemos emplear el siguiente comando:

`docker-compose logs`{{execute}}

Para parar el contenedor en ejecución y eliminarlo podemos utilizar el comando `docker-compose down`, el cual buscará todos los servicios referenciados en nuestro fichero *docker-compose.yml*, parará sus contenedores y los eliminará. Las imágenes continúan descargadas en nuestro sistema.

`docker-compose down`{{execute}}

![docker-compose down](./assets/docker-compose_down.png)


## Gestión docker-compose
