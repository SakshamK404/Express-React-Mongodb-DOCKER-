
---

In the context of composing a sample application, the utilization of Docker Development Environments is recommended.

[Access Docker Dev Environments here <img src="../open_in_new.svg" alt="Open in Docker Dev Environments" align="top"/>](https://open.docker.com/dashboard/dev-envs?url=https://github.com/docker/awesome-compose/tree/master/react-express-mongodb)

### Description of the Application:

A combination of a React application, a NodeJS backend, and a MongoDB database constitutes the core structure of this project:

```
.
├── backend
│   ├── Dockerfile
│   ...
├── compose.yaml
├── frontend
│   ├── ...
│   └── Dockerfile
└── README.md
```

[_compose.yaml_](compose.yaml)
```
services:
  frontend:
    build:
      context: frontend
    ...
    ports:
      - 3000:3000
    ...
  server:
    container_name: server
    restart: always
    build:
      context: server
      args:
        NODE_PORT: 3000
    ports:
      - 3000:3000
    ...
    depends_on:
      - mongo
  mongo:
    container_name: mongo
    restart: always
    ...
```
The compose file outlines an application featuring three services: `frontend`, `backend`, and `db`.
During deployment, Docker Compose facilitates the mapping of port 3000 from the frontend service container to port 3000 on the host system, as stipulated within the file. It's crucial to ensure that port 3000 on the host isn't already in use.

### Deployment via Docker Compose:

```
$ docker compose up -d
Creating network "react-express-mongodb_default" with the default driver
Building frontend
Step 1/9 : FROM node:13.13.0-stretch-slim
 ---> aa6432763c11
...
Successfully tagged react-express-mongodb_app:latest
WARNING: Image for service app was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating frontend        ... done
Creating mongo           ... done
Creating app             ... done
```

### Expected Outcome:

Upon starting the application, the listing of containers should display running containers along with the port mapping, as illustrated below:

```
$ docker ps
CONTAINER ID        IMAGE                               COMMAND                  CREATED             STATUS                  PORTS                      NAMES
06e606d69a0e        react-express-mongodb_server        "docker-entrypoint.s…"   23 minutes ago      Up 23 minutes           0.0.0.0:3000->3000/tcp     server
ff56585e1db4        react-express-mongodb_frontend      "docker-entrypoint.s…"   23 minutes ago      Up 23 minutes           0.0.0.0:3000->3000/tcp     frontend
a1f321f06490        mongo:4.2.0                         "docker-entrypoint.s…"   23 minutes ago      Up 23 minutes           0.0.0.0:27017->27017/tcp   mongo
```

Following the application's initiation, access `http://localhost:3000` via your web browser.

![page](./output.png)

To cease and remove the containers:

```
$ docker compose down
Stopping server   ... done
Stopping frontend ... done
Stopping mongo    ... done
Removing server   ... done
Removing frontend ... done
Removing mongo    ... done
```

### Explanation of `docker-compose`:

__Version__:

The initial line specifies the version of the file. This indicates the version of the compose.yaml file being utilized.

__Services__:

This section delineates the creation of containers, comprising three services: `frontend`, `backend` (referred to as `server`), and `db`.

##### Service app (backend - NodeJS):

An image for the app is constructed from the `Dockerfile`, further elaborated below.

__Explanation of service server__:

- The definition of a service utilizing Node.js, labeled as `server`.
- Naming the container service for the Node server as `server`, enhancing readability amidst multiple containers on a machine.
- Automatic initiation of the Docker container in the event of failure.
- Construction of the `server` image using the Dockerfile from the current directory, with an argument passed to the backend `DockerFile`.
- Port mapping from the host to the container.

##### Service mongo:

An additional service, `mongo`, is added. Unlike `backend`, all instructions are directly included here. The standard mongo image is pulled from DockerHub registry.

__Explanation of service mongo__:

- Specification of a `mongodb` service, denoted as `mongo`.
- Retrieval of the mongo 4.2.0 image from DockerHub.
- Mounting the current db directory to the container.
- For persistent storage, the host directory `/data` is mounted to the container directory `/data/db`, ensuring data continuity.
- Interlinking the `app` container with the `mongo` container ensures accessibility between the two services.
- Port mapping from the host to the container.

Ensure the local MongoDB version matches the specified version in the image (`mongo:4.2.0`) to properly reflect changes locally.
