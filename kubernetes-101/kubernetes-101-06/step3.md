# Nginx Ingress Controller

Un Ingress es un objeto API que define reglas que permiten el acceso externo a los servicios en un clúster. Un controlador de Ingress cumple las reglas establecidas en el Ingress.



Lo primero que vamos a hacer es habilitar el controlador ingress de nginx:

`minikube addons enable ingress`{{execute}}

Verificamos que se ha intalado correctamente:

`kubectl get pods -n kube-system`{{execute}}



Comenzamos implementando una aplicación: hello-app

`kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0`{{execute}}

Exponemos la implementación:

`kubectl expose deployment web --type=NodePort --port=8080`{{execute}}

`kubectl get service web`{{execute}}

`minikube service web --url`{{execute}}



A continuación tenemos que crear el "ingress resource" mediante el fichero: example-ingress.yaml

```yaml
 apiVersion: networking.k8s.io/v1beta1 # for versions before 1.14 use extensions/v1beta1
 kind: Ingress
 metadata:
   name: example-ingress
   annotations:
     nginx.ingress.kubernetes.io/rewrite-target: /$1
 spec:
   rules:
   - host: hello-world.info
     http:
       paths:
       - path: /
         backend:
           serviceName: web
           servicePort: 8080
```

`kubectl apply -f example-ingress.yaml`{{execute}}

`kubectl get ingress`{{execute}}

Para hacer las pruebas debemos obtener la IP del cluster local y añadir la sigueinte entrada al /etc/hosts:

add /etc/hosts  minikube ip

Una vez añadida, comprobamos que funciona correctamente:

`curl hello-world.info`{{execute}}



Creamos un segundo despliegue:

`kubectl create deployment web2 --image=gcr.io/google-samples/hello-app:2.0`{{execute}}

`kubectl expose deployment web2 --port=8080 --type=NodePort`{{execute}}

Añadimos al ingress:

add example-ingress.yaml:

```yaml
     - path: /v2
        backend:
          serviceName: web2
          servicePort: 8080
```

`kubectl apply -f example-ingress.yaml`{{execute}}

Comprobamos que todo funciona correctamente:

`curl hello-world.info`{{{execute}}}

`curl hello-world.info/v2`{{execute}}