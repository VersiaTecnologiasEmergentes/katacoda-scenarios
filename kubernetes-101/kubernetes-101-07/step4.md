# Establecer un namespace por defecto.

Podemos indicar en un determinado contexto (un contexto determina el cluter y el usuario que podemos utilizar) un namespace, de tal manera que cuando utilicemos dicho contexto se va a utilizar el namespace indicado, y no será necesario indicarlo con la opción -n. Para ello es necesario determinar el contexto en el que estamos trabajando:

`kubectl config current-context`{{execute}}

Vemos que se corresponde con la confguración que tenemos en el config:

`kubectl config view`{{execute}}

Para modificar el contexto actual:

`kubectl config set-context --current --namespace=desarrollo`{{execute}}

`kubectl config current-context`{{execute}}

Podemos ver como el namespace lo ha modificado:

```yaml
- context:
    cluster: minikube
    namespace: desarrollo
    user: minikube
  name: minikube
current-context: minikube
```

Si ahora vemos los Pods que están corriendo:

`kubectl get pods`{{execute}}

Para terminar, borramos el deploy:

`kubectl delete deploy elastic -n desarrollo`{{execute}}

`kubectl get deploy -n desarrollo`{{execute}}