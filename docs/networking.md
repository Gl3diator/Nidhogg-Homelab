# Networking

## Overview

This homelab uses a split networking model:

- **Public web traffic** is routed through Cloudflare Tunnel and a central Nginx reverse proxy
- **Internal services** are accessed through Tailscale or the local network only

This avoids exposing admin tools directly to the internet.

---

## Public Path

```text
Internet
  ↓
Cloudflare Tunnel
  ↓
nginx_proxy
  ↓
symfony_nginx
  ↓
symfony_php
  ↓
symfony_db
```

### Publicly Reachable Service
- Symfony web application

### Public Exposure Method
- Cloudflare Tunnel
- Nginx reverse proxy
- No direct router port forwarding required

---

## Internal Path

### Tailscale / LAN Only
- Portainer
- Glances
- File Browser

These services are intentionally kept off the public path.

---

## Docker Networking

### Main Networks
- `symfony-app_default` → Symfony stack internal communication
- `proxy_proxy` → Cloudflared and Nginx proxy communication

### Service-to-Service Flow
- `cloudflared` connects to `nginx_proxy`
- `nginx_proxy` connects to `symfony_nginx`
- `symfony_nginx` connects to `symfony_php`
- `symfony_php` connects to `symfony_db`

---

## Port Usage

### Public
- `80` → central Nginx reverse proxy

### Internal / Admin
- `8081` → File Browser
- `9000` → Portainer
- `8082` → Symfony Nginx (temporary direct access for testing)

---

## Security Notes

- Public traffic should only reach the reverse proxy path
- Admin services remain accessible through Tailscale or LAN only
- Temporary direct ports like `8082` should eventually be removed when no longer needed
- Cloudflare Tunnel is preferred over opening home router ports directly

---

## Future Direction

When a real domain is added:

- Cloudflare Tunnel can route a stable hostname to `nginx_proxy`
- Symfony can move off temporary TryCloudflare URLs
