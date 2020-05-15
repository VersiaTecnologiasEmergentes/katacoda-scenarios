## Modificando el arranque por defecto

Debemos fijarnos que hemos levantando el entorno por defecto, si queremos proporcionarle más memoria o cpu podemos añadir los siguientes parámetros:

```bash
 minikube start --cpus 4 --memory 8192
```

También podemos cambiar el runtime a utilizar, por ejemplo seleccionado ***CRI-O***

```bash
minikube start \
    --network-plugin=cni \
    --enable-default-cni \
    --container-runtime=cri-o \
    --bootstrapper=kubeadm
```