# 🌐 Symfony App

## Overview
This is the main public web application currently hosted in the homelab.

It runs as a Dockerized stack behind a reverse proxy and is exposed publicly through Cloudflare Tunnel.

---

## Stack Components
- Nginx
- PHP-FPM
- MariaDB

---

## Access Model
- Public entrypoint goes through:
  - Cloudflare Tunnel
  - central Nginx reverse proxy


---

## Purpose
- Learn real-world web hosting flow
- Deploy a PHP/Symfony application in containers
- Practice reverse proxy and public routing

---

## Runtime Layout
- App code lives under `/srv/apps/Technologies-Web-2.0` (same app on my git)
- Stack config lives under `/srv/compose/symfony-app`

---

## Notes
- currently using temprarry "trycloudflare.com" tunnel
- Long-term plan is to use a real domain name
