## Creando un segundo clúster.

Aunque Katakoda no permite levantar un segundo clúster en la máquina que despliega, vamos a recoger como se haría porque es una práctica bastante interesante. Nos permite tener varios clúster en local  como por ejemplo uno para Desarrollo y otro para Producción. Para levantar el nuevo clúster:

```bash
minikube start -p dev
```

Para ver la lista de clusters que tenemos creada usaremos la opción *"profile list"*:

`minikube profile list`{{execute}}

Para comprobar en que profile estoy usaremos:

`minikube profile`{{execute}}

Para comenzar a usar el segundo clúster que me hemos creado, tendremos que cambiar de profile:

```bash
minikube profile list
```

Para borrarlos usaremos *minikube delete* desde el contexto del *profile* que queramos borrar.