# PROJECT 20: MIGRATION TO THE CLOUD WITH CONTAINERIZATION PART 1 - DOCKER & DOCKER COMPOSE.

- **Install Docker and prepare for migration to the Cloud**

- Install Docker Engine, which is a client-server application that contains:

- A server with a long-running daemon process **_dockerd_**.
- APIs that specify interfaces that programs can use to talk to and instruct the Docker daemon.
- A command-line interface (CLI) client **_docker_**.

- Install Docker Engine using either of the following links below

  - first
  - second

- **Remember our Tooling website? It is a PHP-based web solution backed by a MySQL database – all technologies you are already familiar with and which you shall be comfortable using by now.**

- Set up MySQL in container

> ## STEP 1: PULL MYSQL DOCKER IMAGE FROM DOCKER HUB REGISTRY

- Start by pulling the appropriate Docker image for MySQL. You can download a specific version or opt for the latest release, as seen in the following command:

  ```
  docker pull mysql/mysql-server:latest
  ```

  ![docker pull](images/project-20/docker-pull.png)

- Check that the image is downloaded successfully.

  ```
  docker image ls
  ```

  ![docker images](images/project-20/check-docker-images.png)

> ## STEP 2: DEPLOY THE MYSQL CONTAINER TO DOCKER ENGINE

- Once the image is available, move on to deploying a new MySQL container with the command below:

  ```
  docker run --name <container_name> -e MYSQL_ROOT_PASSWORD=<my-secret-pw> -d mysql/mysql-server:latest
  ```

  - Replace <container_name> with the name of your choice. If you do not provide a name, Docker will generate a random one
  - The -d option instructs Docker to run the container as a service in the background
  - Replace <my-secret-pw> with your chosen password
  - In the command above, we used the latest version tag. This tag may differ according to the image you downloaded

- Check the MySQL container that is running using the command:

  ```
  docker ps -a
  ```

  ![docker running](images/project-20/running-mysql-container.png)
