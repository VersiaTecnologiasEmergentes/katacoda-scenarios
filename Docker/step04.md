
# Volúmenes

**CONTENIDO**

- Introducción
- Creación de un volúmen


## Introducción

El sistema de ficheros de un contenedor docker se almacena en su capa de escritura, la cual puede ser accedida si ejecutamos una instancia en modo interactivo, permitiéndonos leer y escribir datos de dicho sistema.

No obstante, esto nos plantea algunas problemáticas:

- El sistema de ficheros solo tiene validez mientras el contenedor existe, perdiéndose una vez este es borrado.
- Inicialmente, los datos solo son accesibles por el propio contenedor.

En los entornos reales (de desarrollo y producción), normalmente necesitamos persistir información de los contenedores en ejecución, así como compartir su acceso (y posible modificación) por parte de otros contenedores que formen parte de nuestra arquitectura (bases de datos, registros comunes, etc).

Es en estas situaciones en las que recurriremos a los volúmenes de Docker.

Existen dos modos principales de persistir datos en el ecosistema Docker:

- **Volumes**: El modo por defecto para persistir datos en Docker. Mediante los volúmenes, los datos se almacenan en una partición del sistema de archivos del host (por defecto, en entornos *Linux* dentro de `/var/lib/docker/volumes/`), y solo pueden ser accedidos por procesos Docker.
- **Bind mounts**: Pueden ser almacenados en cualquier directorio del sistema host, y su gestión no queda limitado simplemente a los procesos Docker.

Existen otras opciones como **tmpfs**, cuyos datos solo están presentes en la memoria del sistema host y no se persisten como tal.

La principal diferencia a la hora de seleccionar *volumes* o *bind-mounts* radica en la existencia previa de una determinada estructura de almacenar la información en el sistema host. De forma generalizada:

- Preferiremos el uso de *volumes* cuando queramos compartir datos entre diversos contenedores (con diversos permisos de escritura/lectura, así como accesos simultáneos, etc). Al montar el volúmen por primera vez en un contenedor, este se crea si no existe previamente. También facilita los procesos de *back-up* y recuperación de información. Finalmente, los volúmenes pueden ser empleados en diversos sistemas operativos hosts, así como en servicios en la nube, ser cifrados, etc.
- Favoreceremos el uso de *bind-mounts* cuando compartamos ficheros de configuración del sistema host al contenedor (por ejemplo, resolución de *DNS*). También cuando la estructura de ficheros y directorios sea constante en las máquinas hosts (siguiendo siempre los mismos patrones y *paths*).


## Creación de un volúmen

A la hora de crear un volúmen en Docker, podemos escoger un nombre predeterminado para el mismo, o dejar que Docker le asigne uno de forma automática. De optar por la primera vía (para este ejemplo utilizaremos el nombre `my_data`), el comando que utilizaremos es:

`docker volume create my_data`{{execute}}

Esto creará una carpeta en nuestro sistema host, en la ruta `/var/lib/docker/volumes/my_data/`.

`ll /var/lib/docker/volumes/my_data/`{{execute}}

![Directorio creado para nuestro volumen](./assets/ll_docker_volume.png)

Podremos inspeccionar las propiedades de este volúmen a través de:

`docker volume inspect my_data`{{execute}}

![Propiedades del volúmen *my_data*](./assets/docker_volume_inspect.png)

Para consultar los volúmenes existentes, basta con:

`docker volume ls`{{execute}}

A continuación, arrancaremos un contenedor que utilice este volúmen, mediante el modificador `-v` de la ejecución de una imagen:

`docker run -it --name ubuntu-volume -v my_data:/data ubuntu bash`{{execute}}

`ll`{{execute}}

`cd /data/`{{execute}}

![Volúmen *my_data* en contenedor](./assets/docker_volume_run.png)

Los archivos creados en este directorio, se persistirán al sistema host, en la ruta por defecto `/var/lib/docker/volumes/my_data/_data/`:

`echo "Persistiendo datos" >> fichero.txt`{{execute}}

`cat fichero.txt`{{execute}}

Si paramos el contenedor (`Ctrl + D`), podremos comprobar que los datos han sido mapeados al sistema de ficheros del host:

`cat /var/lib/docker/volumes/my_data/_data/fichero.txt`{{execute}}

Finalmente, podremos deshacernos de un volúmen concreto mediante `docker volume rm`:

`docker volume rm my_data`{{execute}}

Si el volúmen está en uso por algún contenedor en ejecución, Docker nos informará de ello y no permitirá su eliminación hasta que el contenedor se detenga:

`docker stop ubuntu-volume`{{execute}}

`docker rm ubuntu-volume`{{execute}}

`docker volume rm my_data`{{execute}}

Una vez eliminado, podemos comprobar que ya no aparece listado en los volúmenes del sistema:

`docker volume ls`{{execute}}





////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////





**Contenido**

- Descripción de Volumen en Docker.

- Volumenes desde linea de comandos.

- Volúmenes desde Dockerfile.

## Descripción de Volumen en Docker.
Por defecto, todos los ficheros creados dentro de un contenedor son almacenados en su capa de escritura, lo que significa que:

* Los datos no persisten cuando el contenedor deja de existir y puede ser complicado extraer los datos del contenedor si otro proceso lo necesita.
* La capa de escritura de un contenedor está estrechamente aclopada a la máquina host donde se ejecuta el contenedor, por lo que puede ser complicado mover los datos a otro lugar fuera del host.
* La escritura en la capa de escritura del contenedor requiere de un controlador de almacenammiento para administrar el sistema de archivos. Este controlador proporciona un sistema de archivos utilizando el kernel de Linux. Esta abstracción reduce el rendimiento en comparación con el uso de volúmenes de datos, que escriben directamente en el sistema de archivos host.

Docker proporciona dos opciones para que los contenedores almacenen archivos en el equipo Host, de modo que los archivos se conserven incluso después que el contenedor se detenga: **_Volumes_** y **_bind mounts_**. Además, si estamos ejecutando Docker en Linux, también podemos utilizar **_tmpfs mount_**. Si estamos utilizando Docker en Windows también podemos utilizar **_named pipes_**.

## Volumenes desde linea de comandos.
El comando que utilizaremos desde linea de comandos para la creación de volúmenes es "docker volume create". Un punto a tener en cuenta es darle un nombre que permita, a posteriori, localizar y asignar Volúmnes de Docker a contenedores.

Vamos a crear un nuevo volumen con el nombre "lab-volume":

`docker volume create --name lab-volume`{{execute}}

Para comprobar su correcta creación vamos a listar los volúmenes disponibles actualmente:

`docker volume ls`{{execute}}

Ahora que ya tenemos creado el nuevo volumen, ya podemos utilizarlo en contenedores. Vamos a crear un nuevo contenedor que haga uso del contenedor que acabamos de crear:

`docker run -it --name lab-volume-test -v lab-volume:/data ubuntu /bin/bash`{{execute}}

Todo lo que creemos sobre el directorio "/data" del contenedor será persistido en el volumen "lab-volume" y, mientras no eliminemos el volumen, no perderemos los datos existentes. Para comprobar esto, vamos a crear un nuevo archivo de texto en el directorio "/data":

`echo -e "Prueba de Persistencia de Volumenes" >> /data/VolumesTestLine.txt`{{execute}}

Comprobamos que el archivo está creado en el directorio "/data":

`ls /data`{{execute}}

Con el fin de poner a prueba la persistencia, vamos a salir del contenedor y a eliminarlo:

`exit`{{execute}}
`docker container rmi lab-volume-test`{{execute}}

////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////

`docker container rm lab-volume-test`{{execute}}

Volvemos a crear contenedor asociando el mismo volumen y comprobamos que sigue existiendo el fichero creado anteriormente:

`docker run -it --name lab-volume-test -v lab-volume:/data ubuntu /bin/bash`{{execute}}
`ls /data`{{execute}}

Salimos del contenedor y lo eliminamos:

`exit`{{execute}}
`docker container rm lab-volume-test`{{execute}}

Ahora vamos a inspeccionar el Volumen creado con el fin de ver sus características agregando "inspect" al comando "volume":

`docker volume inspect lab-volume`{{execute}}

Como podemos ver, entre otros datos, nos proporciona la ruta donde está alojado el volumen en el host.

Por último, vamos a eliminar el volumen:

`docker volume rm lab-volume`{{execute}}

En este punto tenemos que tener en cuenta que, si el Volumen a eliminar está siendo utilizado por algún contenedor, previamente tendremos que eliminar el contenedor ya que Docker no nos permitirá eliminarlo mientras esté en uso.

**Especificando un Directorio del Host**

Si necesitamos especificar un directorio concreto del host a la hora de crear un Volumen lo podemos realizar a través del argumento "-v". Vamos a crear un nuevo directorio en el host que nos sirva como base para la creación de un nuevo volumen:

`mkdir /labdirectory`{{execute}}