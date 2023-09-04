# Docker System Pruning and Cleaning Commands

- [Docker System Pruning and Cleaning Commands](#docker-system-pruning-and-cleaning-commands)
  - [docker prune](#docker-prune)
  - [docker rm vs prune](#docker-rm-vs-prune)

## docker prune

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker system prune` | `docker system prune` | Removes all stopped containers, all dangling images, and all unused networks. You'll be asked for a confirmation before it deletes these resources. |
| Prune without confirmation | `docker system prune -f` | Removes all the resources as above but without asking for confirmation. |
| Prune including unused volumes | `docker system prune -a --volumes` | In addition to the resources mentioned above, this also removes all unused images (not just dangling ones) and all unused volumes. |

A few things to remember when using `docker system prune`:

- Be cautious with this command, especially with the `-a` and `--volumes` flags, as it can delete more than you might expect.
- "Dangling images" are untagged images, that are the leaves of the images tree (not intermediary layers).
- It won't remove volumes created by a `docker-compose.yml` file by default.

## docker rm vs prune

|     | `docker rm` | `docker system prune` |
| --- | ----------- | --------------------- |
| **Purpose** | Removes specific containers. | Removes unused data. |
| **Target** | Specific containers identified by their IDs or names. | All stopped containers, all networks not used by at least one container, all dangling images, and build cache. |
| **Effect on running containers** | Can forcefully remove if `-f` is used. | No effect on running containers. |
| **Effect on stopped containers** | Can remove if the container IDs are specified. | Removes all stopped containers. |
| **Effect on images and networks** | No effect. | Removes all dangling images and unused networks. |
| **Effect on volumes** | Can remove associated volumes if `-v` is used. | Removes all unused volumes if `--volumes` is used. |

So, you can see that `docker rm` is more targeted – you specify exactly what you want to remove. On the other hand, `docker system prune` is more of a cleanup command – it gets rid of unused or dangling resources.
