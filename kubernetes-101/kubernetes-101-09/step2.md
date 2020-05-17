## Implementación del ejemplo.

Primeramente descargamos los fichero:

- MySql:

  `curl -LO https://k8s.io/examples/application/wordpress/mysql-deployment.yaml`{{execute}}

- WordPress:

  `curl -LO https://k8s.io/examples/application/wordpress/wordpress-deployment.yaml`{{execute}}

Creamos el fichero kustomization.yaml

```shell
cat <<EOF >./kustomization.yaml
secretGenerator:
- name: mysql-pass
  literals:
  - password=YOUR_PASSWORD
EOF
```

```shell
cat <<EOF >>./kustomization.yaml
resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml
EOF
```

Ahora aplicamos todos los manifiestos:

`kubectl apply -k ./`{{execute}}

Verificamos que se ha creado el secret:

`kubectl get secret`{{execute}}

Verificamos los reclamos de PV:

`kubectl get pvc`{{execute}}

Verificamos que los pods se han creado correctamente:

`kubectl get pods`{{execute}}

Par ver los servicios:

`kubectl get services wordpress`{{execute}}

Finalmente para acceder a la aplicación:

`minikube service wordpress --url`{{execute}}

Accedemos vía la redirección web y comprobamos que funciona correctamente.

Podemos hacer una prueba:

`kubectl scale deploy wordpress --replicas=3`{{execute}}

`kubectl get pods`{{execute}}

`kubectl describe service wordpress`{{execute}}



## Borrar el ejemplo

`kubectl delete -k ./`{{execute}}