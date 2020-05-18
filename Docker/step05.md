
# Redes

**CONTENIDO**

- Redes en Docker
- Primeras pruebas
- Pruebas con las redes
- Red HOST
- Configurando el NAT para redireccionar la conectividad externa



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

Si queremos ver como responde el Gateway de la nueva red:

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

`CONTAINER2=$(docker ps |grep vmlab1| cut -d " " -f1)`{{execute}}

`docker inspect $CONTAINER2`{{execute}}

Para limpiar el entorno, bastará con que paremos el Docker lo eliminemos y borremos las redes.

`docker stop $(docker ps -qa)`{{execute}}

`docker rm $(docker ps -qa)`{{execute}}

`docker network prune -f`{{execute}}



## Pruebas con las redes

Vamos a crear un red llamada *labnet* y posteriormente levantaremos dos contenedores para hacer algunas pruebas.

Creamos la nueva red:

`docker network create --driver bridge labnet`{{execute}}

`docker network ls`{{execute}}

Levantamos dos contenedores:

`docker run -dit --rm --name vlab1 --network labnet alpine sh`{{execute}}

`docker run -dit --rm --name vlab2 --network labnet alpine sh`{{execute}}

`docker ps -a`{{execute}}

Si ahora ejecutamos un inspect sobre la red labnet veremos las redes asignadas a nuestros dos contenedores.

`docker network inspect labnet`{{execute}}

Para comprobar que tenemos red entre ellos podemos hacer attach a uno de los contenedores y probar.

`docker attach vlab1`{{execute}}

`ping -c 3 vlab2`{{execute}}

`exit`{{execute}}

Al salir, katakoda mata el container, por lo que debemos levantarlo otra vez. En un entorno normal saldríamos de la consola con "^q^p"

`docker run -dit --rm --name vlab1 --network labnet alpine sh`{{execute}}

Vamos a complicarlo un poco más creando otra red y levantaremos un contenedor en esta nueva red.

`docker network create --driver bridge labnet2`{{execute}}

Comprobamos que la red se ha creado correctamente:

`docker network list`{{execute}}

`docker run -dit --rm --name vlab3 --network labnet2 alpine sh`{{execute}}

Entramos en esta nueva máquina y comprobamos que no llegamos por ping ni a vlab1 ni a vlab2 ya que están en otra red.

`docker attach vlab3`{{execute}}

`ping -c 2 vlab2`{{execute}}

`ping -c 2 vlab1`{{execute}}

`exit`{{execute}}

`docker run -dit --rm --name vlab3 --network labnet2 alpine sh`{{execute}}

Vamos a conectar vlab3 a la red labnet y comprobar que llemaos al resto de instancias.

`docker network connect labnet vlab3`{{execute}}

`docker attach vlab3`{{execute}}

`ping -c2 vlab1`{{execute}}

`ping -c2 vlab2`{{execute}}

`exit`{{execute}}

`docker run -dit --rm --name vlab3 --network labnet2 alpine sh`{{execute}}



Para terminar vamos a borrar todo para dejar el entorno lo más limpio posible.

`docker stop $(docker ps -qa)`{{execute}}

`docker network prune -f`{{execute}}



## Red HOST

Si conectamos un contenedor a la red host estaremos exponiendo los servicios directamente desde el anfitrión. Para hacer una prueba rápida usaremos un contenedor *nginx* de la siguiente forma.

Lanzamos la instncia del docker nginx en la red host:

`docker run -d --rm --name nginx1 --network host nginx`{{execute}}

Comprobamos que se ha levantado correctamente

`docker ps -a`{{execute}}

Probamos desde la máquina anfitrion:

`curl localhost`{{execute}}

Limpiamos para terminar esta prueba:

`docker stop $(docker ps -qa)`{{execute}}




## Configurando el NAT para redireccionar la conectividad externa.

Vamos a ver como hacer NAT con los puertos de la red host hacia una estancia determinada.

Creamos la instancia de nginx redirigiendo el puerto 8080 del host anfitrion al 80 de la instancia:

`docker run -d --rm --name nginx1 -p 8080:80 nginx`{{execute}}

`docker ps -a`{{execute}}

Comprobamos que llegamos a través del host anfitrion por el 8080:

`curl localhost:8080`{{execute}}

Comprobamos la ip asignada a la instancia para comprobar que llegamos directamente por el puerto 80:

`docker network inspect bridge`{{execute}}

Hacemos: curl IP_ASIGNADA:80



Para terminar limpiamos el entorno:

`docker stop $(docker ps -qa)`{{execute}}
