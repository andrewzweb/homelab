# GitLab Docker Compose Setup

This repository contains a Docker Compose configuration to set up GitLab, PostgreSQL, Redis, and GitLab Runner. This setup can be used for local development, testing, or as a foundational service for a CI/CD pipeline.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- Docker: [Installation Guide](https://docs.docker.com/get-docker/)
- Docker Compose: [Installation Guide](https://docs.docker.com/compose/install/)

## Getting Started

### 1. Clone this Repository

```bash
git clone git@github.com:andrewzweb/homelab.git
cd gitlab
```

### 2. Create .env file in the root directory with the following content:

```
GITLAB_REGISTRATION_TOKEN=your_registration_token_here
GITLAB_EXTERNAL_URL=http://gitlab
```

### 3. Create the necessary directories for persistent storage:

```sh
mkdir -p ../gitlab-data/config ../gitlab-data/logs ../gitlab-data/data ../gitlab-runner-data/config
```

### 4. Copy `config.toml` to `../gitlab-runner-data/config/config.toml`

```sh
cp config.toml ../gitlab-runner-data/config/config.toml
```

### 5. When run gitlab need take root password what generated

```sh
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```

### 6. After create own user and aprove use root user

### 7. Go to Setting -> CI&CD -> Runners

## Troubleshouting

### Find all runner what exist

```sh
docker exec -it gitlab-runner gitlab-runner list
```

### Check health

```sh
docker-compose exec gitlab-runner gitlab-runner verify
```

### For register and unregister

```sh
docker-compose exec gitlab-runner gitlab-runner unregister --all-runners
docker-compose exec gitlab-runner gitlab-runner register
```

### Get config for runner

```sh
docker exec -it gitlab-runner cat /etc/gitlab-runner/config.toml
```
