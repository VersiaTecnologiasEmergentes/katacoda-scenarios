# Rollback de nuestra aplicación.

Los deployments nos brindan la posibilidad de volver a versiones anteriores. Podemos sacar el hisotrial con:

`kubectl rollout history deployment nginx-deployment`{{execute}}

Para volver atrás:

`kubectl rollout undo deployment nginx-deployment`{{execute}}

Podemos ver como los Pods han vuelto a la versión anterior:

`kubectl describe pods`{{execute}}