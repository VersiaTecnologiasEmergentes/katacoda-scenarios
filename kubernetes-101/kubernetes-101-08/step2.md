# Volúmenes locales.

Vamos a ver una forma de mapear algunos directorios locales en el los Pods. Esta no es una forma adecuada para hacerlo pero nos servirá para ir entendiendo un poco como funcionan los volumens.

Para esta primera prueba vamos a usar el sigueinte yaml:

`cat kubernetes_101_lab/volumenes/lab1/volume.yaml`{{execute}}

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volumenes
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /home
      name: home
    - mountPath: /git
      name: git
      readOnly: true
    - mountPath: /temp
      name: temp
  volumes:
  - name: home
    hostPath:
      path: /root/datos
  - name: git
    gitRepo:
      repository: https://github.com/vthot4/kubernetes_101_lab.git
  - name: temp
    emptyDir: {}$
```

Antes de hacer el despliegue vamos a crear el directorio datos, aunque no es necesario, porque si tiene permisos y no existe lo creará el durante la ejecución.

`mkdir datos`{{execute}}

Desplegamos la prueba:

`kubectl apply -f kubernetes_101_lab/volumenes/lab1/volume.yaml`{{execute}}

`kubectl get pods`{{execute}}

`kubectl describe pods volumenes`{{execute}}

Vemos que los volumenes están creados, pero vamos a ver dentro del Pod:

`kubectl exec -it volumenes bash`{{execute}}

`cd git`{{execute}}

`cd /home/`{{execute}}

Probamos el mapeo del directorio:

`touch prueba`{{execute}}

`exit`{{execute}}

`cd datos`{{execute}}

`ls -lar`{{execute}}

Para ver que los datos persisten:

`kubectl delete pod volume`{{execute}}

Esta no es una forma óptima porque al Pod se le asigna un directorio de la máqiuna local. En un clúster tendríamos que ver la forma de que la información estuvierá sincronizada entre todos los nodos :).



