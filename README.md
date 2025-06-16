# 🐳 Docker & ☸️ Kubernetes - Guía de Comandos

Este repositorio contiene una recopilación de los comandos más útiles y utilizados en **Docker** y **Kubernetes**, con explicaciones claras y ejemplos prácticos.

---

## 🐳 Docker – Comandos Esenciales

### 🔧 Imágenes

```bash
docker pull <imagen>
```
Descarga una imagen desde Docker Hub.

```bash
docker images
```
Lista las imágenes disponibles en tu sistema.

```bash
docker rmi <imagen>
```
Elimina una imagen.

---

### 🚀 Contenedores

```bash
docker run <imagen>
```
Crea y ejecuta un contenedor a partir de una imagen.

```bash
docker run -d -p 80:80 --name web nginx
```
Ejecuta un contenedor en segundo plano con el puerto 80 expuesto.

```bash
docker ps
```
Muestra los contenedores en ejecución.

```bash
docker ps -a
```
Muestra todos los contenedores, estén o no en ejecución.

```bash
docker stop <id|nombre>
docker start <id|nombre>
docker restart <id|nombre>
```
Controla el estado de un contenedor.

```bash
docker exec -it <id|nombre> bash
```
Entra dentro de un contenedor para trabajar en su terminal.

---

### 🧹 Limpieza

```bash
docker system prune
```
Elimina todo lo que no se esté usando: contenedores, redes, imágenes y cachés.

---

## ☸️ Kubernetes – Comandos Básicos

### ⚙️ Workloads (Aplication Resources)

#### Pod
DESCRIPCIÓN: Unidad básica que ejecuta contenedores.

```bash
kubectl run nginx1 --image=nginx
```
Crea un Pod llamado `nginx1` con la imagen de NGINX.

```bash
kubectl get pods
```
Lista todos los `Pods` en el espacio de nombres actual.

```bash
kubectl get pod nginx -o yaml
```
Da información sobre el pod `nginx` con formato YAML.  
El flag `--show-labels` da información sobre los `labels` configurados en el archivo YAML (o con el flag `-L <nombre_Label1>,<nombre_Label2>` para verlos en otras columnas).  
El flag `-n <nombre_namespace>` da información sobre los recursos de ese tipo que se ubican en el `namespace` indicado.

```bash
kubectl delete pod <nombre> --grace-period=5
kubectl delete pod/<nombre> --grace-period=5
```
Dos formas de eliminar un Pod.  
El `grace-period` indica el tiempo que esperará para eliminarlo (en segundos).  
En caso de problemas, el flag `--now` no espera a los procesos pendientes y elimina el pod.

```bash
kubectl delete pods --all
```
⚠️ ¡CUIDADO! ⚠️ => Elimina TODOS los `Pods`.

```bash
kubectl delete all --all
```
⚠️ ¡CUIDADO! ⚠️ => Elimina TODO.

```bash
kubectl get all
```
Muestra todos los recursos: `Pods`, `Services`, `Deployments`, etc.

```bash
kubectl describe pod <nombre>
kubectl describe pod/<nombre>
```
Dos frmas de mostrar detalles técnicos de un Pod (eventos, configuración, etc).

```bash
kubectl logs <nombre-pod>
```
Muestra los logs del contenedor.  
Otra opción útil es el `-f` (follow) para ver los logs en tiempo real.  
En caso de tener varios recursos, solo mostrará los logs del primero, podemos mostrar los logs de un recurso concreto con `-c <nombre_Recurso>`.

```bash
kubectl exec -it nginx1 -- /bin/bash
```
Permite conectarse al shell del Pod `nginx1`.  
En caso de tener varios recursos, podemos elegir un recurso concreto con `-c <nombre_Recurso>`.

```bash
kubectl proxy
```
Para acceder a la API de Kubernetes a través de un servidor proxy local.

```bash
kubectl port-forward nginx 9999:80
```
Por si no quieres crear servicios, puedes usar el port-forward para el desarrollo.

#### ReplicaSet
DESCRIPCIÓN: Mantiene un número fijo de Pods idénticos.
```bash
kubectl create rs <replica_Set_Name>
```
Crea un `ReplicaSet`.

#### Deployment
DESCRIPCIÓN: Administra ReplicaSets y permite actualizaciones de versión.  
```bash
kubectl create deployment <deployment_Name>
```
Crea un `Deployment`.

```bash
kubectl get deploy
```
Devuelve información sobre el `Deployment`.

```bash
kubectl scale deploy <resource_Name> --replicas=5
```
Escalar un deployment.  
⚠️ ¡CUIDADO! ⚠️ => No se edita el archivo en local.

#### StatefulSet
DESCRIPCIÓN: Igual que Deployment pero con identidad persistente y almacenamiento estable (ideal para BBDD).

#### DaemonSet
DESCRIPCIÓN: Ejecuta un Pod en cada nodo del clúster, útil para logs o monitoreo.

#### Job
DESCRIPCIÓN: Ejecuta una tarea una vez hasta que termine correctamente.

#### CronJob
DESCRIPCIÓN: Ejecuta Jobs de forma programada.

---

### ⚙️ Recursos de Red (Networking)

#### Services
Middleware entre el cliente y los `Application Resources`.  

```bash
kubectl expose pod nginx1 --port=80 --name=nginx1-svc --type=LoadBalancer
```
Este comando crea un servicio de tipo `LoadBalancer` para exponer el Pod `nginx1` en el puerto 80.  
Se puede cambiar el tipo para crear un `Cluster IP`(este es el servicio por defecto si no se pone nada), `Node Port` o `Load Balancer`(solo funciona en entornos Cloud).  
Se puede usar para otros tipos de recursos.

```bash
kubectl get svc
```
Este comando lista todos los servicios disponibles en el clúster de Kubernetes, mostrando detalles como la IP externa, el puerto, y el tipo de servicio.

##### Tipos
- Cluster IP: Accesible solo desde dentro del cluster.
- Node Port: Similar a `Cluster IP` pero para fuera de la red.
- Load Balancer: Similar a `Node Port` pero integrado con balanceadores de carga de terceros (AWS, Azure,...).

---

### ⚙️ Configuración y Contextos

```bash
kubectl config view
```
Muestra la configuración actual de kubectl (clústeres, usuarios, contextos).

```bash
kubectl config current-context
```
Muestra el contexto actual (a qué clúster estás conectado).

```bash
kubectl config use-context <nombre>
```
Cambia al contexto indicado.

```bash
kubectl config set-context --current --namespace=pro
```
Modifica o crea un contexto (en este caso cambiamos el `namespace`).

```bash
kubectl apply -f <file>.yaml
```
Aplica una configuración declarativa a un recurso en el clúster. Si el recurso no existe, lo crea; si ya existe, lo actualiza.

```bash
kubectl apply -f ./folder/
```
Aplica todos los archivos YAML contenidos en una carpeta (ideal para múltiples recursos como `Pods`, `Services`, `Deployments`, `ConfigMaps`, etc.).

```bash
kubectl edit <resourde_Type> <resource_Name>
```
Es una forma de editar `al vuelo` un recurso.  
⚠️ ¡CUIDADO! ⚠️ =>
- No se edita el archivo en local.
- Si hay errores de sintaxis puede haber problemas en el funcionamiento del recurso.
- Al guardar el archivo se aplica la configuración directamente sin confirmación extra.

```bash
kubectl label pod <nombre_Pod> <label_Name>=<label_Value>
kubectl label pod <nombre_Pod> <label_Name>-
```
Añade o borra nuevos `labels` en la configuración del Pod.  
Para editar uno ya creado se usa el flag `--overwrite`.  
- Selectores:  
    - Con el flag `-l <label_Name>=<label_Value>` (o `!=`) cuando hacemos CASI CUALQUIER COMANDO, podremos filtrar todos los `Pods` por `labels` (añadir el flag `--show-labels` para poder ver los `labels`).
    - También se puede con `-l '<label_Name> in(<label_Value1>,<label_Value2>)'` o `notin(...)` para facilitar la lectura del comando.


---

### ⚙️ Namespaces

Los `namespaces` permiten dividir lógicamente los recursos dentro de un mismo clúster de Kubernetes. Son útiles para separar entornos (`producción`, `desarrollo`, `test`), evitar conflictos de nombres y aplicar políticas específicas a grupos de recursos.

#### Comandos útiles
```bash
kubectl get namespaces
```
Lista todos los `namespaces` disponibles en el clúster.

```bash
kubectl create namespace <nombre>
```
Crea un `namespace` con el nombre indicado.

```bash
kubectl delete namespace <nombre>
```
Elimina un `namespace` y todos los recursos que contiene.  
⚠️ ¡CUIDADO! ⚠️ => Se eliminan todos los recursos dentro del namespace.

```bash
kubectl apply -f <archivo>.yaml -n <namespace>
```
Aplica un archivo YAML en el `namespace` indicado.

```bash
kubectl get all -n <namespace>
```
Muestra todos los recursos dentro del `namespace` indicado.

```bash
kubectl config set-context --current --namespace=<nombre>
```
Configura el `namespace` por defecto para tu contexto actual.

```bash
kubectl get events --namespace pro --field-selector type="Warning" -w
```
Muestra los eventos de ese `namespace`.  
En este caso se puede filtrar (`--field-selector type="Warning"`) por las columnas que se requiera (`LAST SEEN`, `TYPE`, `REASON`, `OBJECT` o `MESSAGE`).  
Con el flag `-w` lo dejaremos escuchando eventos.

---

#### 🧾 Uso en archivos YAML
Para que un recurso YAML se cree dentro de un `namespace` específico, se añade el campo en `metadata`:

```yaml
metadata:
  name: frontend
  namespace: desarrollo  # 👈 Define en qué namespace se creará
```

---

#### 🧠 Notas
- Si no se indica ningún `namespace`, se usa el espacio por defecto: `default`.
- Puedes tener recursos con el mismo nombre en diferentes namespaces sin conflicto.
- Es buena práctica usar namespaces distintos para separar entornos o equipos.

---

## 🧪 Minikube

```bash
minikube start
```
Inicia el clúster local.

```bash
minikube stop
```
Detiene el clúster.

```bash
minikube status
```
Verifica si está en ejecución.

```bash
minikube dashboard
```
Abre el panel gráfico de Kubernetes en tu navegador.

```bash
minikube ip
```
Muestra la IP del clúster local.

```bash
minikube ssh
```
Permite iniciar una sesión SSH en la máquina virtual que Minikube está utilizando para simular un clúster de Kubernetes local.

---

## 📌 Notas

- Las imágenes de Docker son reutilizadas por Kubernetes.
- Kubernetes usa motores como Docker o containerd para ejecutar los contenedores.
- `kubectl` es la herramienta para interactuar con el clúster, y usa el archivo `kubeconfig` para saber a qué clúster conectarse.

---

## 🧠 Autor

Este documento forma parte de mi aprendizaje y práctica con Docker y Kubernetes. Está pensado como guía personal y referencia profesional.  
Desarrollado y mantenido con ❤️ por Pablo.

