# Propiedades del Pod.

Para obtener información detallada del Pod vamos usar **"describe"**.  Este modificador necesita que le pasemos el tipo de objeto y el nombre del recurso:

`kubectl get pods`{{execute}}

`clear`{{execute}}

Algunos de los estados posibles del Pod son:

- **Pendind**. Kubernetes ha aceptado el Pod, pero todavía no se ha creado una o más imágenes del contenedor. Esto incluye el tiempo antes de ser programado, así como el tiempo dedicado a descargar imágenes a través de la red, lo que podría llevar un tiempo.
- **Running**.  El Pod se ha vinculado a un nodo y se han creado todos los Contenedores. Al menos un contenedor aún se está ejecutando o está en proceso de iniciarse o reiniciarse.
- **Succeeded**.  Todos los contenedores en el Pod han finalizado con éxito y no se reiniciarán.
- **Failed**.  Todos los Contenedores en el Pod han terminado, y al menos un Contenedor ha terminado en error.
- **Unknow**. Por alguna razón, no se pudo obtener el estado del Pod, generalmente debido a un error en la comunicación con el host del Pod.



Podemos obtener información ampliada del Pod: 

`kubectl describe pod webserver`{{execute}}

En este caso nos debería de mostrar toda la información referente a los dos pods, ya que los dos empiezan por *webserver*.