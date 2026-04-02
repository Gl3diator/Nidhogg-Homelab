# Homelab Architecture

## Overview

This homelab follows a **hybrid access model**:

- **Public services** are exposed through **Cloudflare Tunnel** and a central **Nginx reverse proxy**
- **Internal services** remain accessible only through **Tailscale** or the local network

The setup is built around Docker and separates:
- public web hosting
- internal administration
- storage and app data

---

##  Flow

```text
Internet
  ↓
Cloudflare Tunnel (cloudflared)
  ↓
Nginx Reverse Proxy (nginx_proxy)
  ↓
Symfony App Web Server (symfony_nginx)
  ↓
PHP-FPM (symfony_php)
  ↓
MariaDB (symfony_db)
```

---

## Public Layer

### Cloudflare Tunnel
Cloudflare Tunnel is used to expose public services without opening ports on the router.

It acts as the secure external entrypoint for the web application.

### Nginx Reverse Proxy
The central `nginx_proxy` container receives traffic from Cloudflare Tunnel and forwards requests to the correct internal application.

This design makes it easier to host multiple web applications later.

---

## Application Layer

### Symfony Stack
The Symfony application is deployed as a dedicated multi-container stack:

- `symfony_nginx` → web server
- `symfony_php` → PHP-FPM runtime
- `symfony_db` → MariaDB database

This separates:
- HTTP serving
- PHP execution
- database persistence

---

## Internal Services

These services are not part of the public web path and remain private:

- **Portainer** → Docker management
- **Glances** → system monitoring
- **File Browser** → file management
- **Tailscale** → secure private access

---

## Access Model

### Public
Public traffic follows this path:

```text
Cloudflare Tunnel
  → nginx_proxy
  → symfony_nginx
  → symfony_php
  → symfony_db
```

### Private
Internal tools are accessed only through:
- Tailscale
- local network

They are not intended to be exposed publicly.

---

## Service Roles

### `cloudflared`
Provides public access to the homelab web application through a secure outbound tunnel.

### `nginx_proxy`
Acts as the front-facing reverse proxy for public apps.

### `symfony_nginx`
Acts as the application web server for Symfony.

### `symfony_php`
Runs PHP-FPM and executes the Symfony application.

### `symfony_db`
Stores the database used by the Symfony application.

### `filebrowser`
Provides a web-based interface for browsing and managing files under `/srv`.

### `portainer`
Provides Docker management through a web interface.

### `glances`
Provides lightweight system monitoring.

### `tailscale`
Provides secure private access to internal services.

---

## Runtime Layout

### Application Code
```text
/srv/apps/Technologies-Web-2.0
```

### Compose Configs
```text
/srv/compose/symfony-app
/srv/compose/nginx_proxy
/srv/compose/cloudflared
/srv/compose/filebrowser
```

### Persistent Data
```text
/srv/data/symfony-db
```

---

## Design Goals

This architecture was chosen to support:

- iterative learning
- simple service separation
- safer public exposure
- future multi-app hosting
- clean Docker-based deployment

---

## Notes

- Cloudflare Tunnel is currently used with a temporary TryCloudflare URL
- A real domain will later replace the temporary URL without changing the main architecture
