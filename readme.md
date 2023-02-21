# Docker Getting Started Tutorial

This tutorial was written with the intent of helping folks get up and running
with containers and is designed to work with Docker Desktop. While not going too much
into depth, it covers the following topics:

- Running your first container
- Building containers
- Learning what containers are
- Running and removing containers
- Using volumes to persist data
- Using bind mounts to support development
- Using container networking to support multi-container applications
- Using Docker Compose to simplify the definition and sharing of applications
- Using image layer caching to speed up builds and reduce push/pull size
- Using multi-stage builds to separate build-time and runtime dependencies

## Getting Started

If you wish to run the tutorial, you can use the following command after installing Docker Desktop:

```bash
# building docker image
docker build -t <your-image-name> .

# start running your application in a container
docker run -d -p 80:80 docker/<your-image-name>

# -d flag to run the new container in “detached” mode (in the background).
# You also use the -p flag to create a mapping between the host’s port 3000 to the container’s port 3000
```

Once it has started, you can open your browser to [http://localhost](http://localhost).

## Development

This project has a `docker-compose.yml` file, which will start the mkdocs application on your
local machine and help you see changes instantly.

```bash
docker compose up
```

## Notes

cannot start a new container while the old container is still running
(old one is already useing thr port)

```bash
# list all containers
docker ps

# stop the old container
docker stop <the-container-id>
```
