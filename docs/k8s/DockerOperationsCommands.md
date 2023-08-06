# Docker Operations Commands

- [Docker Operations Commands](#docker-operations-commands)
  - [docker ps](#docker-ps)
  - [docker logs](#docker-logs)
  - [docker exec](#docker-exec)
  - [docker attach](#docker-attach)
  - [docker kill](#docker-kill)
    - [docker stop vs docker kill](#docker-stop-vs-docker-kill)
  - [docker top](#docker-top)
  - [docker stats](#docker-stats)

Docker operation commands are used to manage running containers and their processes

## docker ps

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker ps` | `docker ps` | Shows all running containers. |
| Show All Containers | `docker ps -a` | Shows all containers, regardless of their status (running, exited, etc.). |
| Show Latest Created Container | `docker ps -l` | Shows the most recently created container, including all its details. |
| Show n Last Created Containers | `docker ps -n` | Shows the 'n' last created containers. Replace 'n' with the number of containers you want to list. |
| Show Containers & Size | `docker ps -s` | Shows all containers and their disk usage. |

Remember, `docker ps` will show you the container ID, which is often required for other Docker commands such as `docker start`, `docker stop`, and `docker rm`

## docker logs

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker logs` | `docker logs container_id` | Shows logs of a specific container. Replace `container_id` with the ID of the container you want to check. |
| Show last N lines | `docker logs --tail N container_id` | Shows the last N lines of output for a specific container. Replace `N` with the number of lines you want to see. |
| Show since a certain time | `docker logs --since '2023-08-03T10:10:09' container_id` | Shows the logs since a specific date or duration. In this example, the logs since August 3, 2023, 10:10:09. |
| Follow logs in real-time | `docker logs -f container_id` | Follows the logs output for a container in real-time, similar to `tail -f`. |
| Show timestamped logs | `docker logs --timestamps container_id` | Shows logs with timestamps for each log entry. |

Remember that the `docker logs` command works on both **running** and **stopped** containers.

## [docker exec](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11436666#overview)

This command opens an interactive Bash shell inside a running container named `my_container`. From there, you can run any command inside the container as if you were SSH-ed into a remote server.
| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker exec` | `docker exec container_id command` | Executes a command in a running container. Replace `command` with the command you want to run. |
| Interactive exec | `docker exec -it container_id /bin/bash` | Starts an interactive bash shell in the container. |
| As a different user | `docker exec -u user container_id command` | Runs a command as a specific user. Replace `user` with the user's name. |
| Detached exec | `docker exec -d container_id command` | Runs a command in a detached mode, in the background. |
| Environment variables | `docker exec -e VAR=value container_id command` | Sets environment variables for the command being run. Replace `VAR` with the variable name and `value` with the corresponding value. |

The `docker exec` command is used to run *a new command in a running Docker container*. This can be incredibly useful for various purposes, including:

1. **Debugging**: If you have a running container and you're unsure why it's not behaving as expected, `docker exec` can be used to start a shell in the context of the container. You can then use standard Linux commands to inspect the container's file system, processes, network settings, etc.

2. **Administrative tasks**: Maybe you need to change a configuration file, install a new package, or run a database migration. Using `docker exec`, you can run these tasks directly within the running container.

3. **Monitoring**: With `docker exec`, you can fetch specific information about the state of a running container. This might be useful in monitoring and logging scenarios.

## docker attach

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker attach` | `docker attach container_id` | Attaches your terminal's standard input, output, and error (or any combination of the three) to a running container. Replace `container_id` with the ID or name of the container you want to attach to. |
| Detach without stopping the container | `docker attach container_id` </br> `CTRL-p CTRL-q` | After attaching to a container, you can detach from it without stopping the container by using the `CTRL-p CTRL-q` key sequence. |
| Attach & detach with options | `docker attach --detach-keys="ctrl-c" container_id` | This command allows you to customize the detach key sequence. In this example, `CTRL-c` is used. |

Remember, if you attach to a container and then use `CTRL-c`, it will send a SIGINT to the container, which could stop it.

To avoid this, you can customize the detach keys or use the default `CTRL-p CTRL-q` key sequence to safely detach. For more options and details, check out the official [Docker documentation](https://docs.docker.com/engine/reference/commandline/attach/).

## docker kill

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker kill` | `docker kill container_id` | Sends a SIGKILL signal to a running container. This will immediately stop the container. Replace `container_id` with the ID or name of the container you want to kill. |
| Send specific signal | `docker kill --signal=SIGNAL container_id` | Sends a specific signal to the container. Replace `SIGNAL` with the signal you want to send (like SIGINT, SIGTERM, etc.). |

Remember, `docker kill` will forcefully stop the container, which could lead to data loss if the container is in the middle of a write operation.

It's generally recommended to use `docker stop` first, which sends a SIGTERM signal that allows the container to shut down gracefully.

If that doesn't work, then you can use `docker kill`.

### [docker stop vs docker kill](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11436664#overview)

|     | `docker stop` | `docker kill` |
| --- | ------------- | ------------- |
| Description | Sends a SIGTERM signal to the main process in a container, asking it to shut down gracefully. If the process doesn't exit within a certain timeframe (default is 10 seconds), a SIGKILL signal is sent to forcefully stop the container. | Immediately sends a SIGKILL signal to a running container, forcibly terminating it. |
| When to use | When you want to give a container the chance to clean up resources, save state, or perform other shutdown tasks. This is generally the recommended way to stop a container. | When you need to immediately stop a container, typically during an error state or when the container is unresponsive. Be aware that it can lead to data loss, as it doesn't give the container a chance to perform a graceful shutdown. |
| Command | `docker stop container_id` | `docker kill container_id` |

Both `docker stop` and `docker kill` can be used to stop a running container, but their approach is different, and the choice between the two depends on whether you want to allow the container to shut down gracefully (`docker stop`) or you need to immediately terminate it (`docker kill`).

## docker top

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker top` | `docker top container_id` | Shows the running processes in a specific container. Replace `container_id` with the ID or name of the container you want to check. |
| Custom format | `docker top container_id args` | This command lets you specify arguments to change the format of the output. Replace `args` with the arguments. For example, `docker top container_id aux` will use the `aux` format, similar to the `ps aux` command in Linux. |

Remember, `docker top` gives you a snapshot of the processes running in a container at the moment you run the command.

It's useful for debugging and understanding what's happening inside a running container.

## docker stats

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker stats` | `docker stats` | Displays a live stream of resource usage statistics for all running containers. |
| Stats for specific containers | `docker stats container_id1 container_id2` | Displays live stream of resource usage statistics for specified containers. Replace `container_id1`, `container_id2` with the IDs or names of the containers you want to check. |
| Stats with specific output format | `docker stats --format "{{.Container}}: {{.CPUPerc}}"` | Allows you to specify the output format. The example command will output the container name and the CPU usage percentage. |
| Stats without streaming | `docker stats --no-stream` | Gives a snapshot of the current statistics and exits instead of displaying a live stream. |

`docker stats` gives you real-time information about CPU usage, memory usage, network IO, and more.

It's an excellent tool for monitoring the performance of your containers.
