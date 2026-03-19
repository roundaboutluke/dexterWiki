# Docker Installation

This is an alternative to the [source installation](source.md).

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [docker-compose](https://docs.docker.com/compose/install/) (or Docker Compose V2)

## Installation

1. Create a directory for Dexter:

    ```bash
    mkdir dexter-docker && cd dexter-docker
    ```

2. Create a `docker-compose.yml`:

    ```yaml
    version: "3"
    services:
      dexter_db:
        image: mariadb:10
        restart: unless-stopped
        environment:
          MYSQL_ROOT_PASSWORD: rootPassword
          MYSQL_DATABASE: dexter
          MYSQL_USER: dexteruser
          MYSQL_PASSWORD: yourStrongPassword
        volumes:
          - ./dexterdb:/var/lib/mysql

      dexter:
        image: ghcr.io/yourorg/dexter:latest
        restart: unless-stopped
        depends_on:
          - dexter_db
        ports:
          - "3030:3030"
        volumes:
          - ./config:/app/config
        environment:
          DEXTER_DB_HOST: dexter_db
          DEXTER_DB_DATABASE: dexter
          DEXTER_DB_USER: dexteruser
          DEXTER_DB_PASSWORD: yourStrongPassword
    ```

3. Create a config directory and add your `local.json`:

    ```bash
    mkdir config
    # Start once to generate default config, then edit
    docker-compose up -d dexter
    docker-compose logs -f dexter
    # Edit config/local.json with your settings
    docker-compose restart dexter
    ```

!!! warning "Database credentials"
    The database credentials in the `dexter_db` environment section are stored on first startup. Changing them after the first launch will **not** update the database — you would need to delete the `dexterdb` directory and start fresh.

## Running

### Start

```bash
docker-compose up -d && docker-compose logs -f
```

### Stop

```bash
docker-compose down
```

### Update

```bash
docker-compose pull && docker-compose up -d
```

### Check status

```bash
docker ps
docker-compose logs -f dexter
```

Next: [Basic Configuration](configuration.md)
