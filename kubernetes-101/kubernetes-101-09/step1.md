# EJEMPLO: Desplegando WordPress y MySQL con Persistent Volumes.

Source: https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/

## Requisitos

`git clone https://github.com/vthot4/kubernetes_101_lab.git`{{execute}}

`cd kubernetes_101_lab/`{{execute}}

`chmod +x environment.sh`{{execute}}

`./environment.sh`{{execute}}

`cd`{{execute}}

Comprobamos que todo esta correcto:

`minikube status`{{execute}}

Abrimos Octant. Para acceder, seleccionaos en la parte superior del terminal web, pulsar sobre el signo mas y luego pulsar en "Select port to view on Host 1". Escribir 8900, y luego pulsar "Display Port".



## Crear Persistent Volume Claims y Persistent Volumes.

MySQL y Wordpress requiere un volumen persistente para almacenar datos. Estos recursos se crearan en el deployment. En nuestro caso usará el StorageClass predeterminada del clúster, que será el *hostPath*.  Cuando se crea el Persistent VOlume Claim, el Persistent VOlume se aprovisiona dinámicamente en función de la configuración de StorageClass.



## Crear un Kustomization.yaml

Vamos a crrar un secret para almacenar una clave. Para ello usaremos:

```bash
cat <<EOF >./kustomization.yaml
secretGenerator:
- name: mysql-pass
  literals:
  - password=YOUR_PASSWORD
EOF
```

Donde sustituiremos YOUR_PASSWORD por la clave que queramos.



## Agregar configuraciones de recursos para MySQL y WordPress.

El siguiente manifiesto describe una implementación de MySQL de instancia única. El contenedor MySQL monta el volumen persistente en */var/lib/mysql*. La variable de entorno MYSQL_ROOT_PASSWORD  establece la contraseña de la base de datos desde secret.

```yaml
application/wordpress/mysql-deployment.yaml 

apiVersion: v1
kind: Service
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress
    tier: mysql
  clusterIP: None
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
---
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: wordpress
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim

```

El siguiente manifiesto describe una implementación de WordPress de instancia única. El contenedor de WordPress monta el volumen persistente en /var/www/html los archivos de datos del sitio web. La variable de entorno WORDPRESS_DB_HOST  establece el nombre del Servicio MySQL definido anteriormente, y WordPress accederá a la base de datos por Servicio. La variable de entorno WORDPRESS_DB_PASSWORD  establece la contraseña de la base de datos del Secret kustomize generado.

```yaml
application/wordpress/wordpress-deployment.yaml 

apiVersion: v1
kind: Service
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  ports:
    - port: 80
  selector:
    app: wordpress
    tier: frontend
  type: LoadBalancer
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wp-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
---
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: frontend
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: wordpress
        tier: frontend
    spec:
      containers:
      - image: wordpress:4.8-apache
        name: wordpress
        env:
        - name: WORDPRESS_DB_HOST
          value: wordpress-mysql
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-pass
              key: password
        ports:
        - containerPort: 80
          name: wordpress
        volumeMounts:
        - name: wordpress-persistent-storage
          mountPath: /var/www/html
      volumes:
      - name: wordpress-persistent-storage
        persistentVolumeClaim:
          claimName: wp-pv-claim

```






