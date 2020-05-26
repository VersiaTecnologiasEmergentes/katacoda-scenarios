## Labels

Para nuestra primera prueba, vamaos a usar el siguiente *yaml*:

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

En este caso vamos a levantar un tomcat. Para ello:

`kubectl apply -f kubernetes_101_lab/labels/lab1/tomcat.yaml`{{execute}}

Podemos ver los Pods desplegados:

`kubectl get pods -o wide`{{execute}}

Para ver la etiquetas:

`kubectl get pod tomcat --show-labels`{{execute}}

Para crear una nueva columna con la etiqueta estado:

`kubectl get pod tomcat --show-labels -L estado`{{execute}}



## ADD LABEL

`kubectl label pod tomcat responsable=sistemas`{{execute}}

Podemos ver las etiquetas:

`kubectl get pod tomcat --show-labels`{{execute}}



## RE-WRITE LABEL

Cambiaremos el valor de la etiqueta de la siguiente forma:

`kubectl label --overwrite pod tomcat estado=test`{{execute}}

`kubectl get pod tomcat --show-labels`{{execute}}



## DELETE LABEL

Para borrar una etiqueta:

`kubectl label pod tomcat responsable-`{{execute}}

`kubectl get pod tomcat --show-labels`{{execute}}



Nota: Aunque podamos hacerlo todo de forma imperativa, deberíamos acostumbrarnos a hacerlo de forma declarativa.