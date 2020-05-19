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

Vamos agregar al directorio creado un nuevo archivo de texto que, a posteriori, podremos utilizar desde el contenedor:

`echo -e "Prueba de Persistencia de Volumenes" >> /labdirectory/VolumesTestLine.txt`{{execute}}

Ahora vamos a lanzar un nuevo contenedor con un Volumen apuntando al directorio "labdirectory" del host y a "containerlab" en el contenedor:

`docker run -it --name lab-test -v /labdirectory:/containerlab ubuntu /bin/bash`{{execute}}

Una vez en la consola del contenedor, comprobamos si estamos viendo los contenidos del directorio del host:

`ls /containerlab/`{{execute}}

Como podemos ver, tenemos acceso al archivo desde el contenedor aunque, físicamente, el archivo está alojado en el host. Todos los cambios que realicemos sobre el directorio desde el contenedor se verán reflejados en el host.

Por último, salimos del contenedor:

`exit`{{execute}}

## Volumenes desde Dockerfile.
Al igual que desde consola, podemos crear o utilizar volumenes a través de los ficheros de tipo Dockerfile para la creación de Imágenes. La cláusula a utilizar en los ficheros Dockerfile para la utilización de volúmenes es **_VOLUME_** seguido del path al que lo queremos vincular.

Veamos un ejemplo creando un nuevo archivo Dockerfile al que agregaremos un volumen. Lo primero que hacemos es crear el archivo Dockerfile:

`cat > Dockerfile`{{execute}}

Agregamos la imagen base que utilizaremos para crear el contenedor:

`FROM ubuntu`{{execute}}

Agregamos a la configuración nuevo volumen:

`VOLUME /dockerfilevollab`{{execute}}

Guardamos el archivo pulsando "Ctrl+D".

Ahora que ya tenemos creado el archivo Dockerfile construimos la imagen con el nombre "dockerfile-volumelab":

`docker build -t dockerfile-volumelab .`{{execute}}

Comprobamos que la nueva imagen se haya generado correctamente:

`docker image ls`{{execute}}

Lanzamos contenedor a partir de nueva imagen:

`docker run --name lab-dockerfile -it dockerfile-volumelab /bin/bash`{{execute}}

Creamos nuevo archivo desde contenedor sobre el volumen configurado:

`echo "Lab Volumenes" >> /dockerfilevollab/file-from-container-lab.txt`{{execute}}

Comprobamos que el nuevo archivo se a creado correctamente:

`ls /dockerfilevollab/`{{execute}}

Salimos del contenedor:

`exit`{{execute}}

Ahora vamos a comprobar que el archivo efectivamente existe en el host. Para determinar cual es la hubicación que Docker le otorga al Volumen podemos inspeccionar el contenedor para comprobar dicho path:

`docker inspect lab-dockerfile`{{execute}}

Para capturar la ruta asignada tendremos que localizar la sección "Mounts" y copiar la propiedad "Source". Si listamos el contenido del path copiado vemos el fichero creado desde el contenedor.

```bash
$ ls /var/lib/docker/volumes/268e7e4aaa5591f21572ffbb21f4de55e393bddbfa9bd6e1400fd1bde7c6575d/_data
file-from-container-lab.txt
```
