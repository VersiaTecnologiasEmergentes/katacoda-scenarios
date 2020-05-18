# Volúmenes

**Contenido**

- Descripción de Volumen en Docker y Ventajas.

- Volumenes desde linea de comandos.

- Volúmenes desde Dockerfile.

## Descripción de Volumen en Docker y Ventajas.
Por defecto, todos los ficheros creados dentro de un contenedor son almacenados en su capa de escritura, lo que significa que:

* Los datos no persisten cuando el contenedor deja de existir y puede ser complicado extraer los datos del contenedor si otro proceso lo necesita.
* La capa de escritura de un contenedor está estrechamente aclopada a la máquina host donde se ejecuta el contenedor, por lo que puede ser complicado mover los datos a otro lugar fuera del host.
* La escritura en la capa de escritura del contenedor requiere de un controlador de almacenammiento para administrar el sistema de archivos. Este controlador proporciona un sistema de archivos utilizando el kernel de Linux. Esta abstracción reduce el rendimiento en comparación con el uso de volúmenes de datos, que escriben directamente en el sistema de archivos host.

Docker proporciona dos opciones para que los contenedores almacenen archivos en el equipo Host, de modo que los archivos se conserven incluso después que el contenedor se detenga: **_Volumes_** y **_bind mounts_**. Además, si estamos ejecutando Docker en Linux, también podemos utilizar **_tmpfs mount_**. Si estamos utilizando Docker en Windows también podemos utilizar **_named pipes_**.

## Agregar volúmenes en docker-compose
