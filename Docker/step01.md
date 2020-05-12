
# Comandos básicos

Lo primero que haremos será comprobar que Docker se encuentra instalado en nuestro sistema:

`docker --version`{{execute}}

Este comando visualiza la versión actualmente instalada de Docker. También podemos utilizar su versión abreviada y obtener el mismo resultado.

`docker -v`{{execute}}

Mediante el comando `info`, podemos acceder a información más detallada sobre Docker (versión, sistema operativo del host, hora del sistema, etc.):

`docker info`{{execute}}

Para obtener un listado completo de los comandos que podemos utilizar en docker, basta con ejecutar:

`docker --help`{{execute}}

Con el objetivo de conocer la ejecución y parámetros de cada comando, podemos emplear la fórmula `docker [COMANDO] --help`, por ejemplo:

`docker run --help`{{execute}}

nos permite conocer los modificadores del comando `run`.
