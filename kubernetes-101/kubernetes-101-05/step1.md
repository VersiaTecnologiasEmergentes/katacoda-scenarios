# LABELS y SELECTORES



## Requisitos

`git clone https://github.com/vthot4/kubernetes_101_lab.git`{{execute}}

`cd kubernetes_101_lab/`{{execute}}

`chmod +x environment.sh`{{execute}}

`./environment.sh`{{execute}}

`cd`{{execute}}

Comprobamos que todo esta correcto:

`minikube status`{{execute}}


Abrimos Octant. Para acceder, seleccionaos en la parte superior del terminal web, pulsar sobre el signo mas y luego pulsar en "Select port to view on Host 1". Escribir 8900, y luego pulsar "Display Port".



## Introducción

Las etiquetas son pares de clave/valor que se asocian a los objetos, como los pods. El propósito de las etiquetas es permitir identificar atributos de los objetos que son relevantes y significativos para los usuarios, pero que no tienen significado para el sistema principal. Se puede usar las etiquetas para organizar y seleccionar subconjuntos de objetos. Las etiquetas se pueden asociar a los objetos a la hora de crearlos y posteriormente modificarlas o añadir nuevas. Cada objeto puede tener un conjunto de etiquetas clave/valor definidas, donde cada clave debe ser única para un mismo objeto.

```yaml
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

Las etiquetas permiten consultar y monitorizar los objetos de forma más eficiente y son ideales para su uso en UIs y CLIs.

Al contrario que los nombres y UIDs, las etiquetas no garantizan la unicidad. En general, se espera que muchos objetos compartan la(s) misma(s) etiqueta(s).

A través del selector de etiqueta, el cliente/usuario puede identificar un conjunto de objetos. El selector de etiqueta es la primitiva principal de agrupación en Kubernetes.

La API actualmente soporta dos tipos de selectores: basados en igualdad y basados en conjunto. Un selector de etiqueta puede componerse de múltiples requisitos separados por coma. En el caso de múltiples requisitos, todos ellos deben ser satisfechos de forma que las comas actúan como operadores AND (&&) lógicos.

La semántica de selectores vacíos o no espefificados es dependiente del contexto, y los tipos de la API que utilizan los selectores deberían documentar su propia validación y significado.

