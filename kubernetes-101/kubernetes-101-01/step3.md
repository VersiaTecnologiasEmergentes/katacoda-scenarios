# Conociendo nuestro clúster.

## Introducción

> Un clúster es una colección de recursos de cómputo, almacenamiento y redes que Kubernetes usa para ejecutar las diversas cargas de trabajo que componen su sistema.


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




