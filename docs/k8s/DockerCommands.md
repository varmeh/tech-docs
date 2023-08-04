# Docker Commands

- [Docker Commands](#docker-commands)
  - [Docker Container Lifecycle](#docker-container-lifecycle)
  - [Docker Run Primer](#docker-run-primer)
    - [1. Basic `docker run`](#1-basic-docker-run)
    - [2. Run in Detached Mode](#2-run-in-detached-mode)
    - [3. Port Mapping](#3-port-mapping)
    - [4. Mounting Volumes](#4-mounting-volumes)
    - [5. Specifying Environment Variables](#5-specifying-environment-variables)
    - [6. Running a Specific Command](#6-running-a-specific-command)
    - [7. Removing the Container after Exit](#7-removing-the-container-after-exit)
    - [8. Running a Interactive Shell](#8-running-a-interactive-shell)

## Docker Container Lifecycle

The lifecycle of a Docker container generally involves the following stages:

1. **Create:** Using the `docker create` command, a new Docker container is created from a specified image. This stage includes setting any configuration options like port mappings, volume mounts, environment variables, etc.

2. **Start:** Once the container is created, you can start it using the `docker start` command. This initiates the process defined in the Dockerfile's CMD instruction.

3. **Running:** When the container is running, it's in the most active state. During this phase, it can be paused, resumed, or stopped. You can also interact with the running container, execute commands in it, monitor its resource usage, etc.

4. **Stop:** When a container is stopped using the `docker stop` command, the main process inside the container is terminated and the container is moved to a stopped state.

5. **Restart:** A stopped container can be restarted using the `docker restart` command. This will put the container back into the running state and restart the main process.

6. **Remove:** When a container is no longer needed, it can be removed with the `docker rm` command. This will destroy the container and free up system resources. Please note, you cannot remove a running container; it must be stopped first.

It's worth noting that `docker run` is a combination of `docker create` and `docker start`. It creates the container and immediately starts it.

And lastly, Docker provides commands to pause (`docker pause`) and unpause (`docker unpause`) a container, adding more steps to the lifecycle, allowing you to temporarily halt processes in a running container.

## Docker Run Primer

### 1. Basic `docker run`

The simplest form of the `docker run` command:

```bash
docker run ubuntu
```

This command will create and start a new container based on the `ubuntu` image.

It will start an interactive shell in the container, because the default command in the `ubuntu` image is `/bin/bash`.

The shell is run within & would not be accessible to us on docker shell.

To access it, run it interactive mode.

```bash
docker run -it ubuntu
```

### 2. Run in Detached Mode

The `-d` option runs the container in the background (detached mode):

```bash
docker run -d nginx
```

This command runs the `nginx` server in the background.

### 3. Port Mapping

The `-p` option maps a host port to a container port:

```bash
docker run -d -p 8080:80 nginx
```

This command maps port 8080 on the host to port 80 on the `nginx` container.

### 4. Mounting Volumes

The `-v` option mounts a directory from the host into the container:

```bash
docker run -v /host/directory:/container/directory ubuntu
```

This mounts `/host/directory` from the host to `/container/directory` in the container.

### 5. Specifying Environment Variables

The `-e` option sets environment variables:

```bash
docker run -e "ENV_VAR=value" ubuntu
```

This sets an environment variable `ENV_VAR` with the value `value`.

### 6. Running a Specific Command

You can also specify a command to run in the container:

```bash
docker run ubuntu echo "Hello, World!"
```

This will start a new container from the `ubuntu` image and execute the `echo "Hello, World!"` command.

### 7. Removing the Container after Exit

The `--rm` option automatically removes the container when it exits:

```bash
docker run --rm ubuntu echo "Hello, World!"
```

This will remove the container once the `echo` command completes and the container exits.

### 8. Running a Interactive Shell

```bash
docker run -it ubuntu /bin/bash
```

Start a `ubuntu` container & launch a bash shell on it
