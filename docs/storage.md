# Storage

## Overview

This homelab keeps a simple separation between:

- application code
- compose/config files
- persistent service data

This makes the environment easier to manage and back up.

---

## Main Layout

### Application Code
```text
/srv/apps/
```

Used for:
- Symfony application code
- other hosted apps later

Current example:
```text
/srv/apps/Technologies-Web-2.0
```

---

### Compose / Infrastructure Config
```text
/srv/compose/
```

Used for:
- stack definitions
- reverse proxy config
- cloudflared config
- filebrowser config

Current stacks:
```text
/srv/compose/filebrowser
/srv/compose/symfony-app
/srv/compose/nginx_proxy
/srv/compose/cloudflared
```

---

### Persistent Data
```text
/srv/data/
```

Used for:
- MariaDB persistent storage
- future service data

Current example:
```text
/srv/data/symfony-db
```

---

## File Browser

### Root Access
File Browser is mounted against:

```text
/srv
```

This gives access to:
- apps
- compose files
- data directories

### Runtime Data
Its local database/config is stored inside the File Browser stack directory.

---

## Symfony Stack

### Code
```text
/srv/apps/Technologies-Web-2.0
```

### Database
```text
/srv/data/symfony-db
```

### Compose
```text
/srv/compose/symfony-app
```
