<p align="center">
  <img src="img/nidhogg_logo.png" width="260"/>
</p>

<h1 align="center">Nidhogg Homelab</h1>

<p align="center">
  Personal homelab for learning infrastructure, Docker, and self-hosting
</p>

<p align="center">
  <a href="#overview">Overview</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-documentation">Documentation</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/OS-Ubuntu%2024.04-orange?style=flat-square"/>
  <img src="https://img.shields.io/badge/Docker-Enabled-blue?style=flat-square"/>
  <img src="https://img.shields.io/badge/Access-Tailscale-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Public-Cloudflare%20Tunnel-purple?style=flat-square"/>
  <img src="https://img.shields.io/badge/status-active-success?style=flat-square"/>
</p>

---

## Overview

This homelab is a **single-node Docker-based environment** used to experiment with self-hosting, networking, and infrastructure concepts.

It focuses on simplicity, private access, and iterative learning rather than production complexity.

It currently follows a **hybrid exposure model**:

- **Public web apps** are exposed through **Cloudflare Tunnel** and a central **Nginx reverse proxy**
- **Internal tools** remain private through **Tailscale**

---

## 🧠 Architecture

<p align="center">
  <img src="img/architecture.png" width="750"/>
</p>

---

## 🧱 Tech Stack

<table>
  <tr>
    <th>Logo</th>
    <th>Name</th>
    <th>Description</th>
  </tr>

  <tr>
    <td><img width="32" src="https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png"></td>
    <td><a href="https://www.docker.com">Docker</a></td>
    <td>Containerization platform for running services</td>
  </tr>

  <tr>
    <td><img width="32" src="https://cdn.worldvectorlogo.com/logos/portainer.svg"></td>
    <td><a href="https://www.portainer.io">Portainer</a></td>
    <td>Web UI for managing Docker containers</td>
  </tr>

  <tr>
    <td><img width="32" src="https://avatars.githubusercontent.com/u/48932923?s=200&v=4"></td>
    <td><a href="https://tailscale.com">Tailscale</a></td>
    <td>Private VPN for secure remote access</td>
  </tr>

  <tr>
    <td><img width="32" src="https://nicolargo.github.io/glances/public/images/glances.png"></td>
    <td><a href="https://nicolargo.github.io/glances/">Glances</a></td>
    <td>Lightweight real-time system monitoring</td>
  </tr>

  <tr>
    <td><img width="32" src="https://cdn.jsdelivr.net/gh/walkxcode/dashboard-icons/png/filebrowser.png"></td>
    <td><a href="https://filebrowser.org">File Browser</a></td>
    <td>Web-based file manager for server storage</td>
  </tr>

  <tr>
    <td><img width="32" src="https://nginx.org/nginx.png"></td>
    <td><a href="https://nginx.org">Nginx</a></td>
    <td>Used both as a central reverse proxy and as the web server</td>
  </tr>

  <tr>
    <td><img width="32" src="https://upload.wikimedia.org/wikipedia/commons/9/94/Cloudflare_Logo.png"></td>
    <td><a href="https://www.cloudflare.com">Cloudflare Tunnel</a></td>
    <td>Secure public exposure without router port forwarding</td>
  </tr>

  <tr>
    <td><img width="32" src="https://symfony.com/logos/symfony_black_03.svg"></td>
    <td><a href="https://symfony.com">Symfony</a></td>
    <td>Main public web application stack</td>
  </tr>

  <tr>
    <td><img width="32" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTyDh73gbtgz6QSkrrW7IlBbo8pcYp3jpJ-3w&s" ></td>
    <td><a href="https://mariadb.org">MariaDB</a></td>
    <td>Database backend for the Symfony application</td>
  </tr>

  <tr>
    <td><img width="32" src="https://assets.ubuntu.com/v1/29985a98-ubuntu-logo32.png"></td>
    <td><a href="https://ubuntu.com/server">Ubuntu Server</a></td>
    <td>Base operating system</td>
  </tr>

</table>

---

## 📊 System

- **CPU**: Intel i3-7100  
- **RAM**: 8GB DDR4-3200  
- **Storage**: 1TB HDD  

---

## 🔒 Access Model

### Internal Only (Tailscale / LAN)
- Portainer
- Glances
- File Browser

### Public
- Symfony application
- Routed through Cloudflare Tunnel → Nginx reverse proxy

---

## Project Structure

```text
homelab/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── SECURITY.md
│
├── img/
│   ├── nidhogg_logo.png
│   └── architecture.png
│
├── docs/
│   ├── architecture.md
│   ├── networking.md
│   ├── services.md
│   ├── monitoring.md
│   ├── storage.md
│   └── hardware.md
│
├── apps/
│   ├── portainer/
│   │   └── README.md
│   ├── glances/
│   │   └── README.md
│   ├── tailscale/
│   │   └── README.md
│   ├── filebrowser/
│   │   └── README.md
│   ├── symfony-app/
│   │   └── README.md
│   ├── nginx/
│   │   └── README.md
│   └── mariadb/
│       └── README.md
│
└── compose/
    ├── portainer.yml
    ├── glances.yml
    ├── filebrowser/
    │   └── docker-compose.yml
    ├── symfony-app/
    │   └── docker-compose.yml
    ├── nginx_proxy/
    │   ├── docker-compose.yml
    │   └── nginx/
    │       └── conf.d/
    │           └── symfony.conf
    └── cloudflared/
        └── docker-compose.yml
```

---

## 📚 Documentation

- [Architecture](docs/architecture.md)
- [Networking](docs/networking.md)
- [Services](docs/services.md)
- [Monitoring](docs/monitoring.md)
- [Storage](docs/storage.md)
- [Hardware](docs/hardware.md)

---

## 📌 Goals

- Learn Docker networking and containerization  
- Build scalable service architecture  
- Deploy APIs and web applications  
- Practice reverse proxy and tunnel-based public hosting  

---

## ⚠️ Status

Project status: **Active / Learning Phase**

This homelab evolves as new tools and concepts are explored.