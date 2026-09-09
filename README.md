<p align="center">
  <img src="img/nidhogg_logo.png" width="260" alt="Nidhogg Homelab"/>
</p>

<h1 align="center">Nidhogg Homelab</h1>

<p align="center">
  Single-node self-hosted lab for Docker, networking, infrastructure, media, monitoring, AI, and web development.
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#-services">Services</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-documentation">Documentation</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/OS-Ubuntu%20Server-E95420?style=flat-square&logo=ubuntu&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/Private%20Access-Tailscale-242424?style=flat-square&logo=tailscale&logoColor=white"/>
  <img src="https://img.shields.io/badge/Host-nidhogg-success?style=flat-square"/>
</p>

---

## Overview

**Nidhogg** is a single-node Ubuntu Server homelab used for learning and experimenting with:

- Docker and Docker Compose
- Linux administration
- networking and private remote access
- monitoring and observability
- self-hosted media
- storage and file sharing
- web application deployment
- reverse proxies and tunneling
- AI assistants and automation

The environment intentionally stays small and practical while providing enough infrastructure to experiment with real self-hosted services.

---

## 🧠 Architecture

Nidhogg combines Docker workloads with native host services.

```mermaid
flowchart TB
    N["🖥️ Nidhogg<br/>Ubuntu Server"]

    N --> Docker["🐳 Docker Compose"]
    N --> TS["🔐 Tailscale"]
    N --> SD["⚙️ systemd"]

    Docker --> MON["📊 Monitoring"]
    Docker --> MEDIA["🎬 Media"]
    Docker --> STORAGE["💾 Storage"]
    Docker --> WEB["🌐 Web"]

    MON --> BESZEL["Beszel"]
    MON --> NODE["Node Exporter"]
    MON --> CAD["cAdvisor"]
    MON --> GLANCES["Glances"]

    NODE --> PROM["Prometheus"]
    CAD --> PROM
    PROM --> GRAF["Grafana"]

    PROM -. "self-metrics" .-> PROM

    MEDIA --> JELLYFIN["Jellyfin"]
    MEDIA --> QBIT["qBittorrent"]

    STORAGE --> FB["File Browser"]
    STORAGE --> SAMBA["Samba"]

    WEB --> NGINX["Nginx Web"]

    TS --> PRIVATE["Private Remote Access"]

    SD --> OPENCLAW["OpenClaw Gateway"]
    OPENCLAW --> LILITH["😈 Lilith"]
```

Docker Compose definitions currently span:

- `/srv/compose/`
- `/srv/homelab/compose/`

OpenClaw is managed separately as a user-level systemd service.

---

## 🧱 Services

| Service | Role | Access |
| --- | --- | --- |
| **Beszel** | Lightweight infrastructure monitoring | Tailscale |
| **Beszel Agent** | Host/container metrics | Internal |
| **Beszel Socket Proxy** | Restricted Docker API access | Loopback |
| **Prometheus** | Metrics collection and time-series storage | Tailscale / private |
| **Grafana** | Monitoring dashboards | Tailscale / private |
| **Node Exporter** | Linux host metrics | Internal |
| **cAdvisor** | Docker/container metrics | Internal |
| **Glances** | Real-time system monitoring | LAN / private |
| **Jellyfin** | Media server | LAN / private |
| **qBittorrent** | Download client | Tailscale |
| **File Browser** | Web file management | LAN / private |
| **Samba** | Network file sharing | LAN |
| **Nginx Web** | Web server | Host port 80 |
| **Portainer** | Docker management UI | Private |
| **Tailscale** | Private overlay networking | Host |
| **OpenSSH** | Remote shell access | Host |
| **OpenClaw / Lilith** | Multi-agent personal assistant | Tailscale |
| **Symfony** | Web application development | Homelab project |
| **MariaDB** | Application database | Internal |
| **Cloudflare Tunnel** | Tunnel-based web publishing | Homelab project |

Administrative interfaces and databases are not intended for direct public Internet exposure.

---

## 🤖 Lilith

Nidhogg hosts **Lilith**, a self-hosted multi-agent personal assistant powered by OpenClaw.

OpenClaw runs as a user-level systemd service with its application gateway bound to loopback. Private remote access is provided through Tailscale Serve.

Lilith has its own reproducibility repository:

**[Gl3diator/Lilith](https://github.com/Gl3diator/Lilith)**

---

## 🌐 Networking

Nidhogg uses several networking layers depending on the service:

- **LAN** for local services and file sharing
- **Tailscale** for private remote access
- **Docker bridge networks** for container isolation and service communication
- **Loopback bindings** for host-local infrastructure endpoints
- **Tailscale Serve** for selected private HTTPS services
- **Cloudflare Tunnel and Nginx** for web-hosting experimentation

Sensitive administration interfaces are kept away from direct public Internet exposure.

---

## 📊 Monitoring

Nidhogg uses multiple monitoring tools for different levels of visibility.

### Beszel

Beszel provides lightweight infrastructure monitoring through:

- Beszel hub
- Beszel Agent
- restricted Docker socket proxy

The Beszel web interface is available privately through Tailscale.

### Prometheus & Grafana

Nidhogg also contains a traditional metrics stack consisting of:

- Prometheus for metrics collection and storage
- Grafana for dashboards and visualization
- Node Exporter for Linux host metrics
- cAdvisor for Docker container metrics

Prometheus and Grafana are configured for private Tailscale access.

### Glances

Glances provides quick real-time visibility into:

- CPU usage
- memory usage
- disk usage
- processes
- system load

Together these tools allow Nidhogg to experiment with both lightweight monitoring and a more traditional Prometheus/Grafana observability stack.

---

## 🎬 Media & Downloads

### Jellyfin

Jellyfin provides self-hosted media streaming from Nidhogg.

### qBittorrent

qBittorrent provides download management, with its web interface available privately through Tailscale.

---

## 💾 Storage & File Access

### Samba

Samba provides network file sharing across the local network.

### File Browser

File Browser provides browser-based management of server files.

Persistent service data and Compose configuration are kept separate where practical.

---

## 🖥️ Hardware

| Component | Specification |
| --- | --- |
| **CPU** | Intel Core i3-7100 |
| **RAM** | 8 GB DDR4-3200 |
| **Storage** | 1 TB HDD |
| **Architecture** | x86_64 |
| **OS** | Ubuntu Server |

The intentionally modest hardware makes resource efficiency an important part of the project.

---

## 📁 Server Layout

Docker Compose deployments currently span two locations:

```text
/srv/compose/
├── beszel/
└── samba/

/srv/homelab/compose/
├── filebrowser/
├── glances/
├── jellyfin/
├── monitoring/
├── portainer/
├── qbittorrent/
├── serinity-web/
└── web/
```

The repository itself contains documentation, application notes, and reproducible Compose examples for the homelab.

---

## 🔒 Access Model

Nidhogg favors private access for administration and infrastructure services.

### Private / Internal

Examples include:

- Beszel
- Glances
- File Browser
- Portainer
- qBittorrent Web UI
- OpenClaw / Lilith
- databases and infrastructure endpoints

Tailscale is the primary private remote-access layer.

### LAN

LAN access is used where appropriate for services such as:

- Samba
- Jellyfin
- File Browser

### Web Hosting

Nidhogg is also used to experiment with:

- Nginx
- Symfony
- MariaDB
- Cloudflare Tunnel
- reverse proxying
- tunnel-based application publishing

---

## 📚 Documentation

Detailed documentation lives under [`docs/`](docs/):

- [Architecture](docs/architecture.md)
- [Networking](docs/networking.md)
- [Services](docs/services.md)
- [Monitoring](docs/monitoring.md)
- [Storage](docs/storage.md)
- [Hardware](docs/hardware.md)

Service-specific notes and Compose examples are kept under `apps/` and `compose/`.

---

## 🎯 Goals

- Learn Linux server administration
- Understand Docker and Docker networking
- Build and operate self-hosted services
- Practice secure private networking
- Explore monitoring and observability
- Build media and storage infrastructure
- Deploy web applications
- Experiment with reverse proxies and tunnels
- Explore self-hosted AI assistants and automation
- Keep the environment reproducible and documented

---

## ⚠️ Security

This repository documents infrastructure but must not contain operational secrets.

The following should remain outside Git:

- passwords
- API keys
- authentication tokens
- private keys
- `.env` files
- database credentials
- tunnel credentials
- private OpenClaw runtime state

Administrative services, monitoring interfaces, databases, and Docker management endpoints are not intended for direct public Internet exposure.

---

<p align="center">
  <b>Nidhogg</b><br>
  Learn it. Host it. Break it. Rebuild it.
</p>
