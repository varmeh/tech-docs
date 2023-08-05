# Docker Operations Commands

Docker operation commands are used to manage running containers and their processes

## docker PS

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker ps` | `docker ps` | Shows all running containers. |
| Show All Containers | `docker ps -a` | Shows all containers, regardless of their status (running, exited, etc.). |
| Show Latest Created Container | `docker ps -l` | Shows the most recently created container, including all its details. |
| Show n Last Created Containers | `docker ps -n` | Shows the 'n' last created containers. Replace 'n' with the number of containers you want to list. |
| Show Containers & Size | `docker ps -s` | Shows all containers and their disk usage. |

Remember, `docker ps` will show you the container ID, which is often required for other Docker commands such as `docker start`, `docker stop`, and `docker rm`
