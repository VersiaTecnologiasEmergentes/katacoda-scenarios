
# Redes

**CONTENIDO**

- Redes en Docker.

- Primeras Pruebas

- Pruebas con las redes.

- Red HOST

- Configurando el NAT para redireccionar la conectividad externa.

- Redes en Docker-compose

  

## Redes en Docker

La implementación nativa de Docker nos proporciona los elementos básicos para la creación de las redes y por tanto el establecimiento de las comunicaciones contenedor-contenedor y contenedor-host. Cuando el proceso Docker se ejecuta, configura una nueva interfaz puente virtual llamada docker0 en el sistema host. Esta interface permite a Docker crear una sub-red virtual que usaran los contenedores que se ejecuten.

`ifconfig`{{execute}}

Cuando un contenedor se inicializa en el sistema, crea una nueva interfaz virtual a la que se le asigna una IP del rango de la subred docker0. Docker configurará de forma automática las reglas de iptables necesarias para redirigir y configurar la mascara NAT para el tráfico originado en el interfaz docker0 hacia el resto de redes.

La red que configura Docker es la 172.17.0.xx, el propio servidor docker es 172.17.0.1 y cada contenedor corriendo adquiere un IP posterior al último (172.117.0.2-254).

Para comenzar el laboratorio levantaremos un pequeño contenedor en segundo plano, mediante:

`docker run -id ubuntu /bin/bash`{{execute}}

Al arrancar el contenedor podremos averiguar cual es la IP asignada con el siguiente comando:

`CONTAINER=$(docker ps -qa)`{{execute}}

`docker inspect --format '{{ .NetworkSettings.IPAddress }}' $CONTAINER`{{execute}}

Existen tres redes preconfiguradsa en Docker:

1. **Bridge.** La red standard que usarán todos los contenedores.
2. **Host.** El contenedor usará el mismo IP del servidor real que tengamos.
3. **None.** Se utiliza para indicar que un contenedor no tiene asignada una red.

Para ver el listado de redes que tenemos creadas:

`docker network list`{{execute}}

Podemos ver las características de cualquiera de ellas con *network inspect*:

`docker network inspect bridge`{{execute}}



## Primeras pruebas

Ahora vamos a empezar a jugar un poco más con las redes. Vamos a crear una red test que use de driver Bridge.

`docker network create test`{{execute}}

`docker network ls`{{execute}}

Podemos revisar sus carecterísticas con:

`docker network inspect test`{{execute}}

SI queremos ver como responde el Gateway de la nueva red:

`ping 172.18.0.1 -c 3`{{execute}}

Vamos a continuar ejecutando una instancia de Docker nueva asociándole directamente la nueva red:

`docker run -di -P --name vmlab1 --network test ubuntu`{{execute}}

`docker ps -a`{{execute}}

`docker network inspect test`{{execute}}

Ahora tenemos al contenedor en nuestra red de test. Si queremos que tenga otro interfaz en la red de bridge y que se pueda ver con esos contenedores podemos seguir los siguientes pasos:

`docker network connect --link vmlab1:red2 bridge vmlab1`{{execute}}

docker inspect b34311275bc4

De la misma forma podemos desconectarlo de la red:

`docker network disconnect bridge vmlab1`{{execute}}

docker inspect b34311275bc4



Para limpiar el entorno, bastará con que paremos el Docker lo eliminemos y borremos las redes.

`docker stop $(docker ps -qa)`{{execute}}

`docker rm $(docker ps -qa)`{{execute}}



## Pruebas con las redes





## Red Host



## Configurando el NAT para redireccionar la conectividad externa.





## Redes en docker-compose



