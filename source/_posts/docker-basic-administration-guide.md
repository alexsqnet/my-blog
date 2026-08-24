---
title: Docker - Basic Administration Guide
date: 2026-06-20 23:36:52
tags: [docker, linux, cli]
category: ["docker"]

---

This guide contains common Docker commands and practical examples useful for managing Docker on a Linux server or VPS.

<!--more-->

## 1. Check whether Docker is installed

```bash
docker --version
```

Check Docker Compose:

```bash
docker compose version
```

Check Docker service status:

```bash
sudo systemctl status docker
```

---

## 2. Install Docker on Ubuntu

Update packages:

```bash
sudo apt update
sudo apt upgrade -y
```

Install required packages:

```bash
sudo apt install -y ca-certificates curl
```

Create the Docker keyring directory:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

Download Docker's official GPG key:

```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

Set permissions:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Add the Docker repository:

```bash
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu   $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Update package information:

```bash
sudo apt update
```

Install Docker Engine and Docker Compose:

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Test Docker:

```bash
sudo docker run hello-world
```

---

## 3. Start, stop, and enable Docker

Start Docker:

```bash
sudo systemctl start docker
```

Stop Docker:

```bash
sudo systemctl stop docker
```

Restart Docker:

```bash
sudo systemctl restart docker
```

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

Check status:

```bash
sudo systemctl status docker
```

---

## 4. Run Docker without sudo

Add your current user to the `docker` group:

```bash
sudo usermod -aG docker $USER
```

Then log out and log back in.

Check membership:

```bash
groups
```

Test:

```bash
docker ps
```

> Membership in the `docker` group effectively grants root-level privileges on the host. Only trusted users should be added to this group.

---

## 5. List containers

Show running containers:

```bash
docker ps
```

Show all containers:

```bash
docker ps -a
```

Alternative syntax:

```bash
docker container ls
```

```bash
docker container ls -a
```

---

## 6. Run a container

Run Nginx:

```bash
docker run nginx
```

Run in detached mode:

```bash
docker run -d nginx
```

Run with a custom name:

```bash
docker run -d --name web nginx
```

Expose a port:

```bash
docker run -d --name web -p 8080:80 nginx
```

This maps:

```text
host port 8080 -> container port 80
```

Open:

```text
http://SERVER_IP:8080
```

---

## 7. Start, stop, restart, and remove containers

Stop a container:

```bash
docker stop web
```

Start it:

```bash
docker start web
```

Restart it:

```bash
docker restart web
```

Remove it:

```bash
docker rm web
```

Force-remove a running container:

```bash
docker rm -f web
```

---

## 8. View container logs

Show logs:

```bash
docker logs web
```

Follow logs:

```bash
docker logs -f web
```

Show the last 100 lines:

```bash
docker logs --tail 100 web
```

Show timestamps:

```bash
docker logs -t web
```

---

## 9. Execute commands inside a container

Open a shell:

```bash
docker exec -it web bash
```

If Bash is not available:

```bash
docker exec -it web sh
```

Run a single command:

```bash
docker exec web ls -la
```

---

## 10. Inspect a container

```bash
docker inspect web
```

Get the container IP:

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web
```

Show container resource usage:

```bash
docker stats
```

---

## 11. List Docker images

```bash
docker images
```

Alternative:

```bash
docker image ls
```

---

## 12. Pull an image

```bash
docker pull nginx
```

Specific version:

```bash
docker pull nginx:1.27
```

---

## 13. Remove Docker images

Remove an image:

```bash
docker rmi nginx
```

Force removal:

```bash
docker rmi -f nginx
```

Remove dangling images:

```bash
docker image prune
```

---

## 14. Build an image

From a directory containing a `Dockerfile`:

```bash
docker build -t myapp .
```

With a version tag:

```bash
docker build -t myapp:1.0.0 .
```

List images afterward:

```bash
docker images
```

---

## 15. Tag an image

```bash
docker tag myapp:1.0.0 username/myapp:1.0.0
```

Example for a registry:

```bash
docker tag myapp:1.0.0 registry.example.com/myapp:1.0.0
```

---

## 16. Docker login and logout

Login to a container registry:

```bash
docker login
```

Login to a specific registry:

```bash
docker login registry.example.com
```

Logout:

```bash
docker logout
```

---

## 17. Push an image

```bash
docker push username/myapp:1.0.0
```

Or:

```bash
docker push registry.example.com/myapp:1.0.0
```

---

## 18. Environment variables

Pass an environment variable:

```bash
docker run -e APP_ENV=production myapp
```

Multiple variables:

```bash
docker run   -e APP_ENV=production   -e PORT=8080   myapp
```

Load variables from a file:

```bash
docker run --env-file .env myapp
```

> Do not commit production secrets, passwords, API keys, or tokens to Git.

---

## 19. Volumes

List volumes:

```bash
docker volume ls
```

Create a volume:

```bash
docker volume create app-data
```

Inspect it:

```bash
docker volume inspect app-data
```

Use it:

```bash
docker run -d   --name app   -v app-data:/data   myapp
```

Remove it:

```bash
docker volume rm app-data
```

Remove unused volumes:

```bash
docker volume prune
```

---

## 20. Bind mounts

Mount a host directory inside a container:

```bash
docker run -d   --name web   -v /opt/app/data:/app/data   myapp
```

Modern syntax:

```bash
docker run -d   --mount type=bind,source=/opt/app/data,target=/app/data   myapp
```

---

## 21. Docker networks

List networks:

```bash
docker network ls
```

Create a network:

```bash
docker network create app-network
```

Run a container on that network:

```bash
docker run -d   --name api   --network app-network   myapi
```

Connect an existing container:

```bash
docker network connect app-network web
```

Disconnect:

```bash
docker network disconnect app-network web
```

Remove a network:

```bash
docker network rm app-network
```

---

## 22. Container DNS between services

Containers on the same user-defined Docker network can communicate using container names.

Example:

```text
api -> postgres:5432
```

Instead of:

```text
api -> localhost:5432
```

Inside a container, `localhost` refers to that container itself.

---

## 23. Restart policies

Automatically restart a container:

```bash
docker run -d   --restart unless-stopped   --name web   nginx
```

Common policies:

```text
no
always
unless-stopped
on-failure
```

For long-running VPS services, this is commonly useful:

```text
unless-stopped
```

---

## 24. Docker Compose basics

Example `compose.yaml`:

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    restart: unless-stopped
```

Start services:

```bash
docker compose up
```

Start in background:

```bash
docker compose up -d
```

Stop and remove containers:

```bash
docker compose down
```

Restart:

```bash
docker compose restart
```

---

## 25. Docker Compose build

Build services:

```bash
docker compose build
```

Build without cache:

```bash
docker compose build --no-cache
```

Build and start:

```bash
docker compose up -d --build
```

---

## 26. Docker Compose status

List services:

```bash
docker compose ps
```

Show logs:

```bash
docker compose logs
```

Follow logs:

```bash
docker compose logs -f
```

Follow logs for one service:

```bash
docker compose logs -f api
```

---

## 27. Docker Compose pull and update

Pull newer images:

```bash
docker compose pull
```

Recreate services:

```bash
docker compose up -d
```

Typical update workflow:

```bash
docker compose pull
docker compose up -d
```

---

## 28. Docker Compose environment file

Example `.env`:

```text
APP_ENV=production
POSTGRES_DB=appdb
POSTGRES_USER=appuser
```

Example Compose usage:

```yaml
services:
  api:
    image: myapi
    environment:
      APP_ENV: ${APP_ENV}

  postgres:
    image: postgres:17
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
```

Keep secret `.env` files out of Git.

Example `.gitignore`:

```text
.env
.env.production
```

---

## 29. View Docker disk usage

```bash
docker system df
```

Detailed view:

```bash
docker system df -v
```

---

## 30. Clean unused Docker resources

Remove stopped containers:

```bash
docker container prune
```

Remove unused images:

```bash
docker image prune
```

Remove unused networks:

```bash
docker network prune
```

Remove unused volumes:

```bash
docker volume prune
```

Remove most unused Docker resources:

```bash
docker system prune
```

Also remove unused images not referenced by containers:

```bash
docker system prune -a
```

> Be careful with cleanup commands on production servers.

---

## 31. Check container resource usage

Live CPU and memory usage:

```bash
docker stats
```

For a single container:

```bash
docker stats web
```

---

## 32. Copy files between host and container

Copy from host to container:

```bash
docker cp file.txt web:/tmp/file.txt
```

Copy from container to host:

```bash
docker cp web:/tmp/file.txt ./file.txt
```

---

## 33. Check a container's ports

```bash
docker port web
```

Or:

```bash
docker ps
```

---

## 34. Check container processes

```bash
docker top web
```

---

## 35. Rename a container

```bash
docker rename old-name new-name
```

---

## 36. Health checks

Check container health:

```bash
docker inspect --format='{{json .State.Health}}' container-name
```

Example Compose health check:

```yaml
services:
  web:
    image: nginx
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 5s
      retries: 3
```

---

## 37. View Docker events

```bash
docker events
```

Useful for debugging container starts, stops, restarts, and network events.

---

## 38. Docker daemon logs

Check Docker service logs:

```bash
sudo journalctl -u docker
```

Follow them:

```bash
sudo journalctl -u docker -f
```

---

## 39. Common troubleshooting commands

Check running containers:

```bash
docker ps
```

Check stopped containers:

```bash
docker ps -a
```

Check logs:

```bash
docker logs container-name
```

Inspect container:

```bash
docker inspect container-name
```

Check networks:

```bash
docker network ls
```

Check ports:

```bash
sudo ss -tulpn
```

Check Docker disk usage:

```bash
docker system df
```

Check daemon status:

```bash
sudo systemctl status docker
```

---

## 40. Example production directory structure

A simple application deployment directory can look like:

```text
/opt/app
├── compose.yaml
├── .env
├── config/
├── data/
└── backups/
```

Recommended ownership:

```bash
sudo chown -R deployuser:deployuser /opt/app
```

---

## 41. Example multi-service Compose file

```yaml
services:
  api:
    image: registry.example.com/api:latest
    restart: unless-stopped
    env_file:
      - .env
    networks:
      - internal

  web:
    image: registry.example.com/web:latest
    restart: unless-stopped
    networks:
      - internal

  postgres:
    image: postgres:17
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - internal

networks:
  internal:

volumes:
  postgres-data:
```

Start it:

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
```

Follow logs:

```bash
docker compose logs -f
```

---

## 42. Update a Dockerized application

A common deployment flow:

```bash
cd /opt/app
docker compose pull
docker compose up -d
docker image prune
```

If images are built locally:

```bash
cd /opt/app
docker compose up -d --build
```

---

## 43. Backup a Docker volume

One simple approach:

```bash
docker run --rm   -v app-data:/data   -v $(pwd):/backup   alpine   tar czf /backup/app-data-backup.tar.gz -C /data .
```

Restore:

```bash
docker run --rm   -v app-data:/data   -v $(pwd):/backup   alpine   tar xzf /backup/app-data-backup.tar.gz -C /data
```

For databases such as PostgreSQL, prefer database-aware backup tools such as `pg_dump`.

---

## 44. Useful commands cheat sheet

```bash
docker ps
docker ps -a
docker images
docker logs -f container-name
docker exec -it container-name bash
docker inspect container-name
docker stats
docker network ls
docker volume ls
docker system df
docker compose ps
docker compose logs -f
docker compose pull
docker compose up -d
docker compose up -d --build
docker compose down
```

---

## Security recommendations

For Docker on a public VPS:

- do not expose database ports publicly unless necessary;
- expose only required ports;
- use a firewall such as UFW;
- avoid running containers as `root` when possible;
- do not mount the Docker socket into containers unless absolutely necessary;
- keep Docker and the host OS updated;
- use specific image versions instead of relying on `latest` for production;
- keep secrets outside Git;
- use read-only mounts where possible;
- configure restart policies;
- back up persistent volumes and databases;
- periodically review unused images, containers, networks, and volumes;
- only add trusted users to the `docker` group.
