## Nodos

> Un nodo es un único host. Puede ser una máquina física o virtual. Su trabajo es ejecutar pods, que son la unidad mínima de procesamiento de Kubernetes. Cada nodo de Kubernetes ejecuta varios componentes de Kubernetes, como pueden ser  *kubelet* y *kube proxy*. Los nodos son gestionados por el nodo master. 

Para ver los nodos que tenemos levantados:

`kubectl get nodes`{{execute}}

Este comando muestra todos los nodos que se pueden utilizar para hospedar nuestras aplicaciones. Ahora solo tenemos un nodo y podemos ver que su estado es *"Ready"* para recibir nuestras aplicaciones. Podemos ver algo más de información usando el modificador *""-o wide"*:

`kubectl get nodes -o wide`{{execute}}



Podemos usar *"kubectl describe"* para obtener información específica del nodo. Obtendremos información referente a:

- Información básica del nodo. (Sistema Operativo, procesador)
- Información sobre el funcionamiento.
- Información sobre la capacidad de la máquina.
- Información sobre el software del nodo.
- Información sobre los Pods que se están ejecutando.

`kubectl describe nodes minikube`{{execute}}
