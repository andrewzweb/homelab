# Nextcloud Docker Setup

This project provides a Docker Compose configuration for running Nextcloud with MariaDB and Redis, designed to work with Nginx Proxy Manager for reverse proxy and SSL termination.

## Prerequisites

- Docker
- Docker Compose
- Nginx Proxy Manager (set up separately)

## Configuration Files

### docker-compose.yml

```yaml
version: '3'

services:
  nextcloud:
    image: nextcloud:latest
    restart: always
    volumes:
      - ../nextcloud_app_data:/var/www/html
    ports:
      - "6060:80"
    environment:
      - MYSQL_HOST=db
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - REDIS_HOST=redis
      - TRUSTED_PROXIES=172.16.0.0/12
      - NEXTCLOUD_TRUSTED_DOMAINS=${NEXTCLOUD_TRUSTED_DOMAINS}
    depends_on:
      - db
      - redis
    networks:
      - nextcloud_network

  db:
    image: mariadb:10.5
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - ../nextcloud_mariadb_data:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
    networks:
      - nextcloud_network

  redis:
    image: redis:alpine
    restart: always
    networks:
      - nextcloud_network

networks:
  nextcloud_network:
    driver: bridge
```

## .env

Create a .env file in the same directory as your docker-compose.yml with the following content:

```
MYSQL_ROOT_PASSWORD=your_strong_root_password
MYSQL_PASSWORD=your_strong_password
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextcloud
NEXTCLOUD_TRUSTED_DOMAINS=your_domain.com
```

Replace the placeholder values with your own secure passwords and domain.

## Setup Instructions

1. Create a new directory for your project and navigate into it:

```sh
mkdir nextcloud-docker && cd nextcloud-docker
```

2. Create the docker-compose.yml file and paste the configuration provided above.

3. Create the .env file with the content provided above, replacing the placeholder values.

4. Create a directory for Nextcloud data:

```sh
mkdir ../nextcloud_data
```

5. 

```sh
sudo chown -R www-data:www-data ../nextcloud_data
```

## Usage

1. Start the services:

```sh
docker-compose up -d
```

2. Configure Nginx Proxy Manager:

- Add a new proxy host
- Set the domain name for your Nextcloud instance
- Forward hostname/IP: nextcloud
- Forward port: 80
- Configure SSL as needed

3. Access Nextcloud through your configured domain.

## Services

- Nextcloud : The main Nextcloud application
- MariaDB : Database for Nextcloud
- Redis : Caching to improve Nextcloud performance


## Updating

To update to the latest version of Nextcloud:

Pull the latest images:

```sh
docker-compose pull
```

Recreate the containers:

```sh
docker-compose up -d
```

## Backup

Regular backups are recommended:
- Back up the ../nextcloud_data directory
- Back up the ../mariadb_data directory
- Alternatively, you can use MariaDB's backup tools to create database dumps

## Troubleshooting

- Check Docker logs: docker-compose logs
- Ensure Nginx Proxy Manager is configured correctly
- Verify that the .env file contains correct information
- Check permissions on ../nextcloud_data and ../mariadb_data

## Security Notes

- Keep your .env file secure and never commit it to version control
- Regularly update your Docker images and host system
- Use strong, unique passwords for all services
- Ensure that the ../mariadb_data directory has restricted access

## License

MIT License
