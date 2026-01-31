# Dockhand 🐳 | Gestión eficiente de Docker

***Dockhand 🐳***

Dockhand es una aplicación moderna y eficiente para la gestión de Docker, que ofrece control en tiempo real de contenedores, orquestación de stacks Docker Compose y soporte multi-entorno. Todo en un paquete ligero, seguro y centrado en la privacidad.

---

## ✨ Características principales


✨ Características principales

Gestión de contenedores

Iniciar, detener, reiniciar y monitorizar contenedores en tiempo real.

Stacks Docker Compose

Editor visual para desplegar y gestionar archivos docker-compose.yml.

Integración con Git

Despliegue de stacks desde repositorios Git.

Soporte para webhooks y auto-sincronización.

Multi-entorno

Gestión de hosts Docker locales y remotos desde una única interfaz.

Terminal y logs

Acceso interactivo a shells dentro de los contenedores.

Visualización de logs en tiempo real.

Explorador de archivos

Navegar, subir y descargar archivos directamente desde los contenedores.

Autenticación

Usuarios locales y SSO vía OIDC.

RBAC opcional en versión Enterprise.

## 🧱 Stack tecnológico

Base del sistema

Capa de sistema propia construida desde cero usando paquetes Wolfi mediante apko.

Todos los paquetes se declaran explícitamente en el Dockerfile.

Frontend

SvelteKit 2

Svelte 5

shadcn-svelte

TailwindCSS

Backend

Bun runtime

API basada en rutas de SvelteKit

Base de datos

SQLite o PostgreSQL

ORM: Drizzle

Docker

Acceso directo a la Docker API (sin agentes intermedios)

## 📦 Requisitos

Docker

Docker Compose (plugin o binario)

Acceso al socket de Docker (/var/run/docker.sock)

(Opcional) Servidor PostgreSQL si no se usa SQLite

---

## 🐳 Despliegue con Docker Compose

Este repositorio utiliza la imagen oficial de **HomeDock OS** y monta los volúmenes necesarios para persistencia de datos, configuración y gestión de stacks Docker.

### 📄 docker-compose.yml

```yaml
services:

  dockhand:
    image: fnsys/dockhand:latest
    container_name: dockhand
    restart: unless-stopped
    ports:
      - "8200:3000"
    environment:
      DATABASE_URL: postgres://dockhand:changeme@postgres:5432/dockhand
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./dockhand_data:/app/data
    depends_on:
      - postgres

  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: dockhand
      POSTGRES_PASSWORD: changeme
      POSTGRES_DB: dockhand
    volumes:
      - ./postgres_data:/var/lib/postgresql/data
```
---

## 📁 Estructura de volúmenes
```
.
├── docker-compose.yml
├── dockhand_data/
│   ├── db/
│   ├── config/
│   ├── sessions/
│   ├── uploads/
│   ├── logs/
│   └── cache/
│
├── postgres_data/
│   ├── base/
│   ├── global/
│   ├── pg_wal/
│   ├── pg_multixact/
│   ├── pg_commit_ts/
│   └── postgresql.conf

```
### 🔹 dockhand_data:/app/data

Este volumen almacena todos los datos persistentes de Dockhand:

Configuración de la aplicación

Estados de stacks y entornos

Metadatos de contenedores

Logs y caché

Archivos gestionados desde el File Browser

👉 Es imprescindible para no perder datos al recrear el contenedor.

### 🔹 postgres_data:/var/lib/postgresql/data

Volumen estándar de PostgreSQL:

Bases de datos

Usuarios y roles

Configuración interna

WAL y metadatos

👉 Si borras este volumen, pierdes la base de datos completa.

### ⚠️ Volumen especial: Docker Socket

```yaml
- /var/run/docker.sock:/var/run/docker.sock
````

Permite a Dockhand gestionar Docker directamente en el host

No es persistente

Concede privilegios elevados

## 🚀 Puesta en marcha

1. Crea la estructura de carpetas:
```bash
mkdir -p dockhand_data postgres_data
```

2. Inicia el contenedor:
```bash
docker compose up -d
```

3. Accede a HomeDock OS desde tu navegador:
```
http://TU_IP:8200
```

## 🔐 Seguridad y buenas prácticas

El acceso al socket de Docker otorga control total sobre el host.

Asegura el acceso a Dockhand con autenticación fuerte.

Cambia las credenciales por defecto de PostgreSQL.

Para entornos expuestos a Internet:

Usa un reverse proxy (Nginx, Traefik, Caddy).

Habilita HTTPS.

Considera OIDC para SSO.

### 🌐 Reverse proxy (opcional)

Dockhand es totalmente compatible con reverse proxies.
Simplemente expón el puerto interno 3000 detrás de tu proxy favorito y añade HTTPS.

### 📁 Persistencia de datos

./dockhand_data
Datos internos y configuración de Dockhand.

./postgres_data
Datos persistentes de PostgreSQL.

### 🧭 Casos de uso habituales

Sustituto moderno de Portainer.

Gestión centralizada de múltiples hosts Docker.

Despliegue continuo de stacks desde Git.

Entornos homelab y profesionales.

---

## 🔄 Actualización

```bash
docker compose pull
docker compose up -d
```
---

## 📘 Recursos

- Repositorio oficial: https://github.com/BansheeTech/HomeDockOS
- Docker Hub: https://hub.docker.com/r/bansheetech/homedock-os
---

## 👤 Autor

README generado para **JLalib** siguiendo el método **README Pro GitHub**.
