# Docker Compose setup for ERPNext

ERPNext is a free and open-source integrated Enterprise Resource Planning (ERP) software developed by Frappé Technologies Pvt. Ltd. and is built on the Frappe framework.

This repository provides a docker-compose.yml file to run ERPNext with MariaDB and Redis using Docker Compose.

## Features

- ERPNext (latest)
- MariaDB 10.6
- Redis 7

## Requirements

- Docker & Docker Compose
- 8-16 GB RAM minimum (ERP requires resources)
- 4+ CPU cores
- 50+ GB disk space (database, data, documents)
- Ports: 80/443 (web), 3306 (database)

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/JLalib/docker-erpnext.git
   cd docker-erpnext
   ```

2. Start the services:
   ```bash
   docker compose up -d
   ```

3. Initial setup (first time only):
   ```bash
   docker compose exec erpnext bench new-site erpnext.localhost
   docker compose exec erpnext bench install-app erpnext
   ```

4. Access ERPNext:
   Open your browser and go to: http://localhost
   - Username: admin
   - Password: (the password generated during the `bench new-site` step)

## Configuration

The docker-compose.yml file uses the following environment variables for the erpnext service:
- `DB_HOST`: MariaDB service name (mariadb)
- `DB_PORT`: MariaDB port (3306)
- `DB_NAME`: Database name (erpnext)
- `REDIS_CACHE`: Redis connection string (redis:6379)

You can modify the MariaDB root password and database name in the docker-compose.yml file if needed.

## Maintenance

### View logs
```bash
docker logs -f erpnext
```

### Backup
```bash
docker exec erpnext bench backup
```

### Update ERPNext
```bash
docker compose pull
docker compose exec erpnext bench migrate
docker compose up -d
```

### Create a new site
```bash
docker compose exec erpnext bench new-site otraempresa.localhost
```

### Access database
```bash
docker exec -it erpnext_db mysql -u root -p erpnext
```

### Restart services
```bash
docker compose restart
```

## HTTPS with Caddy (example)

For production, you might want to use a reverse proxy like Caddy:

```
erpnext.tudominio.com {
    reverse_proxy localhost:8000
}
```

## Multi-site with subdomains

```
empresa1.tudominio.com {
    reverse_proxy localhost:8000
}

empresa2.tudominio.com {
    reverse_proxy localhost:8000
}
```

## Notes

- ERPNext is more resource-intensive than typical applications. For production with many users, allocate at least 8GB RAM.
- The default MariaDB container does not set a password for the root user by default. We set one via the `MYSQL_ROOT_PASSWORD` environment variable. Change it to a strong password in production.
- The data for MariaDB is persisted in a named volume `mariadb_data`.

## License

This repository contains only the docker-compose.yml and README.md. The ERPNext software is licensed under GPL-3.0.