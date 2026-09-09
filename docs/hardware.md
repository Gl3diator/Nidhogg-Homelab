# Hardware

## Overview

Nidhogg is intentionally built on modest consumer hardware.

The server is powerful enough for learning Docker, Linux administration, networking, monitoring, media hosting, web development, and self-hosted applications while encouraging efficient resource usage.

---

## System Specifications

| Component | Specification |
| --- | --- |
| Hostname | `nidhogg` |
| CPU | Intel Core i3-7100 |
| Architecture | x86_64 |
| Memory | 8 GB DDR4-3200 |
| Storage | 1 TB HDD |
| Operating System | Ubuntu Server |
| Kernel | Linux 6.8 series |
| Container Runtime | Docker |

---

## CPU

Nidhogg uses an **Intel Core i3-7100**.

The processor is suitable for the current homelab's lightweight and moderate workloads, including:

- Docker containers
- monitoring
- file services
- web applications
- infrastructure experimentation
- lightweight automation

CPU usage should be monitored when several workloads are active simultaneously.

---

## Memory

Nidhogg has:

```text
8 GB DDR4-3200
```

Memory is one of the primary resource constraints of the system.

Services should therefore remain reasonably lightweight, especially when running multiple applications at the same time.

Current workloads include services for:

- monitoring
- media
- file management
- networking
- web hosting
- AI / automation

Beszel and Glances can be used to observe memory pressure and identify unusually heavy workloads.

---

## Storage

Nidhogg currently uses:

```text
1 TB HDD
```

The disk stores the operating system and homelab workloads, including service configuration and persistent application data.

Depending on service configuration, storage may also contain:

- media
- downloads
- Docker images
- Docker volumes
- container logs
- databases
- monitoring data
- application files

The exact partition layout and per-service storage paths should be documented only after they have been verified on the server.

---

## Storage Monitoring

Check filesystem capacity with:

```bash
df -h
```

Check Docker storage usage with:

```bash
docker system df
```

For a more detailed Docker breakdown:

```bash
docker system df -v
```

Check usage beneath `/srv` with:

```bash
sudo du -xh --max-depth=2 /srv 2>/dev/null | sort -h
```

These commands are read-only and do not delete data.

---

## Resource Constraints

The hardware encourages careful service selection and resource management.

Important considerations include:

### Memory

With 8 GB of RAM, unnecessary or duplicate services can create memory pressure.

### Storage

Media, downloads, Docker images, volumes, logs, and databases can consume HDD capacity over time.

### Disk Performance

A single HDD has significantly different performance characteristics from SSD or NVMe storage.

Workloads involving databases, many small files, heavy logging, or simultaneous media and download activity can compete for disk I/O.

### CPU

The i3-7100 is suitable for many homelab services, but CPU-intensive workloads should be monitored carefully.

---

## Monitoring

Hardware resource usage is primarily observed through:

- Beszel
- Glances
- standard Linux utilities

Useful command-line tools include:

```bash
free -h
```

```bash
uptime
```

```bash
lsblk
```

```bash
df -h
```

```bash
docker stats
```

`docker stats` runs continuously until stopped with `Ctrl+C`.

---

## Design Philosophy

Nidhogg is not intended to be enterprise infrastructure.

Its hardware is deliberately modest, making efficiency part of the learning process.

The system provides an environment for understanding:

- resource constraints
- service consolidation
- container overhead
- storage management
- network architecture
- monitoring
- troubleshooting
- capacity planning

The objective is to understand the infrastructure rather than hide complexity behind oversized hardware.

---

## Future Expansion

Potential hardware improvements can be evaluated when an actual workload requires them.

Typical areas to evaluate include:

- additional memory
- SSD storage
- additional bulk storage
- backup storage
- improved media storage capacity

Upgrades should be driven by measured resource constraints rather than added solely for complexity.

Beszel, Glances, and standard Linux metrics can help identify where the existing hardware becomes a bottleneck.
