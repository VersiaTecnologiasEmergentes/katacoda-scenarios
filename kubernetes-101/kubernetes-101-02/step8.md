## Crear un Pod usando Yaml.

vamos a levantar el mismo Pod que hemos usado en los ejemplos anteriores pero vamos a usar un yaml. Vamos a ver como podemos lanzar un pod o un deployment y los matices que presentan las diferentes opciones de despliegue.

El primer Yaml que vamos a usar es este:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
  labels:
    zone: prod
    version: v1
spec:
  containers:
   - name: nginx   
     image: nginx
```

Podemos crear el fichero o usar el que hemos bajado en repo del inicio del documento. Podemos verlo:

`cat kubernetes_101_lab/pod/lab1/nginx.yaml`{{execute}}

Explicamos algunas de las partes del fichero:

- **apiVersion.** Indicamos la versión que necesitamos.
- **kind.** Es donde indicamos el tipo de componente que queremos crear.
- **metadata.** Nos sirve para poner determinadas características al Pod. Más adelante veremos la utilidad de los labels.
- **spec.** Lugar donde definimos las especificaciones del Pod. Por ejemplo el nombre de la imagen que vamos a usar.

Existen numerosas etiquetas que no vamos a ver, pero que nos podemos ir encontrando en ejemplos y en la documentación.

Para crear nuestro primer Pod con un fichero *"yaml"* usaremos el modificador **create** de kubectl:

`kubectl create -f kubernetes_101_lab/pod/lab1/nginx.yaml`{{execute}}

Comprobamos que el Pod se ha creado correctamente:

`kubectl get pods`{{execute}}

`kubectl get deploy`{{execute}} 

Comprobamos que hemos creado un Pod y no un despliegue.

