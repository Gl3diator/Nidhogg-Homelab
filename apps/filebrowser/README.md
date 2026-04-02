# 📁 File Browser

## Overview
File Browser is a web-based file manager for the homelab.

---

## Purpose
- Browse server files
- Upload / download files
- Edit files quickly
- Manage project directories

---

## Access
- URL: http://<server-ip>:8081
- Access: LAN / Tailscale ONLY
- Not exposed publicly

---

## Security
- Never exposed through Nginx or Cloudflare
- Only accessible internally

---

## Storage
- Root directory: /srv
- Database stored locally in container volume

---

## Notes
Used to manage:
- Symfony app files
- Docker configs
- General server storage
