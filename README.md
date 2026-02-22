# Dockhand – Contenedor Docker

**Dockhand** es una plataforma moderna para la gestión de Docker, diseñada para ser potente, intuitiva y accesible para todos.

✔ Perfecto para homelabs  
✔ Ideal para pequeñas empresas  
✔ Preparado para entornos enterprise  
✔ Sin telemetría  
✔ Base de datos SQLite por defecto  
✔ Gratuito para siempre en uso personal  

Dockhand permite gestionar contenedores, imágenes y recursos Docker desde una interfaz web moderna sin la complejidad de herramientas empresariales tradicionales.

---

## 🚀 Características

- Imagen oficial: `fnsys/dockhand:latest`
- Interfaz web moderna y potente
- SQLite integrado por defecto
- Compatible incluso con Raspberry Pi
- Sin telemetría
- Persistencia de datos mediante volumen Docker
- Reinicio automático (`unless-stopped`)

---

## 🐳 docker-compose.yml

```yaml
services:
  dockhand:
    image: fnsys/dockhand:latest
    container_name: dockhand
    restart: unless-stopped
    ports:
      - 3000:3000
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - dockhand_data:/app/data

volumes:
  dockhand_data:
```

---

## 🔐 Seguridad

Dockhand necesita acceso al socket Docker:

```
/var/run/docker.sock
```

⚠️ Esto otorga control completo sobre el host Docker.

Recomendaciones:

- No exponer directamente a Internet
- Proteger mediante firewall o VPN
- Colocar detrás de reverse proxy con autenticación (Nginx, Traefik, Caddy)

---

## 🌐 Acceso a la interfaz web

Una vez iniciado el contenedor:

```
http://TU-IP:3000
```

---

## ▶️ Puesta en marcha

```bash
docker compose up -d
```

---

## 🛑 Detener el contenedor

```bash
docker compose down
```

---

## 🔄 Actualizar Dockhand

```bash
docker compose pull
docker compose up -d
```

---

## 📦 Persistencia de datos

Los datos se almacenan en el volumen:

```
dockhand_data
```

Puedes inspeccionarlo con:

```bash
docker volume inspect dockhand_data
```
