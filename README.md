# Library

## Overview

The Library is a documentation organizer powered by [Paperless-ng](https://github.com/jonaswinkler/paperless-ng).

## Setup

1. cp `.env.sample` to `.env` and update specified values.
2. cp `docker-compose.env.sample` to `docker-compose.env` and configure as necessary.
3. Verify container is ready by checking the healthcheck: `docker ps`
    - Containers still starting:
    ![alt text](docs/healthcheck_starting.png)
    - Containers ready:
    ![alt text](docs/healthcheck_healthy.png)
4. By default, try to the load the application by navigating to <http://localhost:8000/>.

## Backup And Restore

### Content Backup

```bash
docker-compose exec -T library document_exporter ../export
tar -czvf export.$(date +%Y-%m-%d).tar.gz -C export .
```

### Content Restore

```bash
tar -xzvf export.BACKUP_DATE.tar.gz -C export
docker-compose exec -T library document_importer ../export
```

### Database Backup

```bash
tar -czvf db.$(date +%Y-%m-%d).tar.gz -C db .
```

### Database Restore

```bash
tar -xzvf db.BACKUP_DATE.tar.gz -C db
```

## Additional Commands

### Create Superuser

```bash
docker-compose run --rm library createsuperuser
```

## Docker-Compose Documentation

The following snippet was cut from the original `docker-compose.yml` provided by Paperless-ng:

```plain
docker-compose file for running paperless from the Docker Hub.
This file contains everything paperless needs to run.
Paperless supports amd64, arm and arm64 hardware.

All compose files of paperless configure paperless in the following way:

- Paperless is (re)started on system boot, if it was running before shutdown.
- Docker volumes for storing data are managed by Docker.
- Folders for importing and exporting files are created in the same directory
  as this file and mounted to the correct folders inside the container.
- Paperless listens on port 8000.

In addition to that, this docker-compose file adds the following optional
configurations:

- Instead of SQLite (default), PostgreSQL is used as the database server.

To install and update paperless with this file, do the following:

- Copy this file as 'docker-compose.yml' and the files 'docker-compose.env'
  and '.env' into a folder.
- Run 'docker-compose pull'.
- Run 'docker-compose run --rm library createsuperuser' to create a user.
- Run 'docker-compose up -d'.

For more extensive installation and update instructions, refer to the
documentation.

Backups and Exports
- docker-compose exec -T library document_exporter ../export
```
