## Como reiniciar los Pods.

Estados -----

Políticas de reinicio:

- Always. Que siempre que tenga una caida rebote siempre. Opción por defecto.
- OnFailure. Se reinicia siempre que falla. SI lo para de forma predeterminada el servicio permanecerá parado.
- Never. Evitamos que se reinicie pase lo que pase. 



Ejemplo:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    app: tomcat
spec:
  containers:
   - name: tomcat     
     image: tomcat
  restartPolicy: Always
```

`kubectl apply -f tomcat_always.yaml`{{execute}}

`kubectl get pods`{{execute}}

`kubectl describe pods tomcat`{{execute}}

Nos conectamos al container:

`kubectl exec -it tomcat bash`{{execute}}

`ps -ef`{{execute}}

`catalina.sh stop`{{execute}}

`kubectl get pods`{{execute}}

Vemos como RESTARS está a 1.

Ahora borramos para 

`kubectl delete pod tomcat`{{execute}}

Cambiamos a OnFailure

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    app: tomcat
spec:
  containers:
   - name: tomcat     
     image: tomcat
  restartPolicy: OnFailure
```



`kubectl apply -f tomcat_OnFailure.yaml`{{execute}}

`kubectl get pods`{{execute}}

kubectl exec -it tomcat bash

catalina.sh stop

kubectl get pods





```yaml
apiVersion: v1
kind: Pod
metadata:
  name: on-failure
  labels:
    app: app1
spec:
  containers:
  - name: on-failure
    image: busybox
    command: ['sh', '-c', 'echo Ejemplo de pod fallado  && exit 1']
  restartPolicy: OnFailure
```

kubectl apply -f restart_OnFailure.yaml

kubectl get pods





## Labels y  Selectors

**LABELS**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    estado: "desarrollo"
spec:
  containers:
   - name: tomcat
     image: tomcat
```

`kubectl apply -f tomcat.yaml`

kubectl get pods -o wide

kubectl get pod tomcat --show-labels

Para crear una nueva columna con la estiqueta estado

kubectl get pod tomcat --show-labels -L estado



Añadir etiqueta

kubectl label pod tomcat responsable=midleware

kubectl get pod tomcat --show-labels

Kubectl label --overwrite pod tomcat estado=test

kubectl get pod tomcat --show-labels

PAra borrar las etiquetas:

kubectl label pod tomcat responsable-

Aunque podamos hacerlo todo de forma imperativa, deberíamos acostumbrarnos a hacerlo de forma declarativa.

**SELECTORS**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat
  labels:
    estado: "desarrollo"
    responsable: "juan"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat1
  labels:
    estado: "desarrollo"
    responsable: "juan"
spec:
  containers:
   - name: tomcat     
     image: tomcat
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat2
  labels:
    estado: "testing"
    responsable: "pedro"
spec:
  containers:
   - name: tomcat     
     image: tomcat

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tomcat3
  labels:
    estado: "produccion"
    responsable: "pedro"
spec:
  containers:
   - name: tomcat     
     image: tomcat

```

kubectl apply -f .

kubectl get pods

kubectl get pods --show-labels

kubectl get pods --show-labels -l estado=desarrollo

kubectl get pods -show-labels -l estado=testing

kubectl get pods --show-labels -l responsable!=juan

kubectl get pods --show-labels -l 'estado in(desarrollo)'

kubectl get pods --show-labels -l 'estado in(desarrollo,testing)'

kubectl get pods --show-labels -l 'estado notin(desarrollo)'

kubectl delete pods -l estado=desarrollo

