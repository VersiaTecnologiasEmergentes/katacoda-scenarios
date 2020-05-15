## Octant. Una consola para Kubernetes.

Aunque el Dashboard puede ser una opción valida a nosotros nos gusta particularmente la opción de usar **Octant** del proyecto Tanzu de VMware. Podemos ver información referente al proyecto en: https://github.com/vmware-tanzu/octant

Instalaremos Octant de la siguiente forma:

`wget https://github.com/vmware-tanzu/octant/releases/download/v0.12.1/octant_0.12.1_Linux-64bit.deb`{{execute}}

`dpkg -i octant_0.12.1_Linux-64bit.deb`{{execute}}

`OCTANT_DISABLE_OPEN_BROWSER=true  OCTANT_LISTENER_ADDR=0.0.0.0:8900 nohup octant &`{{execute}}

Para acceder, seleccionaos en la parte superior del terminal web, pulsar sobre el signo mas y luego pulsar en "Select port to view on Host 1". Escribir 8900, y luego pulsar "Display Port".



Esta herramienta nos ayudará a comprender cómo se ejecutan las aplicaciones en el clúster de Kubernetes. Podremos visualizar gráficamente las dependencias de los objetos de Kubernetes, redireccionar los puertos locales a un Pod en ejecución, inspeccionar los logs de un Pod, etc...

Octant es una herramienta diseñada para los clientes, lo que significa que los usuarios no tienen que instalar nada en el clúster par comenzar a usarla. Octant usará las credenciales locales del usuario por lo que no nos deberemos de preocupar de dar permisos en Kubernetes. Otro tema muy interesante es que Octant admite múltiples archivos *kubeconfig* por lo que los usuarios pueden cambiar de contexto de forma ágil y sencilla.



