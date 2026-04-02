# 🌐 Nginx

## Overview
Nginx is used in this homelab in two different roles:

1. As a reverse proxy for public traffic
2. As the web server inside the Symfony application stack

---

## Roles

### 1. Central Reverse Proxy
The `nginx_proxy` service is the public entrypoint for hosted web applications.

It receives incoming traffic from Cloudflare Tunnel and forwards requests to the correct internal service.

### 2. Symfony Web Server
The `symfony_nginx` service is the application web server for Symfony.

It serves static files and forwards PHP requests to the PHP-FPM container.

---

## Why Nginx
Nginx was chosen because it is:
- lightweight
- fast
- widely used
- ideal for reverse proxying
- well suited for containerized PHP apps

---

## Access Model

### Public
- `nginx_proxy`
- receives traffic from Cloudflare Tunnel

### Internal
- `symfony_nginx`
- only used inside the Symfony stack

---

## Runtime Layout
- Proxy stack: `/srv/compose/nginx_proxy`
- Symfony stack: `/srv/compose/symfony-app`

---

## Notes
- Nginx is used both as a reverse proxy and as a web server
- The proxy layer and app layer are intentionally separated
- This makes it easier to host multiple apps later
