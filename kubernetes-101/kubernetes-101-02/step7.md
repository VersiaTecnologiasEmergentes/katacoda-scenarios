## Ficheros Yaml. Introducción.

Hasta ahora hemos estado hablando de cómo trabajar con Kubernetes exclusivamente desde la línea de comando, pero esa una es una buena forma para entornos productivos y es más recomendable es uso de ficheros **Yaml** *(Another Markup Language)*.  Es un formato basado en texto legible por humanos para especificar información de tipo de configuración. El uso de este tipo de ficheros para las definiciones de K8 le brinda una serie de ventajas, que incluyen:

- **Convivencia**. Evitamos tener comandos con parámetros inmanejables. 
- **Mantenimiento.** Los archivos se pueden agregar a nuestro gestor de código, con lo que conseguimos todos los beneficios que nos proporciona este tipo de gestión.
- **Flexibilidad.** Permitirá crear estructuras mucho más complejas y sencillas de entender.

Yaml es un subconjunto de JSON, lo que significa que cualquier archivo JSON válido también es un archivo Yaml válido.  No es habitual que nos encontremos ficheros en JSON, pero podría ser posible. 

El mundo de Yaml es amplio, pero para el universo de Kubernetes nos bastará con conocer **List** y **Maps**. No quiere decir que no podamos hacer cosas más complejas, pero estas dos serán la base.

**Mapas Yaml** Los mapas nos permiten asociar pares de nombre-valor, cosa que nos viene muy bien para definir configuraciones. Un ejemplo sencillo:

```yaml
---
apiVersion: v1
Kind: Pod
```

También podemos especificar estructuras un poco más complicadas, creando una clave que se asigne a otro mapa, en lugar de una cadena, como en el siguiente ejemplo:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
	name: webserver
	labels:
		app: web
```

En este caso, tenemos una clave, metadatos, que tiene como valor un mapa con 2 claves más, name y labels.  El procesador Yaml sabe cómo se relacionan las piezas usando el sangrado de líneas. Se suelen usar   dos espacios, pero la cantidad de espacios no es relevante mientras que siempre usemos la misma regla.

**Listas Yaml** Las listas de YAML son literalmente una secuencia de objetos. Por ejemplo:

```yaml
args:
	- sleep
	- "1000"
	- message
	- "Bring back Firefly"
```

por su puesto, los miembros de una lista pueden ser también mapas:

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: webserver
  labels:
    app: web
spec:
  containers:
    - name: front-end
      image: nginx
      ports:
        - containerPort: 80
    - name: rss-reader
      image: nginx
      ports:
        - containerPort: 90
```

Como podemos ver aquí, tenemos una lista de contenedores "objetos", cada uno de los cuales consiste en un nombre, una imagen y una lista de puertos. Cada elemento de la lista debajo de los puertos es en sí mismo un mapa que enumera el containerPort y su valor.