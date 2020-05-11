# Conociendo nuestro clúster.

> Un clúster es una colección de recursos de cómputo, almacenamiento y redes que Kubernetes usa para ejecutar las diversas cargas de trabajo que componen su sistema.



Comenzaremos levantando el entorno de minikube.

`minikube start`{{execute}}

Una vez arrancado vamos a ver los detalles del clúster que hemos creado mediante *kubectl cluster-info*:

`kubectl cluster-info`{{execute}}

Una forma sencilla de tener un diagnóstico del estado del clúster es usar:

`kubectl get componentstatuses`{{execute}}

Deberemos obtener algo parecido a esta salida:

```bash
$ kubectl get componentstatuses
NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok
scheduler            Healthy   ok
etcd-0               Healthy   {"health":"true"}
```

Podemos ver aquí los componentes que conforman el clúster de Kubernetes. 

- **controller-manageres**  Es el responsable de ejecutar varios controladores que regulan el comportamiento en el clúster; por ejemplo, garantizar que todas las réplicas de un servicio estén disponibles y en buen estado.
- **scheduleres** responsable de colocar diferentes Pods en diferentes nodos en el clúster. 
- **etcd** Es el almacenamiento para el clúster donde se almacenan todos los objetos API.



## Kubeconfig

Utilizaremos los archivos **kubeconfig** para organizar la información acerca de los clústeres, los usuarios, los Namespaces y los mecanismos de autenticación. La herramienta **kubectl** utilizará estos archivos para hallar la información que necesita para escoger un clúster y comunicarse con el servidor API de un clúster.

Por defecto Kubectl busca un archivo llamado ***config*** en el directorio $HOME/.kube . En nuestro caso:

`cat .kube/config`{{execute}}

Podremos modificar esta ubicación por defecto usando la variable de entorno **KUBECONFIG** o medainte el modificador de kubectl **--kubeconfig**.



## Nodos

> Un nodo es un único host. Puede ser una máquina física o virtual. Su trabajo es ejecutar pods, que son la unidad mínima de procesamiento de Kubernetes. Cada nodo de Kubernetes ejecuta varios componentes de Kubernetes, como pueden ser  *kubelet* y *kube proxy*. Los nodos son gestionados por el nodo master. 

Para ver los nodos que tenemos levantados:

`kubectl get nodes`{{execute}}

Este comando muestra todos los nodos que se pueden utilizar para hospedar nuestras aplicaciones. Ahora solo tenemos un nodo y podemos ver que su estado es *"Ready"* para recibir nuestras aplicaciones.

Podemos usar *"kubectl describe"* para obtener información específica del nodo. Obtendremos información referente a:

- Información básica del nodo. (Sistema Operativo, procesador)
- Información sobre el funcionamiento.
- Información sobre la capacidad de la máquina.
- Información sobre el software del nodo.
- Información sobre los Pods que se están ejecutando.

`kubectl describe nodes minikube`{{execute}}



## Octant. Una consola para Kubernetes.

Aunque el Dashboard puede ser una opción valida a nosotros nos gusta particularmente la opción de usar **Octant** del proyecto Tanzu de VMware. Podemos ver información referente al proyecto en: https://github.com/vmware-tanzu/octant

Instalaremos Octant de la siguiente forma:

`wget https://github.com/vmware-tanzu/octant/releases/download/v0.12.1/octant_0.12.1_Linux-64bit.deb`{{execute}}

`dpkg -i octant_0.12.1_Linux-64bit.deb`{{execute}}

`$ OCTANT_DISABLE_OPEN_BROWSER=true  OCTANT_LISTENER_ADDR=0.0.0.0:8900 nohup octant &`{{execute}}



Esta herramienta nos ayudará a comprender cómo se ejecutan las aplicaciones en el clúster de Kubernetes. Podremos visualizar gráficamente las dependencias de los objetos de Kubernetes, redireccionar los puertos locales a un Pod en ejecución, inspeccionar los logs de un Pod, etc...

Octant es una herramienta diseñada para los clientes, lo que significa que los usuarios no tienen que instalar nada en el clúster par comenzar a usarla. Octant usará las credenciales locales del usuario por lo que no nos deberemos de preocupar de dar permisos en Kubernetes. Otro tema muy interesante es que Octant admite múltiples archivos *kubeconfig* por lo que los usuarios pueden cambiar de contexto de forma ágil y sencilla.

