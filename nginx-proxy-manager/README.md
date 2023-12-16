# NginxProxyManager

## Step 1. Crete env

Before running create .env for example

```
# Nginx Proxy Manager
DB_MYSQL_HOST=db
DB_MYSQL_PORT=3306
DB_MYSQL_USER=npm-homelab
DB_MYSQL_PASSWORD=npm_password
DB_MYSQL_NAME=npm

# MariaDB
MYSQL_ROOT_PASSWORD=root_password
MYSQL_DATABASE=npm
MYSQL_USER=npm
MYSQL_PASSWORD=npm_password
```

## Step 2. Creeate network

And create docker network

```
docker network create nginx
```

## Step 3. Set your data and change default access data

When go to your IP and port http://your_domain:81

> Default Administrator User
> Email:    admin@example.com
> Password: changeme
