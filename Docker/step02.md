
# Imágenes en Docker - I

En Docker, uno de los conceptos básicos es el de **imagen**. Una imagen de docker es una especie de plantilla de solo lectura que permite describir un entorno que pueda ser ejecutado en un **contenedor**.

Podemos entender una **imagen** como una instantánea de un contenedor. Las imágenes son inmutables, y podemos ejecutar varias instancias de ellas en diferentes contenedores.

En este apartado, veremos como ejecutar una imagen "base". Más adelante, veremos como extender una imagen base, y crear nuestras propias plantillas.

El entorno de *katakoda* nos da algunas imágenes por defecto. Podemos observarlas mediante:

`docker images`{{execute}}

![Default images](./assets/default_images.png)

Este listado nos ofrece información básica sobre las imágenes descargadas en nuestro sistema (repositorio oficial, etiqueta, ID, fecha de creación y tamaño).


## Descargar una imagen

A continuación, vamos a descargar una imagen base de [Docker Hub](https://hub.docker.com/). *Docker Hub* es un registro de imágenes de Docker que permite compartir estas plantillas. Existen registros públicos y privados, tanto de forma local como remota, para almacenar y distribuir imágenes.

La primera imagen base que utilizaremos es la de [hello world](https://hub.docker.com/_/hello-world). Si accedemos al enlace, podemos ver información básica de la plantilla, así como copiar el comando para descargarnos la imagen a nuestro sistema:

`docker pull hello-world`{{execute}}

Podemos comprobar que la imagen está descargada en nuestro sistema listando nuevamente las imágenes mediante el comando utilizado previamente:

`docker images`{{execute}}

![New hello-world image](./assets/hello-world_image.png)

Cuando descargamos una imagen, dicha plantilla está presente en nuestro sistema, pero no se está ejecutando. Podemos utilizar el comando `docker ps` para comprobarlo:

`docker ps`{{execute}}

![Running images](./assets/docker_ps.png)

**Nota**: Los nombres de los contenedores se generan de forma automática (a no ser que se especifique un nombre de contenedor concreto). Por tanto, es posible que los nombres mostrados en la imagen no se correspondan con los de vuestro sistema.


## Ejecutar una imagen

Ahora, es el momento de ejecutar una instancia de la imagen descargada. El comando `docker run NOMBRE_IMAGEN` crea un contenedor con una instancia de la plantilla referenciada.

`docker run hello-world`{{execute}}

![Run hello-world](./assets/docker_run_hello-world.png)

Si ejecutamos nuevamente el comando `docker ps` no veremos ningún contenedor ejecutando nuestra imagen. Esto es porque la imagen *hello-world* finaliza su ejecución una vez muestra la salida por pantalla (no es una imagen que permanezca en ejecución de fondo). Podemos utilizar el parámetro `-a` para observar todos los contenedores (estén en ejecución actualmente o no).

`docker ps -a`{{execute}}

![Docker ps -a (1)](./assets/docker_ps_-a_1.png)

Si volvemos a ejecutar la imagen (`docker run hello-world`), nos creará una nueva instancia en un nuevo contenedor.

`docker run hello-world`{{execute}}
`docker ps -a`{{execute}}

![Docker ps -a (2)](./assets/docker_ps_-a_2.png)

Si queremos volver a ejecutar o levantar un contenedor que se encuentra actualmente parado, podemos hacerlo mediante el comando `docker container start|restart CONTAINER_ID` (no es necesario especificar el ID completo del contenedor, normalmente con los 3-4 primeros caracteres ya se identifican de forma unívoca):

`docker container start 2dc`{{execute}}
`docker ps -a`{{execute}}

![Docker ps -a (3)](./assets/docker_ps_-a_3.png)

## Destrucción de imágenes

Cuando no queramos seguir almacenando una imagen en nuestro sistema (bien sea porque es defectuosa, vamos a sustituirla o no queremos que siga ocupando espacio de almacenamiento), podemos proceder a su eliminación mediante el comando `docker rmi`.

`docker rmi hello-world`{{execute}}

*Nota*: El parámetro para `rmi` puede ser tanto el nombre de la imagen como el ID (o sus primeros caracteres). Para más opciones, basta con consultar `docker rmi --help`

En caso de que la imagen tenga un contenedor asociado que la está ejecutando docker nos informará de ello y no nos permitirá borrarla inicialmente.