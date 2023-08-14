# Files in Docker Ecosystem

- [Files in Docker Ecosystem](#files-in-docker-ecosystem)
  - [Dockerfile](#dockerfile)
    - [What is a Dockerfile?](#what-is-a-dockerfile)
    - [Common Instructions](#common-instructions)
    - [Sample File](#sample-file)
    - [Building an Image with a Dockerfile](#building-an-image-with-a-dockerfile)
    - [Instruction Comparisons](#instruction-comparisons)
      - [COPY vs ADD](#copy-vs-add)
      - [CMD vs ENTRYPOINT](#cmd-vs-entrypoint)
  - [.dockerignore File](#dockerignore-file)
    - [What is a .dockerignore file?](#what-is-a-dockerignore-file)
    - [How to Use .dockerignore](#how-to-use-dockerignore)
    - [Common Patterns](#common-patterns)
    - [Example .dockerignore File](#example-dockerignore-file)
    - [Considerations](#considerations)
  - [References](#references)
    - [Dockerfile](#dockerfile-1)
    - [Dockerfile Best Practices](#dockerfile-best-practices)
    - [Dockerfile Multistage Builds](#dockerfile-multistage-builds)
    - [.dockerignore](#dockerignore)

## Dockerfile

### What is a Dockerfile?

A Dockerfile is a script containing instructions to create a Docker image. It allows you to automate the process of building images, making them consistent and reproducible.

### Common Instructions

| Instruction | Purpose                                              | Example                                         |
|-------------|------------------------------------------------------|-------------------------------------------------|
| `FROM`      | Sets the base image for the build.                   | `FROM python:3.7-slim`                          |
| `WORKDIR`   | Sets the working directory for subsequent commands.  | `WORKDIR /app`                                  |
| `RUN`       | Executes a command within the container.             | `RUN pip install Flask`                         |
| `CMD`       | Specifies the default command to run when starting.  | `CMD ["python", "app.py"]`                      |
| `ENTRYPOINT`| Similar to CMD but allows additional arguments.      | `ENTRYPOINT ["python", "app.py"]`               |
| `LABEL`     | Adds metadata to an image in form of `key-value` pair| `LABEL key="value"`                             |
| `COPY`      | Copies files from host to container.                 | `COPY . /app`                                   |
| `ADD`       | Similar to COPY but supports URLs & tar extraction.  | `ADD example.tar.gz /tmp`                       |
| `ENV`       | Sets environment variables in the container.         | `ENV NAME World`                                |
| `EXPOSE`    | Informs Docker the container listens on specific ports. | `EXPOSE 80`                                  |
| `VOLUME`    | Creates a mount point for persistent data.           | `VOLUME /data`                                  |
| `ARG`       | Defines variables that could be passed at build time with `docker build`  | `ARG mode=DEV` </br> `ARG version` |

For details, refer [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/).

### Sample File

```Dockerfile
# Use an official Python runtime as the base image
FROM python:3.7-slim

# Set the working directory
WORKDIR /app

# Copy the local code to the container
COPY . /app

# Install dependencies
RUN pip install --trusted-host pypi.python.org Flask

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

### Building an Image with a Dockerfile

You can build an image from a Dockerfile using the `docker build` command:

```bash
docker build -t myapp:latest .
```

### Instruction Comparisons

Both `COPY` and `ADD` instructions in a Dockerfile serve the purpose of copying files from the host machine into the image, but they have some differences in functionality. Here's a comparison:

#### COPY vs ADD

| Attribute             | COPY                                  | ADD                                   |
|-----------------------|---------------------------------------|---------------------------------------|
| **Basic Functionality**| Copies files and directories from the host to the image. | Similar to COPY, but with additional features. |
| **Remote URLs**       | Doesn't support copying from remote URLs. | Supports copying files from remote URLs. |
| **Tar Extraction**    | Doesn't extract tar archives.         | Automatically extracts tar archives into the destination directory. |
| **Wildcard Support**  | Supports wildcards in the source path. | Supports wildcards in the source path. |
| **Best Practices**    | Preferred for simple file copying as it's more explicit. | Use when tar extraction or remote URL functionality is needed. |

- **Examples**

  - **COPY**: For straightforward copying of local files.

    ```dockerfile
    COPY ./app /app
    ```

  - **ADD**: For handling tar archives or remote URLs.

    ```dockerfile
    ADD ./app.tar.gz /app
    ADD http://example.com/app.jar /app
    ```

- **Conclusion**

While `ADD` offers more features, it is generally recommended to use `COPY` unless you specifically need the tar extraction or remote URL capabilities of `ADD`. Using `COPY` can make the Dockerfile more clear and maintainable, as it explicitly indicates that you are only performing a simple copy operation.

#### CMD vs ENTRYPOINT

| Attribute             | CMD                                   | ENTRYPOINT                            |
|-----------------------|---------------------------------------|---------------------------------------|
| **Basic Functionality**| Specifies the command and arguments that will be executed when the container starts. | Similar to CMD, but with a more "fixed" command structure. |
| **Overriding**        | Can be overridden by specifying a command when running `docker run`. | The command specified in ENTRYPOINT is always executed. Arguments passed in `docker run` are appended to the ENTRYPOINT command. |
| **Usage**             | Best for defining default commands and arguments that can be easily overridden. | Best for wrapping a container around a specific command, making the container function like that executable. |
| **Form**              | Can be used in shell or exec form.    | Can be used in shell or exec form.    |
| **Combination**       | Often used with ENTRYPOINT to define default arguments for the entrypoint command. | Works with CMD to define a fixed command and allow additional arguments to be specified. |

- **Examples**

  - **CMD**: Used to set a default command that can be overridden.

    ```dockerfile
    CMD ["python", "app.py"]
    ```

  - **ENTRYPOINT**: Used to define the container's main command, making the container behave like that command.

    ```dockerfile
    ENTRYPOINT ["python", "app.py"]
    CMD ["--version"]
    ```

  In this example, running `docker run <image>` will execute `python app.py --version`. If you provide additional arguments to `docker run`, they will replace `--version` but keep the ENTRYPOINT command.

- **Conclusion**

- Use `CMD` if you want to define a default command that users can easily override.
- Use `ENTRYPOINT` if you want to make your container behave like a specific command-line tool, with parameters passed to `docker run` behaving like parameters to that tool.

It's common to use both in combination to define a main command with ENTRYPOINT and default arguments with CMD, allowing users to provide additional or alternative arguments.

## .dockerignore File

### What is a .dockerignore file?

A `.dockerignore` file allows you to specify patterns for files and directories that should be excluded from the build context when building a Docker image.

By excluding unnecessary files, you can reduce the build context size and potentially speed up the build process.

### How to Use .dockerignore

To use a `.dockerignore` file, simply create a file named `.dockerignore` in the same directory as your Dockerfile and include the patterns for the files and directories you wish to exclude.

### Common Patterns

Here are some common patterns you might include in a `.dockerignore` file:

| Pattern          | Purpose                                        | Example                  |
|------------------|------------------------------------------------|--------------------------|
| `*`              | Ignores all files with a certain extension.    | `*.log`                  |
| `/dir/*`         | Ignores all files in a specific directory.     | `/temp/*`                |
| `!`              | Exception to a pattern.                        | `!important.log`         |
| `filename`       | Ignores a specific file.                       | `debug.log`              |

### Example .dockerignore File

Here's an example of what a `.dockerignore` file might look like for a python fastapi project:

```.dockerignore
# Byte-compiled files
*.pyc
*.pyo
__pycache__/

# Development files and directories
.env
.venv/
.vscode/
.idea/

# Markdown files - Exclude all except README.md
*.md
!README.md
docs/

# Git related files
.git/
.gitignore

# Unit test and coverage reports
tests/
htmlcov/
.coverage
*.coveragerc

# Logs and databases
*.log
*.sql
*.sqlite

# Build and cache directories
build/
dist/
*.egg-info/
.cache/
node_modules/
```

### Considerations

- It's a good practice to ignore files that are not needed in the build, such as local environment configurations, temporary files, and logs.
- Patterns in `.dockerignore` are processed in the order they appear, so more specific patterns should be listed before more general ones.

## References

### [Dockerfile](https://docs.docker.com/engine/reference/builder)

### [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

### [Dockerfile Multistage Builds](https://docs.docker.com/build/building/multi-stage/)

### [.dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
