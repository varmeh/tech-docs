# Docker Lifecycle Commands

- [Docker Lifecycle Commands](#docker-lifecycle-commands)
  - [docker create](#docker-create)
  - [docker start](#docker-start)
  - [docker run](#docker-run)
  - [docker stop](#docker-stop)
  - [docker restart](#docker-restart)
    - [docker start vs docker restart](#docker-start-vs-docker-restart)
  - [docker rm](#docker-rm)

The lifecycle of a Docker container generally involves the following stages:

1. **Create:** Using the `docker create` command, a new Docker container is created from a specified image. This stage includes setting any configuration options like port mappings, volume mounts, environment variables, etc.

2. **Start:** Once the container is created, you can start it using the `docker start` command. This initiates the process defined in the Dockerfile's CMD instruction.

3. **Running:** When the container is running, it's in the most active state. During this phase, it can be paused, resumed, or stopped. You can also interact with the running container, execute commands in it, monitor its resource usage, etc.

4. **Stop:** When a container is stopped using the `docker stop` command, the main process inside the container is terminated and the container is moved to a stopped state.

5. **Restart:** A stopped container can be restarted using the `docker restart` command. This will put the container back into the running state and restart the main process.

6. **Remove:** When a container is no longer needed, it can be removed with the `docker rm` command. This will destroy the container and free up system resources. Please note, you cannot remove a running container; it must be stopped first.

It's worth noting that `docker run` is a combination of `docker create` and `docker start`. It creates the container and immediately starts it.

And lastly, Docker provides commands to pause (`docker pause`) and unpause (`docker unpause`) a container, adding more steps to the lifecycle, allowing you to temporarily halt processes in a running container.

## docker create

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker create` | `docker create ubuntu` | Creates a new container based on the `ubuntu` image, but doesn't start it. |
| Create with a Specific Command | `docker create ubuntu /bin/echo 'Hello, World!'` | Creates a container that, when started, will echo "Hello, World!". |
| Specifying a Name | `docker create --name my_container ubuntu` | Creates a new container with the name `my_container`. |
| Port Mapping | `docker create -p 8080:80 nginx` | Sets up a mapping from port 8080 on the host to port 80 on the container. |
| Mounting Volumes | `docker create -v /host/directory:/container/directory ubuntu` | Mounts `/host/directory` from the host to `/container/directory` in the container. |
| Specifying Environment Variables | `docker create -e "ENV_VAR=value" ubuntu` | Sets an environment variable `ENV_VAR` with the value `value`. |

The `docker create` command allows you to set up a container with the desired configuration before starting it. Once created, you can start the container using the `docker start` command followed by the container ID or name.

## docker start

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker start` | `docker start container_id` | Starts a specific container by replacing `container_id` with the ID of the container you want to start. |
| Start and Attach | `docker start -a container_id` | Starts a specific container and attaches STDOUT/STDERR and forward signals. |
| Start and Interactive | `docker start -i container_id` | Starts a specific container and attaches container's STDIN. Useful if you want to provide input to the container. |
| Start Multiple Containers | `docker start container_id1 container_id2` | Starts multiple containers by specifying each container's ID separated by a space. |

As usual, replace `container_id` with the ID of the container you want to start.

You can get the ID of the container by using the `docker ps -a` command, which lists all containers (not just running ones)

## docker run

`docker run = docker create + docker start`

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker run` | `docker run ubuntu` | Creates and starts a new container based on the `ubuntu` image. |
| Name a container | `docker run --name redis-container redis` | Starts a redis container named `redis-container` |
| Run in Interactive Mode | `docker run -it ubuntu` | Starts an interactive shell in the `ubuntu` container. |
| Run in Detached Mode | `docker run -d nginx` | Runs the `nginx` server in the background (detached mode). |
| Port Mapping | `docker run -d -p 8080:80 nginx` | Maps port 8080 on the host to port 80 on the `nginx` container. |
| Mounting Volumes | `docker run -v /host/directory:/container/directory ubuntu` | Mounts `/host/directory` from the host to `/container/directory` in the container. |
| Specifying Environment Variables | `docker run -e "ENV_VAR=value" ubuntu` | Sets an environment variable `ENV_VAR` with the value `value`. |
| Running a Specific Command | `docker run ubuntu echo "Hello, World!"` | Starts a new container from the `ubuntu` image and executes the `echo "Hello, World!"` command. |
| Removing the Container after Exit | `docker run --rm ubuntu echo "Hello, World!"` | Removes the container once the `echo` command completes and the container exits. |

**Recommended**:

```bash
docker run -d --rm --name redis-container -p 6379:6379 redis
```

- `-d` - ensures running in background as a daemon
- `--rm` - ensures container is removed after exit. So, we don't have to manually remove it using `docker rm` command

## docker stop

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker stop` | `docker stop container_id` | Stops a specific container. Replace `container_id` with the ID of the container you want to stop. |
| Stop Multiple Containers | `docker stop container_id1 container_id2` | Stops multiple containers. Just replace `container_id1` and `container_id2` with the IDs of the containers you want to stop. |
| Stop with Timeout | `docker stop -t seconds container_id` | Stops a specific container but allows it 'seconds' to stop before killing it. Replace 'seconds' with the number of seconds you want to wait before forcefully stopping the container. |

## docker restart

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker restart` | `docker restart container_id` | Restarts a specific container. Replace `container_id` with the ID of the container you want to restart. |
| Restart Multiple Containers | `docker restart container_id1 container_id2` | Restarts multiple containers. Just replace `container_id1` and `container_id2` with the IDs of the containers you want to restart. |
| Restart with Timeout | `docker restart -t seconds container_id` | Restarts a specific container but allows it 'seconds' to stop before killing it. Replace 'seconds' with the number of seconds you want to wait before forcefully restarting the container. |

### docker start vs docker restart

|     | `docker start` | `docker restart` |
| --- | -------------- | ---------------- |
| **Action on a stopped container** | Starts the stopped container. | Starts the stopped container. |
| **Action on a running container** | No effect, as the container is already running. | Stops the running container, then restarts it. |
| **Common Use** | Used when a stopped container needs to be started. | Used when a running container needs to be reset or a stopped container needs to be started. |

- Remember, the behavior of these commands is defined by the state of the container at the time the command is run.

- Always consider the current state of your container before choosing the appropriate command.

## docker rm

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker rm` | `docker rm container_id` | Removes a specific container. Replace `container_id` with the ID of the container you want to remove. |
| Remove Multiple Containers | `docker rm container_id1 container_id2` | Removes multiple containers. Just replace `container_id1` and `container_id2` with the IDs of the containers you want to remove. |
| Force Remove | `docker rm -f container_id` | Forces the removal of a running container (uses SIGKILL). Replace `container_id` with the ID of the container you want to remove. |
| Remove with Volume | `docker rm -v container_id` | Removes the specified container and its associated volumes. |

- Always remember that a container must be stopped before it can be removed.

- The `docker rm -f` command can stop and remove a container in one step, but use it cautiously as it forcefully kills the container.
