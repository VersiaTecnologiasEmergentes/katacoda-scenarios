
# Limpieza

En este apartado mostraremos algunos de los comandos más útiles para limpiar nuestros sistemas y deshacernos de los componentes que ya no utilizamos o que pueden estar ocasionando conflictos.

# Imágenes

# Contenedores

# Volúmenes

# Redes

# Sistema

Agrupando los comandos de limpieza vistos en los puntos anteriores, podemos deshacernos de todos los componentes que han quedado *descolgados* ejecutando:

`docker system prune`

Antes de proceder a su eliminación, realizará una comprobación de si realmente queremos ejecutar la acción. También podemos forzar la eliminación de **todas** las imágenes y contenedores (descolgados y no usados) invocando:

`docker system prune -a`