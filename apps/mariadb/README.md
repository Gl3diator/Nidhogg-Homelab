# 🗄️ MariaDB

## Overview
MariaDB is the database backend used by the Symfony application.

It stores the application data and runs as a dedicated container in the Symfony stack.

---

## Purpose
- Persist application data
- Support CRUD operations for the Symfony app
- Provide a clean database service inside Docker

---

## Access
- Internal Docker network only
- Not exposed publicly
- Not intended for direct external access

---

## Why MariaDB
MariaDB was chosen because it is:
- lightweight
- widely supported
- compatible with Symfony / Doctrine
- easy to run in containers

---

## Runtime Layout
- Stack location: `/srv/compose/symfony-app`
- Data path: `/srv/data/symfony-db`

---

## Notes
- Used only by the Symfony application stack
- Should be backed up if data matters
- Not exposed through Tailscale or Cloudflare
