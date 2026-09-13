# Storage

## Overview

Nidhogg separates service configuration, application files, and persistent data where practical.

The homelab has evolved over time, so Docker Compose projects currently exist under both:

- `/srv/compose/`
- `/srv/homelab/compose/`

This document describes the current known layout without assuming that every service uses the same storage structure.

---

## Current Compose Layout

Known Compose projects are located at:

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

Each directory contains the Compose definition for its respective stack.

---

## Directory Roles

Nidhogg uses several `/srv` locations for different purposes.

| Path | Purpose |
| --- | --- |
| `/srv/apps/` | Application source code where applicable |
| `/srv/compose/` | Docker Compose and infrastructure configuration |
| `/srv/data/` | Persistent service/application data where applicable |
| `/srv/homelab/compose/` | Existing Docker Compose deployments |

The `/srv/apps`, `/srv/compose`, and `/srv/data` separation remains a useful organizational pattern, but the current server also contains established deployments under `/srv/homelab/compose`.

Existing services do not need to be moved simply for documentation consistency.

---

## Persistent Data

Container configuration and persistent data should be treated separately.

Persistent data can include:

- databases
- media libraries
- application state
- configuration files
- downloads
- monitoring data
- file shares

The exact host paths used by each service depend on its Compose configuration.

This repository should not claim specific bind-mount or volume paths unless they have been verified from the running configuration.

---

## Media Storage

Jellyfin consumes media stored on Nidhogg.

qBittorrent manages downloaded content that may later be consumed by media services.

The exact media and download directories are intentionally not assumed here.

When documenting these paths, they should be verified directly from the relevant Compose files:

```bash
cd /srv/homelab/compose/jellyfin
docker compose config
```

and:

```bash
cd /srv/homelab/compose/qbittorrent
docker compose config
```

These commands render the effective Compose configuration without changing the running containers.

---

## Samba Storage

Samba provides network file sharing from Nidhogg.

The Samba Compose definition is located at:

```text
/srv/compose/samba/docker-compose.yml
```

Share paths, usernames, and credentials are not documented here.

Share paths should be verified from the server configuration before being added to public documentation.

Credentials must never be committed to the repository.

---

## File Browser Storage

File Browser provides browser-based file management.

Its Compose definition is located at:

```text
/srv/homelab/compose/filebrowser/docker-compose.yml
```

The directories visible through File Browser depend on the bind mounts configured for the container.

Those paths should be documented only after verification from the Compose configuration.

---

## Database Storage

MariaDB is used by application experiments such as the Symfony stack.

Database storage must remain persistent across container recreation.

Database data should be stored using a Docker volume or an appropriate persistent host directory.

The following must never be committed to Git:

- database passwords
- database dumps containing private data
- `.env` files containing credentials
- application secrets

Before changing or deleting database storage, create and verify a backup.

---

## Docker Volumes

Docker-managed volumes may also contain persistent application state.

List existing volumes with:

```bash
docker volume ls
```

Inspect a specific volume with:

```bash
docker volume inspect VOLUME_NAME
```

These commands are read-only.

Do not remove a Docker volume unless its ownership and contents have been identified and any important data has been backed up.

---

## Storage Inspection

### Check Filesystem Usage

```bash
df -h
```

### Check `/srv` Usage

```bash
sudo du -xh --max-depth=2 /srv 2>/dev/null | sort -h
```

This can help identify which service directories consume the most disk space.

### Check Docker Disk Usage

```bash
docker system df
```

For additional detail:

```bash
docker system df -v
```

These commands report Docker storage usage without deleting anything.

---

## Permissions

Bind-mounted directories must be accessible by the user or group expected by the container.

When troubleshooting storage problems, inspect ownership and permissions before changing them:

```bash
ls -ld /path/to/directory
```

and:

```bash
stat /path/to/directory
```

Avoid recursively changing ownership or permissions until the container's expected UID/GID and mount configuration have been verified.

Commands such as recursive `chmod` or `chown` can affect large amounts of data and should not be used as a first troubleshooting step.

---

## Backups

Persistent data is more important than the containers themselves.

Containers and images can normally be recreated from Compose definitions, while application data may not be recoverable without a backup.

Important backup targets can include:

- Compose configuration
- application configuration
- databases
- media metadata
- monitoring state
- important file shares
- application source code not already stored in Git

Secrets should be backed up securely outside the public repository.

---

## Repository Boundaries

The Nidhogg-Homelab repository should contain reproducible infrastructure documentation and safe configuration examples.

It should not contain:

- passwords
- API tokens
- private keys
- `.env` secrets
- Cloudflare credentials
- Tailscale authentication keys
- database credentials
- private application state
- sensitive user files
- bulk media or download data

Where configuration requires secrets, the repository should document the required variable or file without including its real value.

---

## Future Direction

A consistent layout remains desirable for future deployments:

```text
/srv/apps/       application source
/srv/compose/    Compose and infrastructure configuration
/srv/data/       persistent application data
```

Existing deployments under `/srv/homelab/compose/` can remain documented as they exist.

Any future migration should be performed deliberately, one service at a time, with:

1. the current mounts inspected
2. persistent data backed up
3. the Compose configuration updated
4. the service recreated
5. functionality verified
6. documentation updated

Storage paths should not be reorganized simply for cosmetic consistency.


---

## Obsidian LiveSync / CouchDB

Persistent CouchDB data for Obsidian LiveSync is stored at:

    /srv/data/obsidian-livesync/couchdb/

This data should remain outside the Git repository.

The Compose configuration lives at:

    /srv/compose/obsidian-livesync/

Repository copies of the Compose file and supporting configuration are stored under:

    compose/obsidian-livesync/

The runtime `.env` file contains credentials and must not be committed.
