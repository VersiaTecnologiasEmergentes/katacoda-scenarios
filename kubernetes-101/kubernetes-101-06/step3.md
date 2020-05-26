# Nginx Ingress Controller



`minikube addons enable ingress`{{execute}}

`kubectl get pods -n kube-system`{{execute}}



`kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0`{{execute}}

`kubectl expose deployment web --type=NodePort --port=8080`{{execute}}

`kubectl get service web`{{execute}}

`minikube service web --url`{{execute}}

example-ingress.yaml

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



add /etc/hosts  minikube ip

`curl hello-world.info`{{execute}}



`kubectl create deployment web2 --image=gcr.io/google-samples/hello-app:2.0`{{execute}}

`kubectl expose deployment web2 --port=8080 --type=NodePort`{{execute}}

add example-ingress.yaml:

```yaml
     - path: /v2
        backend:
          serviceName: web2
          servicePort: 8080
```

`kubectl apply -f example-ingress.yaml`{{execute}}

`curl hello-world.info`{{{execute}}}

`curl hello-world.info/v2`{{execute}}