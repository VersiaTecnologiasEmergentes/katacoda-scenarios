## Complementos de minikube

Minikube dispone de complementos que nos pueden ayudar a administrar el clúster de Kubernetes. Podemos ver la lista de los que dispone por defecto mediante:

 `minikube addons list `{{execute}}

Vamos a instalar el complemento *metrics-server* para ver como se añadiría uno. Este complemento nos proporciona una solución para monitorizar kubernetes. 

  `minikube addons enable metrics-server  `{{execute}}

Tardará de 2 a 3 minutos para que podamos tener métricas disponibles. Podemos acceder a ellos:

 `kubectl top node `{{execute}}

`kubectl top pods --all-namespaces `{{execute}}

