
# Limpieza

**CONTENIDO**

- Imágenes
- Contenedores
- Volúmenes
- Redes
- Sistema

En este apartado mostraremos algunos de los comandos más útiles para limpiar nuestros sistemas y deshacernos de los componentes que ya no utilizamos o que pueden estar ocasionando conflictos.



## Imágenes

Para eliminar aquellas imágenes que no nos resulten útiles, podemos emplear el siguiente comando:

`docker rmi IMAGEN_1 ... IMAGEN_n`

*Nota*: Las imágenes pueden ser referenciadas tanto por su nombre como por su ID (completo o primeros caracteres). Para más opciones, basta con consultar `docker rmi --help`

En caso de que alguna imagen tenga un contenedor asociado que la está ejecutando, docker nos informará de ello y no nos permitirá borrarla inicialmente.

Finalmente, podemos eliminar **todas** las imágenes concatenando comandos mediante:

`docker rmi $(docker images -a -q)`

Esta llamada ejecuta el comando `rmi` pasándole como parámetros los IDs (`-q`) de todas las imágenes (`docker images -a`) en el sistema.



## Contenedores

De forma análoga a `rmi` (remove images), el comando `rm` nos permite eliminar contenedores del sistema:

`docker rm CONTENEDOR_1 ... CONTENEDOR_n`

También podemos emplear una fórmula similar para eliminar **todos** los contenedores:

`docker rm $(docker ps -a -q)`



## Volúmenes

Para eliminar volúmenes, basta con indicar el nombre de aquellos que queremos borrar:

`docker volume rm VOLUMEN_1 ... VOLUMEN_n`

Existe también un atajo para borrar **todos** los volúmenes con un solo comando:

`docker volume prune`

Opcionalmente, al borrar un contenedor, podemos borrar su volúmen relacionado utilizando el modificador `-v` al ejecutar el comando.



## Redes

Por último, las redes creadas también pueden ser eliminadas mediante:

`docker network rm RED_1 ... RED_n`

O bien borrar **todas** las redes del sistema con:

`docker network prune`



## Sistema

Agrupando los comandos de limpieza vistos en los puntos anteriores, podemos deshacernos de todos los componentes que han quedado *descolgados* ejecutando:

`docker system prune`

Antes de proceder a su eliminación, realizará una comprobación de si realmente queremos ejecutar la acción. También podemos forzar la eliminación de **todas** las imágenes y contenedores (descolgados y no usados) invocando:

`docker system prune -a`
