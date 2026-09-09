# Services

## Overview

Nidhogg hosts a collection of self-hosted services for monitoring, media, storage, networking, web development, administration, and AI.

Docker workloads are primarily managed with Docker Compose, while some host-level services are managed through systemd.

---

## Service Summary

| Service | Purpose | Access |
| --- | --- | --- |
| Beszel | Infrastructure monitoring | Tailscale |
| Beszel Agent | Host and container metrics | Internal |
| Beszel Socket Proxy | Restricted Docker API access | Loopback |
| Glances | Real-time system monitoring | LAN / private |
| Prometheus | Metrics collection and storage | Tailscale / private |
| Grafana | Monitoring dashboards | Tailscale / private |
| Node Exporter | Linux host metrics | Internal |
| cAdvisor | Docker container metrics | Internal |
| Jellyfin | Media streaming | LAN / private |
| qBittorrent | Download management | Tailscale |
| File Browser | Web-based file management | LAN / private |
| Samba | Network file sharing | LAN |
| Nginx Web | General web serving | Host port 80 |
| Portainer | Docker management | Private |
| Tailscale | Private overlay networking | Host |
| OpenSSH | Remote shell access | Host |
| OpenClaw / Lilith | Multi-agent personal assistant | Tailscale |
| Symfony | Web application development | Application stack |
| MariaDB | Application database | Internal |
| Cloudflare Tunnel | Tunnel-based web publishing | Web infrastructure |

---

## Beszel

**Purpose:** Lightweight infrastructure monitoring

Beszel provides monitoring for Nidhogg through a small multi-container stack.

### Components

- `beszel`
- `beszel-agent`
- `beszel-socket-proxy`

### Access

The Beszel interface is bound to the Tailscale interface on port `8090`.

### Docker Access

The Beszel socket proxy exposes a restricted Docker API endpoint locally on:

```text
127.0.0.1:2375
```

This avoids exposing the Docker API broadly across the network.

### Compose

```text
/srv/compose/beszel/docker-compose.yml
```

---

## Glances

**Purpose:** Real-time host monitoring

Glances provides visibility into:

- CPU
- memory
- disks
- processes
- system load

### Access

Glances listens on port:

```text
61208
```

It is intended for LAN or private administrative access.

### Compose

```text
/srv/homelab/compose/glances/docker-compose.yml
```

---

## Jellyfin

**Purpose:** Self-hosted media streaming

Jellyfin provides media playback and library access to devices on the network.

### Access

Jellyfin publishes:

```text
8096/tcp
```

### Compose

```text
/srv/homelab/compose/jellyfin/docker-compose.yml
```

---

## qBittorrent

**Purpose:** Download management

qBittorrent provides torrent/download management for the homelab.

### Web Interface

The Web UI is bound to the Tailscale interface on:

```text
8083/tcp
```

### Container Ports

The container also exposes:

```text
6881/tcp
6881/udp
```

### Compose

```text
/srv/homelab/compose/qbittorrent/docker-compose.yml
```

The Web UI is intended to remain private.

---

## File Browser

**Purpose:** Browser-based file management

File Browser provides a web interface for managing server files.

### Access

The service publishes:

```text
8081/tcp
```

It is intended for LAN or private administrative access.

### Compose

```text
/srv/homelab/compose/filebrowser/docker-compose.yml
```

File Browser should not be exposed directly to the public Internet.

---

## Samba

**Purpose:** LAN file sharing

Samba provides network shares to local devices.

### Access

Samba is intended for the local network rather than public Internet access.

### Compose

```text
/srv/compose/samba/docker-compose.yml
```

Share paths and credentials are intentionally not documented in the public repository.

---

## Nginx Web

**Purpose:** General web serving

The `nidhogg-web` container uses:

```text
nginx:alpine
```

and publishes:

```text
80/tcp
```

on the host.

### Compose

```text
/srv/homelab/compose/web/docker-compose.yml
```

Tailscale Serve can proxy private HTTPS traffic to the local service on port `80`.

---

## Portainer

**Purpose:** Docker administration

Portainer provides a graphical interface for Docker management.

### Access

Portainer is an administrative interface and should remain private through LAN or Tailscale access.

### Compose

```text
/srv/homelab/compose/portainer/docker-compose.yml
```

The Docker socket and Portainer interface must not be exposed directly to the public Internet.

---

## Tailscale

**Purpose:** Private remote access

Tailscale provides the primary private networking layer for Nidhogg.

It is installed directly on the host rather than deployed as a Docker container.

### Uses

Tailscale provides private access to services including:

- Beszel
- qBittorrent
- OpenClaw / Lilith
- selected web services through Tailscale Serve

### Service

```text
tailscaled.service
```

---

## OpenSSH

**Purpose:** Remote shell administration

OpenSSH provides command-line access to Nidhogg.

### Service

```text
ssh.service
```

SSH access should be protected with appropriate authentication and network controls.

---

## OpenClaw / Lilith

**Purpose:** Self-hosted multi-agent personal assistant

OpenClaw hosts Lilith directly on Nidhogg.

Unlike most Nidhogg workloads, OpenClaw is not currently deployed through Docker Compose.

### Service

OpenClaw runs as a user-level systemd service:

```text
openclaw-gateway.service
```

### Gateway

The application gateway listens locally on:

```text
127.0.0.1:18789
[::1]:18789
```

Tailscale Serve provides private remote access to the gateway.

### Lilith Repository

Lilith's reproducible, non-secret configuration is maintained separately:

https://github.com/Gl3diator/Lilith

Private OpenClaw runtime state and credentials are not stored in the public homelab repository.

---

## Monitoring Stack

Nidhogg also contains a dedicated monitoring Compose project:

```text
/srv/homelab/compose/monitoring/docker-compose.yml
```

This project is separate from the Beszel and Glances Compose definitions and remains part of the homelab configuration.

---

## Serinity Web

Nidhogg contains a `serinity-web` Compose project for web application experimentation.

### Compose

```text
/srv/homelab/compose/serinity-web/docker-compose.yml
```

A dedicated Docker network is also present:

```text
serinity_internal
```

Application-specific details should be documented alongside the project as the stack evolves.

---

## Symfony

**Purpose:** Web application development

Symfony is used as part of Nidhogg's application-hosting and development experiments.

The project explores a multi-container application architecture involving:

- Nginx
- PHP-FPM
- Symfony
- MariaDB

Symfony-specific application documentation is kept separately under the repository's application documentation.

---

## MariaDB

**Purpose:** Application database

MariaDB provides relational database storage for web application experiments such as the Symfony stack.

Database services are infrastructure components and should remain internal.

Database credentials and persistent database contents must not be committed to Git.

---

## Cloudflare Tunnel

**Purpose:** Tunnel-based web publishing

Cloudflare Tunnel is part of Nidhogg's experimentation with securely publishing web applications without direct router port forwarding.

A typical experimental flow is:

```text
Internet
    ↓
Cloudflare Tunnel
    ↓
Nginx
    ↓
Web Application
```

Cloudflare credentials and tunnel tokens must remain outside the repository.

Administrative services should not be routed through the public tunnel.

---

## Security Principles

The following interfaces should remain private:

- Portainer
- Glances
- File Browser
- Beszel
- qBittorrent Web UI
- MariaDB
- OpenClaw
- Docker API / socket proxy

Nidhogg prefers:

- Tailscale for remote administration
- LAN access for local services
- loopback for host-local endpoints
- Docker networks for internal container communication
- tunnel-based publishing only for intentionally public web applications

Secrets, passwords, tokens, private keys, and runtime credentials are not documented in this repository.
