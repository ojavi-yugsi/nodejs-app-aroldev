# CI/CD Lab with Jenkins and ArgoCD

Este laboratorio te guiará a través de la configuración de un pipeline CI/CD que automatiza el despliegue de una aplicación Node.js utilizando **Jenkins** y **ArgoCD**, aplicando principios GitOps. Trabajarás con dos repositorios: uno para el código de la aplicación y otro para el **Helm chart** que gestiona los despliegues en Kubernetes.

## Tabla de Contenidos

- [Descripción del Flujo CI/CD](#descripción-del-flujo-cicd)
- [Repositorios](#repositorios)
- [Instrucciones del Laboratorio](#instrucciones-del-laboratorio)
  - [1. Forkea Ambos Repositorios](#1-forkea-ambos-repositorios)
  - [2. Crear un Dockerfile](#2-crear-un-dockerfile)
  - [3. Crear el Pipeline en Jenkins](#3-crear-el-pipeline-en-jenkins)
  - [4. Configurar ArgoCD](#4-configurar-argocd)
  - [5. Actualizar la Aplicación para Desplegar](#5-actualizar-la-aplicación-para-desplegar)
- [Resultado Final](#resultado-final)
- [Enlaces Útiles](#enlaces-útiles)

---

## Descripción del Flujo CI/CD

### 1. **Gestión de Código en GitHub**
   - Los desarrolladores realizan cambios en el repositorio de código fuente de la **aplicación Node.js**.
   - Jenkins es notificado (webhook o polling) cuando se detectan cambios.

### 2. **Pipeline CI en Jenkins**
   - Jenkins construye la aplicación y crea una imagen Docker.
   - La imagen Docker es subida a un registro de contenedores (Docker Hub, ECR, etc.).
   - Jenkins actualiza el repositorio **GitOps** (Helm chart) con la nueva etiqueta de la imagen Docker.
   - Jenkins hace commit de estos cambios en el repositorio GitOps.

### 3. **Monitoreo y Sincronización con ArgoCD**
   - **ArgoCD** monitorea el repositorio GitOps y detecta cuando se realizan cambios.
   - ArgoCD sincroniza los cambios con el clúster de Kubernetes, desplegando la nueva versión de la aplicación.

### 4. **Despliegue en Kubernetes**
   - Kubernetes aplica los cambios y realiza un **rolling update**, reemplazando gradualmente los pods antiguos con los nuevos.
   - Si hay algún problema, ArgoCD o el administrador pueden hacer un rollback a una versión anterior usando GitOps.

---

## Repositorios

- **Node.js Application**: [nodejs-app-aroldev](https://github.com/aroldev/nodejs-app-aroldev)
- **Helm Chart (Infrastructure)**: [nodejs-app-aroldev-infra](https://github.com/aroldev/nodejs-app-aroldev-infra)

Forkea ambos repositorios a tu cuenta de GitHub antes de empezar el laboratorio.

---

## Instrucciones del Laboratorio

### 1. Forkea Ambos Repositorios

1. Ve a GitHub y haz un fork de estos repositorios:
   - [Repositorio de la Aplicación Node.js](https://github.com/aroldev/nodejs-app-aroldev)
   - [Repositorio de Infraestructura Helm Chart](https://github.com/aroldev/nodejs-app-aroldev-infra)

2. Clona tus forks localmente:
   ```bash
   git clone https://github.com/<tu-usuario>/nodejs-app-aroldev.git
   git clone https://github.com/<tu-usuario>/nodejs-app-aroldev-infra.git