# Docker Command Categorization

Docker commands can be broadly categorized based on their functionality. Here's a basic grouping of Docker commands:

1. **[Container Lifecycle Management Commands](./DockerLifecycleCommands.md):**
   - `docker create`
   - `docker run`
   - `docker start`
   - `docker stop`
   - `docker restart`
   - `docker rm`

2. **[Container Operations Commands](./DockerOperationsCommands.md):**
   - `docker ps`
   - `docker exec`
   - `docker attach`
   - `docker logs`

3. **Image Management Commands:**
   - `docker images`
   - `docker rmi`
   - `docker pull`
   - `docker push`
   - `docker build`

4. **Network Management Commands:**
   - `docker network create`
   - `docker network rm`
   - `docker network ls`

5. **System Information Commands:**
   - `docker version`
   - `docker info`
   - `docker system df`

6. **[System Pruning and Cleaning Commands](./DockerPruningCommands.md):**
   - `docker system prune`
   - `docker container prune`
   - `docker image prune`
   - `docker network prune`
   - `docker volume prune`

Remember, this is just a basic categorization and the Docker CLI has more commands and subcommands that you can explore in the [official Docker documentation](https://docs.docker.com/engine/reference/commandline/cli/).
