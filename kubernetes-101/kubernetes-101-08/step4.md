# Creando un PersistentVolumen con NFS

En este ejemplo vamos a ver como podemos usar un servidor de NFS como proveedor de almacenamiento para nuestros Pods. Los pasos del laboratorio que vamos a realizar son:

1. Definir un PersistentVolumen.
2. Creando un PersistentVolumen con NFS.
3. Solicitud de almacenamiento en Kubernetes: PersistentVolumenClaims



## 1. Definir un PersistentVolumen.

Un PersistentVolumen es un objeto que representa los volúmenes disponibles en el clúster. Será en donde se va a definir los detalles del tipo de almacenamiento que vamos a utilizar, el tamaño disponible, los modos de acceso, las políticas de reciclaje, etc.

En un principio existen tres modos de acceso, que depende del tipo de almacenamiento que vayamos a utilizar:

- **ReadWriteOnce:** read-write solo para un nodo (RWO)

- **ReadOnlyMany:** read-only para muchos nodos (ROX)

- **ReadWriteMany:** read-write para muchos nodos (RWX)

Las políticas de reciclaje de volúmenes también depende del backend y son:

- **Retain:** Reclamación manual
- **Recycle:** Reutilizar contenido
- **Delete:** Borrar contenido



## 2. Creando un persistentVolumen con NFS.

Los primero que vamos a hacer es instalar el servidor NFS en la máquina local:

`apt install nfs-kernel-server -y`{{execute}}

Cremamos el directorio que exportaremos por NFS:

`mkdir -p /var/shared`{{exeute}}

Configuramos el fichero de NFS:

`echo "/var/shared *(rw,sync,no_root_squash,no_all_squash)" >> /etc/exports`{{execute}}

Para que el servicio coja los cambios, reiniciamos el servicio:

`systemctl restart nfs-kernel-server.service`{{execute}}

`showmount -e 127.0.0.1`{{execute}}

Ahora vamos a montar el directorio compartido:

`apt install nfs-common`{{execute}}

Y ya podemos montarlo:

`mkdir /var/data`{{execute}}

`mount -t nfs4 127.0.0.1:/var/shared /var/data`{{execute}}

`df |grep shared`{{execute}}



Ya tenemos todo preparado para crear el Volumen de Kubernetes. Para ello usaremos el fichero *nfs-pv.yaml*

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  nfs:
    path: /var/shared
    server: 127.0.0.1
```

`cat kubernetes_101_lab/volumenes/lab3/nfs-pv.yaml`{{execute}}

Lo creamos:

`kubectl create -f kubernetes_101_lab/volumenes/lab3/nfs-pv.yaml`{{execute}}

`kubectl get pv`{{execute}}

El tipo de volumen disponible lo vamos a referenciar con su nombre (nfs-pv), tiene 5Gb de capacidad, estamos utilizando NFS, el modo de acceso es RWX y su política de reciclaje es de reutilización del contenido.



## 3. Solicitud de almacenamiento en Kubernetes: PersistentVolumenClaims

A continuación si nuestro pod necesita un volumen, necesitamos hacer una solicitud de almacenamiento usando un objeto del tipo *PersistentVolumenCliams*.

Cuando creamos un PersistentVolumenCliams, se asignará un PersistentVolumen que se ajuste a la petición. Está asignación se puede configurar de dos maneras distintas:

- **Estática:** Primero se crea todos los PersistentVolumenCliams por parte del administrador, que se irán asignando conforme se vayan creando los PersistentVolumen.

- **Dinámica:** En este caso necesitamos un “provisionador” de almacenamiento (para cada uno de los tipos de almacenamiento), de tal manera que cada vez que se cree un PersistentVolumenClaim, se creará bajo demanda un PersistentVolumen que se ajuste a las características seleccionadas.


Siguiendo con el ejercicio anterior vamos a crear una solicitud de almacenamiento del volumen creado anteriormente con NFS. Definimos el objeto en el fichero nfs-pvc.yaml:

```yaml
piVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
```

`cat kubernetes_101_lab/volumenes/lab3/nfs-pvc.yaml`{{execute}}

Creamos el recurso:

`kubectl create -f kubernetes_101_lab/volumenes/lab3/nfs-pvc.yaml`{{execute}}

`kubectl get pv,pvc`{{execute}}

Como podemos observar al crear el *pvc* se busca del conjunto de *pv* uno que cumpla sus requerimientos, y se asocian (*status bound*) por lo tanto el tamaño indicado en el *pvc* es el valor mínimo de tamaño que se necesita, pero el tamaño real será el mismo que el del *pv* asociado.

Si queremos añadir un volumen a un pod a partir de esta solicitud, podemos usar la definición del fichero pod-nginx-pvc:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: www-vol
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
      - mountPath: /usr/share/nginx/html
        name: nfs-vol
  volumes:
    - name: nfs-vol
      persistentVolumeClaim:
        claimName: nfs-pvc
```

`cat kubernetes_101_lab/volumenes/lab3/pod-nginx-pvc.yaml`{{execute}}

Lo crearemos mediante:

`kubectl create -f kubernetes_101_lab/volumenes/lab3/pod-nginx-pvc.yaml`{{execute}}

`kubectl get pods`{{execute}}

`kubectl describe pods`{{execute}}



Podemos probar el nginx que hemos levantado de la siguiente forma:

`kubectl port-forward www-vol 8080:80 &`{{execute}}

`curl localhot:8080`{{execute}}

Evidentemente al montar el directorio DocumentRoot del servidor (/usr/share/nginx/html) en el volumen NFS, no tiene index.html, podemos crear uno en el directorio compartido del master y estará disponible en todos los nodos:

`echo "Espero que funcione ....." >> /var/shared/index.html`{{execute}}