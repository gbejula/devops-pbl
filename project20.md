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

> ### STEP 1: PULL MYSQL DOCKER IMAGE FROM DOCKER HUB REGISTRY

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

> ### STEP 2: DEPLOY THE MYSQL CONTAINER TO DOCKER ENGINE

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

> ### Step 3: Connecting to the MySQL Docker Container

Approach 1

- Connecting directly to the container running the MySQL server:

```
docker exec -it mysql bash

or

$ docker exec -it mysql mysql -uroot -p
```

Provide the root password when prompted. With that, you’ve connected the MySQL client to the server.

Finally, change the server root password to protect your database. Exit the the shell with exit command Flags used

- exec used to execute a command from bash itself
- -it makes the execution interactive and allocate a pseudo-TTY
- bash this is a unix shell and its used as an entry-point to interact with our container
- mysql The second mysql in the command "docker exec -it mysql mysql -uroot -p" serves as the entry point to interact with mysql container just like bash or sh
- -u mysql username
- -p mysql password

Approach 2:

- At this stage, we remove the previous mysql docker container since we know how to create a docker container

  ```
  docker ps -a
  docker stop mysql
  docker rm mysql or <container ID> 04a34f46fb98
  ```

  ![removed mysql](images/project-20/remove-created-mysql.png)

- Verify the container is deleted using:

  ```
  docker ps -a
  ```

- Create a network

  ```
  docker network create --subnet=172.18.0.0/24 tooling_app_network
  ```

  ![docker network](images/project-20/docker-network.png)

- Creating a custom network is not necessary because even if we do not create a network, Docker will use the default network for all the containers that is run. Hence, verify the running docker network using:

- But there are use cases where this is necessary. For example, if there is a requirement to control the cidr range of the containers running the entire application stack. This will be an ideal situation to create a network and specify the **--subnet**

- For clarity’s sake, we will create a network with a subnet dedicated for our project and use it for both MySQL and the application so that they can connect.

  ```
  docker network ls
  ```

  ![check network](images/project-20/docker-check-network.png)

- Run the MySQL Server container using the created network.

- First, let us create an environment variable to store the root password:

  ```
  export MYSQL_PW=xxxxxxx
  The password should follow immediately without a space after the = sign.

  verify the password using echo:

  echo $MYSQL_PW
  ```

- This was my mistake previously as I did not add the password

  ![password wrong](images/project-20/en-variable-psd.png)

- I later corrected it and added a password

  ![password added](images/project-20/password.png)

- Then, pull the image and run the container, all in one command like below:

  ```
  docker run --network tooling_app_network -h mysqlserverhost --name=mysql-server -e MYSQL_ROOT_PASSWORD=$MYSQL_PW  -d mysql/mysql-server:latest
  ```

  ![new docker](images/project-20/create-mysql-db.png)

Flags used

- -d runs the container in detached mode
- --network connects a container to a network
- -h specifies a hostname

- Discovered that the container is not running

  ![container not running](images/project-20/server-not-running2.png)

- Then ran, commamnd

  ```
  docker container start name or container ID
  ```

  ![container running](images/project-20/server-starting.png)
