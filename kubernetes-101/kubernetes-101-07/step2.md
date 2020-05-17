# Creando/Borrando namespaces

## CREANDO NAMESPACES

Podemos crear namespaces mediante de dos formas:

- Mediante comandos (modo imperativo):

  `kubectl create namespace desarrollo`{{execute}}

  `kubectl get namespaces`{{execute}}

- Mediante el uso de un fichero *yaml* (modo declarativo):

  ```yaml
  kind: Namespace
  apiVersion: v1
  metadata:
    name: produccion
  ```

  `kubectl apply -f kubernetes_101_lab/namespace/lab1/produccion-namespace.yaml`{{execute}}
  
  `kubectl get namespaces`{{execute}}
  
  Podemos ver los Pods de nuestro nuevo namespace:
  
  `kubectl get pods -n produccion`{{execute}}

Si queremos ver todos los Pods de todos los namespaces:

`kubectl get pods --all-namespaces`{{execute}}



## Información del namespace

Podemos ver las características del namespace creado:

`kubectl describe namespace produccion`{{execute}}

`kubectl describe ns produccion`{{execute}}

También podemos obtener su definición yaml:

`kubectl get ns produccion -o yaml`{{execute}}



## BORRANDO NAMESPACES

Para borrar los namespaces podemos optar, otra vez por las dos vías:

- Vía comandos:

  `kubectl delete namespace desarrollo`{{execute}}

- Vía fichero *yaml*:

  `kubectl delete -f kubernetes_101_lab/namespace/lab1/produccion-namespace.yaml`{{execute}}

Comprobamos que se han borrado correctamente:

`kubectl get namespaces`{{execute}}