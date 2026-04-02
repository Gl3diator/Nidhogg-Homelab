# Services

## Overview

This document lists the main services currently used in the homelab and their roles.

---

## Portainer
**Purpose:** Manage Docker containers through a web interface

- Access: Tailscale / LAN only
- Exposure: Private
- Notes: Used for container visibility, container management, and quick service checks

---

## Glances
**Purpose:** Lightweight system monitoring

- Access: Tailscale / LAN only
- Exposure: Private
- Notes: Used to monitor CPU, RAM, processes, and system load

---

## File Browser
**Purpose:** Web-based file manager for server storage

- Access: Tailscale / LAN only
- Exposure: Private
- Notes: Used to manage `/srv`, application files, and compose directories

---

## Tailscale
**Purpose:** Secure private access to the homelab

- Access: Private VPN
- Exposure: Private
- Notes: Used as the main access layer for internal/admin services

---

## Cloudflared
**Purpose:** Public tunnel entrypoint

- Access: Public path
- Exposure: Public
- Notes: Exposes the Symfony application through Cloudflare Tunnel without router port forwarding

---

## Nginx Proxy
**Purpose:** Central reverse proxy for public applications

- Access: Public path
- Exposure: Public
- Notes: Receives traffic from Cloudflare Tunnel and forwards it to the appropriate internal web service

---

## Symfony Nginx
**Purpose:** Web server for the Symfony application

- Access: Internal stack
- Exposure: Internal (plus temporary direct testing on `8082`)
- Notes: Serves static assets and forwards PHP requests to PHP-FPM

---

## Symfony PHP-FPM
**Purpose:** PHP runtime for the Symfony application

- Access: Internal stack only
- Exposure: Internal
- Notes: Executes the Symfony application code

---

## MariaDB
**Purpose:** Database backend for Symfony

- Access: Internal stack only
- Exposure: Internal
- Notes: Stores application data and should be treated as persistent state

---

## Summary

### Public Services
- Cloudflared
- Nginx Proxy
- Symfony application

### Private Services
- Portainer
- Glances
- File Browser
- Tailscale
- MariaDB