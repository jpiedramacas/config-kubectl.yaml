# Despliegue de Aplicaciones en Kubernetes

Este documento proporciona una guía detallada sobre los archivos de configuración necesarios para desplegar cualquier aplicación en Kubernetes. Se incluyen ejemplos y explicaciones de archivos YAML para `Deployment`, `Service`, `PersistentVolume`, y `PersistentVolumeClaim`. Cada sección detalla las partes configurables y proporciona recomendaciones para adaptarlas a tus necesidades específicas.

## Archivos de Configuración

### 1. Deployment YAML

El archivo `Deployment` define cómo se debe desplegar una aplicación en Kubernetes, incluyendo el número de réplicas, la imagen del contenedor y los puertos expuestos.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-deployment # Nombre del deployment, modificar según sea necesario
spec:
  replicas: 1 # Número de réplicas del pod, ajustar según requisitos
  selector:
    matchLabels:
      app: mi-app # Etiqueta para seleccionar los pods, debe coincidir con las etiquetas en template.metadata.labels
  template:
    metadata:
      labels:
        app: mi-app # Etiqueta para el pod, debe coincidir con el selector
    spec:
      containers:
      - name: mi-contenedor # Nombre del contenedor, modificar según sea necesario
        image: mi-imagen # Imagen del contenedor, cambiar a la imagen que se desea usar (por ejemplo, nginx:latest)
        ports:
        - containerPort: 8080 # Puerto en el contenedor que la aplicación expone, ajustar según sea necesario
```

#### Descripción de Campos Clave:

- `metadata.name`: Nombre único del deployment.
- `spec.replicas`: Número de réplicas del pod a ejecutar.
- `spec.selector.matchLabels`: Selector que coincide con las etiquetas del template para identificar los pods gestionados.
- `spec.template.metadata.labels`: Etiquetas aplicadas al pod. Deben coincidir con `spec.selector.matchLabels`.
- `spec.template.spec.containers`: Define los contenedores que se ejecutarán dentro del pod.
  - `name`: Nombre del contenedor.
  - `image`: Imagen del contenedor.
  - `ports.containerPort`: Puerto expuesto por el contenedor.

### 2. Service YAML

El archivo `Service` define cómo se exponen los pods en la red. Permite acceder a la aplicación desde dentro y fuera del clúster.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mi-servicio # Nombre del servicio, modificar según sea necesario
spec:
  selector:
    app: mi-app # Selector que coincide con las etiquetas de los pods definidos en el Deployment
  ports:
    - protocol: TCP
      port: 80 # Puerto en el que el servicio estará disponible externamente
      targetPort: 8080 # Puerto en el contenedor al que se redirige el tráfico, debe coincidir con containerPort en el Deployment
```

#### Descripción de Campos Clave:

- `metadata.name`: Nombre único del servicio.
- `spec.selector`: Selector que coincide con las etiquetas de los pods gestionados por el servicio.
- `spec.ports`: Define los puertos expuestos por el servicio.
  - `port`: Puerto en el que el servicio está disponible externamente.
  - `targetPort`: Puerto en el contenedor al que se redirige el tráfico, debe coincidir con `containerPort` en el Deployment.

### 3. PersistentVolume YAML (Opcional)

El archivo `PersistentVolume` define un volumen persistente que puede ser utilizado por los pods para almacenamiento de datos.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mi-pv # Nombre del PersistentVolume, modificar según sea necesario
spec:
  capacity:
    storage: 1Gi # Capacidad de almacenamiento del volumen, ajustar según requisitos
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data" # Ruta en el nodo donde se almacenarán los datos, modificar según sea necesario
```

#### Descripción de Campos Clave:

- `metadata.name`: Nombre único del PersistentVolume.
- `spec.capacity.storage`: Capacidad de almacenamiento del volumen.
- `spec.accessModes`: Modos de acceso al volumen.
- `spec.hostPath.path`: Ruta en el nodo donde se almacenarán los datos.

### 4. PersistentVolumeClaim YAML (Opcional)

El archivo `PersistentVolumeClaim` solicita un volumen persistente del clúster con las características especificadas.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mi-pvc # Nombre del PersistentVolumeClaim, modificar según sea necesario
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi # Cantidad de almacenamiento solicitada, ajustar según requisitos
```

#### Descripción de Campos Clave:

- `metadata.name`: Nombre único del PersistentVolumeClaim.
- `spec.accessModes`: Modos de acceso solicitados para el volumen.
- `spec.resources.requests.storage`: Cantidad de almacenamiento solicitada.

## Despliegue

Para desplegar estos recursos en tu clúster de Kubernetes, sigue estos pasos:

1. Crea el deployment:
   ```sh
   kubectl apply -f deployment.yaml
   ```

2. Crea el servicio:
   ```sh
   kubectl apply -f service.yaml
   ```

3. (Opcional) Crea el PersistentVolume:
   ```sh
   kubectl apply -f persistentvolume.yaml
   ```

4. (Opcional) Crea el PersistentVolumeClaim:
   ```sh
   kubectl apply -f persistentvolumeclaim.yaml
   ```

## Personalización

Asegúrate de modificar los valores en los archivos YAML según las necesidades específicas de tu aplicación, como nombres, imágenes de contenedor, puertos y rutas de almacenamiento.

## Conclusión

Este documento proporciona una guía clara y detallada para desplegar aplicaciones en Kubernetes utilizando archivos YAML para Deployment, Service, PersistentVolume, y PersistentVolumeClaim. Personaliza los archivos según tus requisitos y sigue los pasos de despliegue para ejecutar tu aplicación en el clúster de Kubernetes.
