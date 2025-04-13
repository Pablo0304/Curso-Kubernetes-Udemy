# 🐳 Archivos YAML en Kubernetes - Guía de Configuración

Este archivo contiene información referente a la configuración de archivos **YAML** para la creación de recursos utilizando buenas prácticas, con explicaciones claras y ejemplos prácticos.

---

## ⚙️ YAML – Atributos

---

### 📁 ApiVersion
Indica la versión de la API de Kubernetes que se usará para interpretar el recurso.  
Ejemplos: `v1`, `apps/v1`, `batch/v1`.

---

### 📁 Kind
Especifica el tipo de recurso que se está definiendo.  
Ejemplos: `Pod`, `Deployment`, `Service`, `Job`.

---

### 📁 Metadata
Contiene la información básica del recurso.  
Ejemplos: `name`, `namespace`, `labels` y `annotations`.

---

### 📁 Spec
Define la configuración deseada del recurso.  
Es la sección donde se especifica el comportamiento, como contenedores, imágenes, reglas, etc.

#### 📁 RestartPolicy
Define cómo debe comportarse el Pod cuando un contenedor finaliza su ejecución (SOLO aplica a Pods).
* `Always` (valor por defecto): rebotará siempre que tenga alguna caída o pase algo.
* `OnFailure`: solo rebotará si falla.
* `Never`: nunca rebotará.

---

## 📌 Notas
