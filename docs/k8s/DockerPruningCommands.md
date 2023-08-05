# Docker System Pruning and Cleaning Commands

## Docker Prune

| Usage | Command | Description |
| ----- | ------- | ----------- |
| Basic `docker system prune` | `docker system prune` | Removes all stopped containers, all dangling images, and all unused networks. You'll be asked for a confirmation before it deletes these resources. |
| Prune without confirmation | `docker system prune -f` | Removes all the resources as above but without asking for confirmation. |
| Prune including unused volumes | `docker system prune -a --volumes` | In addition to the resources mentioned above, this also removes all unused images (not just dangling ones) and all unused volumes. |

A few things to remember when using `docker system prune`:

- Be cautious with this command, especially with the `-a` and `--volumes` flags, as it can delete more than you might expect.
- "Dangling images" are untagged images, that are the leaves of the images tree (not intermediary layers).
- It won't remove volumes created by a `docker-compose.yml` file by default.
