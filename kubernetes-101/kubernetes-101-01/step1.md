# Creando el  clúster de Kubernetes.

En este pequeño laboratorio vamos a ver los conceptos básicos de Kubernetes. Para ello vamos a utilizar **minikube**, una herramienta que nos permite desplegar un clúster de Kubernetes con un único nodo en una máquina virtual. Nos permitirá aprender y conocer Kubernetes en un entorno sencillo y sin necesidad de muchos recursos. 

Para evitar problemas con la instalación, hemos optado que estuviera instalado. Si queremos ver cual es el procedimiento de instalación en los diferentes entornos podemos recurrir a:

> https://kubernetes.io/es/docs/tasks/tools/install-minikube/

Para instalarlo en un entorno Linux, deberemos instalar previamente un hypervisor como  [KVM](http://www.linux-kvm.org/) o [Virtualvox](https://www.virtualbox.org/wiki/Downloads) y el interfaz de línea de comando de Kubernetes: [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) .  Una vez cumplidos estos requisitos podemos realizar la instalación de Minikube con:

``` bash
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube 

$ sudo cp minikube /usr/local/bin && rm minikube
```

Minikube presenta las siguientes características:

- DNS
- NodePorts
- ConfigMAps and Secrets.
- Dashboards.
- Container Runtime: Docker, CRI-O, and containerd.
- Enabling CNI(Container Network Interface).
- Ingress.



## Empezamos a jugar con minikube.

Lo primero que tenemos que hacer es comprobar que minikube esta instalado y funciona correctamente:

`minikube version`{{execute}}

Comprobamos que tenemos la última versión de minikube:

`minikube update-check`{{execute}}

Si vemos que existe alguna versión superior lo actualizaremos de la siguiente forma:

`wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`{{execute}}

`chmod +x minikube-linux-amd64`{{execute}}

`sudo mv minikube-linux-amd64 /usr/local/bin/minikube`{{execute}}

Confirmamos la nueva versión:

`minikube version`{{execute}}

Podemos ver todos las posibilidades que nos ofrece:

`minikube `{{execute}}

Podemos ver algo más de información:

`minikube config view `{{execute}}

Y comprobamos que el demonio de docker se comunica con el demonio de docker dentro de la máquina virtual de minikube:

`docker ps `{{execute}}

Comprobado que minikube esta instalado y operativo, vamos a levantar el clúster mono-nodo:

`minikube start `{{execute}}

Si todo ha ido bien, deberíamos tener funcionando un clúster kubernetes en nuestra máquina. Minikube ha levantado una máquina virtual sobre el hypervisor donde ejecuta el clúster.

![image-20200514194723445](./assets/minikube-architecture.png)

Comprobamos el estado del clúster:
`minikube status`{{execute}}