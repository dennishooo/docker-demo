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

# publishing the image to docker
docker push <account name>/<image name>

# create a volume
docker volume create <volume name>

# start the container with volume mount
docker run -dp 3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started

# start the container with bind mount
docker run -dp 3000:3000 --mount type=bind,src=/Users/dennisho/Desktop,target=/etc/todos hkhdennis/items

# inspect the details of the volume
docker volume inspect <volume name>

# runnig docker with source code watched
docker run -dp 3000:3000 \
    -w /app --mount type=bind,src="$(pwd)",target=/app \
    node:18-alpine \
    sh -c "yarn install && yarn run dev"
```

each container starts from the image definition each time it starts.

Volumes provide the ability to connect specific filesystem paths of the container back to the host machine.

bind mount: share directory from host filesystem to container

In general, `each container should do one thing and do it well.`

If two containers are on the same network, they can talk to each other.

## multi container

```bash
# create a network for the container to communicate
docker network create <network name>

# start a container in correspnding network
docker run -d \
     --network todo-app --network-alias mysql \
     -v todo-mysql-data:/var/lib/mysql \
     -e MYSQL_ROOT_PASSWORD=secret \
     -e MYSQL_DATABASE=todos \
     mysql:8.0


# nicolaka/netshoot container, which ships with a lot of tools that are useful for troubleshooting or debugging networking issues.
docker run -it --network todo-app nicolaka/netshoot

# to follow the logs of specific services within docker compose
docker compose logs -f <service name>
```

## Docker Compose

The big advantage of using Compose is you can define your application stack in a file, keep it at the root of your project repo (it’s now version controlled), and easily enable someone else to contribute to your project.

By default, Docker Compose automatically creates a network specifically for the application stack (which is why we didn’t define one in the compose file).
