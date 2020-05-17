# Persistent volume.

Vamos a ver el equivalente de los volumens de docker en Kubernetes. En el ejemplo anterior hemos asignado directamente al Pod los volumenes que queríamos. Ahora lo vamos a hacer mediante un volumen persistente. Para crearlo usaremos el siguiente yaml:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-volume
  labels:
    type: local
spec:
  storageClassName: sistemaficheros
  capacity:
    storage: 2Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/root/data"
```

Creamos el directorio del volumen:

`mkdir data`{{execute}}

Para crear nuestro pv:

`kubectl apply -f kubernetes_101_lab/volumenes/lab2/pv.yaml`{{execute}}

Podemos ver si se ha creado correctamente mediante:

`kubectl get pv`{{execute}}



## Persistem Volume Claim.

Ahora vamos a crear el Persisten Volume Claim, que es la forma que tiene el Pod de reclamar el Persistent Volume que hemos creado antes. EL yaml que vamos a usar es este:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-claim
spec:
  storageClassName: sistemaficheros
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Para aplicarlo:

`kubectl apply -f kubernetes_101_lab/volumenes/lab2/pvclaim.yaml`{{execute}}

Para ver los claims que tenemos:

`kubectl get pvc`{{execute}}



## Creación del Pod

Ya vamos teniendo todas las piezas y sólo nos falta asociar al pod el reclaim del volumen que hemos creado. Para ello creamos el siguiente yaml:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
spec:
  volumes:
    - name: pv-storage
      persistentVolumeClaim:
        claimName: pv-claim
  containers:
    - name: task-pv-container
      image: nginx
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: pv-storage
```

Para desplegarlo:

`kubectl apply -f kubernetes_101_lab/volumenes/lab2/pod-pvc.yaml`{{execute}}

`kubectl get pod`{{execute}}

`kubectl describe pod pv-pod`{{execute}}



