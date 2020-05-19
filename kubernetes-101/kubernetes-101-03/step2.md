## Trabajando de forma declarativa con los Pods.

En este pequeño apartado vamos a ver la diferencia entre una aproximación imperativa y una declarativa reflejandola en el uso de *kubectl*:

- **kubectl create.** utilizará la gestión imperativa especificando lo que deseamos crear, eliminar o remplazar.  (https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-config/)
- **kubectl apply.** Utilizará un enfoque declarativo en el que le decimos al API cómo queremos que esten los recursos del clúster. (https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)



En entornos productivos la forma más acertada sería el enfoque declarativo ya que nos obliga a definir la cunfiguración de nuestros despliegues en yamls que son versionables. Aún así la aproximación imperativa es interesante porque se suele usar para borrar objetos y en algunos casos para crear plantillas que luego podemos usar para el modo declarativo.

Vamos a  ver un ejemplo:

`cat kubernetes_101_lab/pod/lab1/nginx.yaml`{{execute}}

Creamos el objeto de manera imperativa:

`kubectl create -f kubernetes_101_lab/pod/lab1/nginx.yaml`{{execute}}

Volvemos a intentar crearlo:

`kubectl create -f kubernetes_101_lab/pod/lab1/nginx.yaml`{{execute}}

Vemos que nos devuelve un error, porque el objeto ya existe.

```bash
$ kubectl create -f kubernetes_101_lab/pod/lab1/nginx.yaml
Error from server (AlreadyExists): error when creating "kubernetes_101_lab/pod/lab1/nginx.yaml": pods "webserver" already exists
```

Ahora vamos a probar a usar el modo declarativo:

`kubectl apply -f kubernetes_101_lab/pod/lab1/nginx.yaml`{{execute}}

Vemos que aunque lanza algún warning lo realiza correctamente porque lo que intenta no es crear el objeto, sino actualizar la definición del mismo.