# Volúmenes

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