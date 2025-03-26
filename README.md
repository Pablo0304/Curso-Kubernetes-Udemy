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

### 📦 Pods

```bash
kubectl run nginx1 --image=nginx
```
Crea un Pod llamado `nginx1` con la imagen de NGINX.

```bash
kubectl get pods
```
Lista todos los Pods en el espacio de nombres actual.

```bash
kubectl delete pod <nombre>
```
Elimina un Pod.

---

### ⚙️ Recursos

```bash
kubectl get all
```
Muestra todos los recursos: Pods, Services, Deployments, etc.

```bash
kubectl describe pod <nombre>
```
Muestra detalles técnicos de un Pod (eventos, configuración, etc).

```bash
kubectl logs <nombre-pod>
```
Muestra los logs del contenedor en un Pod.

---

### 📁 Configuración y Contextos

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

---

### 🧪 Minikube

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

---

## 📌 Notas

- Las imágenes de Docker son reutilizadas por Kubernetes.
- Kubernetes usa motores como Docker o containerd para ejecutar los contenedores.
- `kubectl` es la herramienta para interactuar con el clúster, y usa el archivo `kubeconfig` para saber a qué clúster conectarse.

---

## 🧠 Autor

Este documento forma parte de mi aprendizaje y práctica con Docker y Kubernetes. Está pensado como guía personal y referencia profesional.  
Desarrollado y mantenido con ❤️ por Pablo.

