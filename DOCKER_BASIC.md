# Docker_Lectures
## Docker Overview

### Containerization vs. Virtualization

- **Virtualization**: Uses virtual machines (VMs) with fixed hardware allocations and separate guest operating systems.
- **Containerization**: Shares the host OS kernel without fixed hardware allocations, leading to lightweight and efficient process isolation.

### Docker Concepts

1. **Docker Images**: Snapshot of a filesystem and configuration needed to run an application. They include binaries, libraries, and application code.
2. **Docker Containers**: Running instances of Docker images. Containers are isolated from each other and the host system.
3. **Docker Host**: The machine where Docker is installed and running.
4. **Docker Client**: The interface (e.g., terminal or Git Bash) used to interact with Docker commands.

### Setting Up Docker on an AWS EC2 Instance

1. **Create an Ubuntu Machine on AWS**: Set the security group to allow all traffic.
2. **Connect using Git Bash**.
3. **Install Docker**:
   ```bash
   sudo su -
   curl -fsSL https://get.docker.com -o get-docker.sh
   sh get-docker.sh
   ```
4. **Verify Docker Installation**:
   ```bash
   docker --version
   ```

### Docker Commands

#### Working with Images
1. **Download an Image**: `docker pull image_name`
2. **List Images**: `docker image ls` or `docker images`
3. **Delete an Image**: `docker rmi image_name/image_id`
4. **Upload an Image to Docker Hub**: `docker push image_name`
5. **Tag an Image**: `docker tag image_name ipaddress_of_local_registry:5000/image_name`
6. **Build an Image from a Customized Container**: `docker commit container_name/container_id new_image_name`
7. **Create an Image from a Dockerfile**: `docker build -t new_image_name .`
8. **Search for an Image**: `docker search image_name`
9. **Delete Unused Images**: `docker system prune -a`

#### Working with Containers
10. **List Running Containers**: `docker container ls`
11. **List All Containers (Running and Stopped)**: `docker ps -a`
12. **Start a Container**: `docker start container_name/container_id`
13. **Stop a Running Container**: `docker stop container_name/container_id`
14. **Restart a Container**: `docker restart container_name/container_id`
    - **Restart After 10 Seconds**: `docker restart -t 10 container_name/container_id`
15. **Delete a Stopped Container**: `docker rm container_name/container_id`
16. **Delete a Running Container**: `docker rm -f container_name/container_id`
17. **Stop All Running Containers**: `docker stop $(docker ps -aq)`
18. **Restart All Containers**: `docker restart $(docker ps -aq)`
19. **Remove All Stopped Containers**: `docker rm $(docker ps -aq)`
20. **Remove All Containers (Running and Stopped)**: `docker rm -f $(docker ps -aq)`
21. **View Logs from a Container**: `docker logs container_name/container_id`
22. **View Ports Used by a Container**: `docker port container_name/container_id`
23. **Inspect a Container**: `docker inspect container_name/container_id`
24. **Attach to a Running Container**: `docker attach container_name/container_id`
25. **Execute a Command in a Container**: `docker exec -it container_name/container_id command`
    - **Launch Bash Shell**: `docker exec -it container_name/container_id bash`
26. **Create a Container from an Image**: `docker run image_name`

### Run Command Options
- **-it**: Open an interactive terminal in a container.
- **--name**: Assign a name to a container.
- **-d**: Run the container in detached mode.
- **-e**: Pass environment variables to the container.
- **-p**: Port mapping between container and host.
- **-P**: Automatic port mapping.
- **-v**: Attach a volume to the container.
- **--volume-from**: Share volume between containers.
- **--network**: Specify a network for the container.
- **--link**: Link containers for multi-container setups.
- **--memory**: Specify the maximum RAM for the container.


#### Example Commands

1. **Pull Docker Images**:
   - **Tomcat**: `docker pull tomee`
   - **Ubuntu**: `docker pull ubuntu`
   - **Jenkins**: `docker pull jenkins/jenkins`

2. **Create and Manage Containers**:

   - **Tomcat Container**:
     ```bash
     # Create and run a Tomcat container named "c1" with port mapping
     docker run --name c1 -p 7070:8080 tomee

     # Check if Tomcat is running
     http://<docker_host_ip>:7070

     # Stop and remove the Tomcat container
     docker stop c1
     docker rm -f c1

     # Run Tomcat in detached mode
     docker run --name mytomcat -p 7070:8080 -d tomee
     ```

   - **Jenkins Container**:
     ```bash
     # Start Jenkins container in detached mode with port mapping
     docker run --name myjenkins -p 9090:8080 -d jenkins/jenkins

     # Check Jenkins status
     http://<docker_host_ip>:9090
     ```

   - **Ubuntu Container**:
     ```bash
     # Create and run an Ubuntu container with interactive terminal
     docker run --name myubuntu -it ubuntu

     # Inside the container
     ls   # List files
     exit # Exit the container
     ```

3. **Scenario Examples**:

   - **Scenario 1: Tomcat Container**:
     ```bash
     # Start Tomcat as a container named "webserver" in detached mode
     docker run --name webserver -p 7070:8080 -d tomee

     # Access Tomcat homepage
     http://<docker_host_ip>:7070
     ```

   - **Scenario 2: Jenkins Container**:
     ```bash
     # Start Jenkins as a container named "devserver" in detached mode
     docker run -d --name devserver -p 9090:8080 jenkins

     # Access Jenkins homepage
     http://<docker_host_ip>:9090
     ```

   - **Scenario 3: Nginx Container**:
     ```bash
     # Start Nginx as a container named "appserver" with automatic port mapping
     docker run --name appserver -P -d nginx

     # Check which port is reserved for Nginx
     docker port appserver
     # or
     docker container ls

     # Access Nginx homepage
     http://<docker_host_ip>:<host_port>
     ```

   - **CentOS Container**:
     ```bash
     # Start CentOS container with an interactive terminal
     docker run --name mycentos -it centos
     exit  # Exit the container
     ```

These commands and scenarios provide a solid foundation for managing Docker containers and images. If you need more details or have specific use cases, feel free to ask!

### Docker Commands and Scenarios

#### MySQL Container
To start a MySQL container, open an interactive terminal, and create sample tables:

1. **Start MySQL Container**:
   ```bash
   docker run --name mydb -d -e MYSQL_ROOT_PASSWORD=sunil mysql:5
   ```

2. **Open Bash Terminal in MySQL Container**:
   ```bash
   docker exec -it mydb bash
   ```

3. **Connect to MySQL Database**:
   ```bash
   mysql -u root -p
   ```

4. **Inside MySQL Prompt**:
   ```sql
   -- List databases
   show databases;

   -- Switch to a database
   use db_name;
   use mysql;

   -- Create tables (replace with actual SQL commands)
   ```

5. **Exit Containers**:
   ```bash
   exit  # Exit MySQL prompt
   exit  # Exit bash terminal
   ```

#### Multi-Container Architecture

1. **Using `--link` Option**

   **Create BusyBox Containers and Link Them**:
   ```bash
   # Create 1st BusyBox container
   docker run --name c10 -it busybox

   # Create 2nd BusyBox container and link to c10
   docker run --name c20 --link c10:c10-alias -it busybox
   ```

   **Check Link**:
   ```bash
   # Inside c20 container
   ping c10
   ```

2. **Development Environment with MySQL and WordPress**:

   ```bash
   # Start MySQL container
   docker run --name mydb -d -e MYSQL_ROOT_PASSWORD=sunil mysql:5

   # Start WordPress container and link to MySQL
   docker run --name mysite -d -p 5050:80 --link mydb:mysql wordpress
   ```

   **Access WordPress**:
   ```plaintext
   http://<docker_host_ip>:5050
   ```

3. **LAMP Architecture**

   ```bash
   # Start MySQL container
   docker run --name mydb -d -e MYSQL_ROOT_PASSWORD=sunil mysql:5

   # Start Tomcat container and link to MySQL
   docker run --name apache -d -p 6060:8080 --link mydb:mysql tomee

   # Start PHP container and link to Tomcat and MySQL
   docker run --name php -d --link apache:tomcat --link mydb:mysql php
   ```

   **Inspect Container**:
   ```bash
   docker inspect apache
   ```

4. **CI/CD Environment with Jenkins and Tomcat Containers**

   ```bash
   # Start Jenkins container
   docker run --name devserver -d -p 7070:8080 jenkins/jenkins

   # Start QA Tomcat container
   docker run --name qaserver -d -p 8080:8080 --link devserver:jenkins tomee

   # Start Prod Tomcat container
   docker run --name prodserver -d -p 9090:8080 --link devserver:jenkins tomee
   ```

   **Access Jenkins and Tomcat**:
   ```plaintext
   Jenkins: http://<docker_host_ip>:7070
   QA Tomcat: http://<docker_host_ip>:8080
   Prod Tomcat: http://<docker_host_ip>:9090
   ```

5. **Testing Environment with Selenium**

   ```bash
   # Start Selenium Hub container
   docker run --name hub -d -p 4444:4444 selenium/hub

   # Start Chrome node container and link to Hub
   docker run --name chrome -d -p 5901:5900 --link hub:selenium selenium/node-chrome-debug

   # Start Firefox node container and link to Hub
   docker run --name firefox -d -p 5902:5900 --link hub:selenium selenium/node-firefox-debug
   ```

   **Access GUI**:
   - **Chrome**: Use VNC Viewer to connect to `public_ip:5901` (Password: `secret`)
   - **Firefox**: Use VNC Viewer to connect to `public_ip:5902` (Password: `secret`)

#### Docker Compose

1. **Install Docker Compose**:

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   ```

   **Check Docker Compose Version**:
   ```bash
   docker-compose --version
   ```

2. **Create a Multi-Container Setup using Docker Compose**:

   Create a `docker-compose.yml` file:

   ```yaml
   version: '3'
   services:
     web:
       image: nginx
       ports:
         - "8080:80"
     db:
       image: mysql:5
       environment:
         MYSQL_ROOT_PASSWORD: sunil
   ```

   **Start Services**:
   ```bash
   docker-compose up -d
   ```

   **Stop Services**:
   ```bash
   docker-compose down
   ```

These commands and scenarios will help you get started with various Docker configurations and setups. If you have more specific needs or questions, let me know!


### Docker Compose Guide

Docker Compose simplifies setting up multi-container environments using a YAML file. Here's a detailed guide on creating and managing Docker Compose files for various scenarios:

#### General Docker Compose Setup

1. **YAML File Format**:
   - YAML files use key-value pairs and are space-indented.
   - Example of a YAML structure:
     ```yaml
     ---
     logiclabs:
       trainers:
         sunil: Devops
         raj: Python
       Coordinators:
         lakshmi: Devops
         rani: AWS
     ```

2. **Validate YAML File**:
   - Use [YAML Lint](http://www.yamllint.com/) to validate your YAML file.

3. **Installing Docker Compose**:
   - Follow the installation steps from the [Docker Compose installation guide](https://docs.docker.com/compose/install/):
     ```bash
     sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
     sudo chmod +x /usr/local/bin/docker-compose
     ```
   - Check installation:
     ```bash
     docker-compose --version
     ```

#### Sample Docker Compose Files

1. **Development Environment (MySQL and WordPress)**:

   Create a `docker-compose.yml` file:
   ```yaml
   version: '3'

   services:
     mydb:
       image: mysql:5
       environment:
         MYSQL_ROOT_PASSWORD: sunilsunil

     mysite:
       image: wordpress
       ports:
         - 5050:80
       links:
         - mydb:mysql
   ```

   **Commands**:
   - Start services:
     ```bash
     docker-compose up -d
     ```
   - Stop services:
     ```bash
     docker-compose stop
     ```
   - Remove containers:
     ```bash
     docker rm -f $(docker ps -aq)
     ```

   **Access WordPress**:
   ```plaintext
   http://<public_ip>:5050
   ```

2. **LAMP Architecture (MySQL, Tomcat, and PHP)**:

   Create a `docker-compose.yml` file:
   ```yaml
   version: '3'

   services:
     mydb:
       image: mysql:5
       environment:
         MYSQL_ROOT_PASSWORD: sunilsunil

     apache:
       image: tomee
       ports:
         - 6060:8080
       links:
         - mydb:mysql

     php:
       image: php
       links:
         - mydb:mysql
         - apache:tomcat
   ```

   **Commands**:
   - Start services:
     ```bash
     docker-compose up -d
     ```
   - Check the containers:
     ```bash
     docker container ls
     ```

3. **CI/CD Environment (Jenkins and Tomcat)**:

   Create a `docker-compose.yml` file:
   ```yaml
   version: '3'

   services:
     devserver:
       image: jenkins/jenkins
       ports:
         - 7070:8080

     qaserver:
       image: tomee
       ports:
         - 8899:8080
       links:
         - devserver:jenkins

     prodserver:
       image: tomee
       ports:
         - 9090:8080
       links:
         - devserver:jenkins
   ```

   **Commands**:
   - Start services:
     ```bash
     docker-compose up -d
     ```
   - Check the containers:
     ```bash
     docker container ls
     ```

   **Access Services**:
   ```plaintext
   Jenkins: http://<public_ip>:7070
   QA Tomcat: http://<public_ip>:8899
   Prod Tomcat: http://<public_ip>:9090
   ```

4. **Testing Environment (Selenium Hub with Chrome and Firefox Nodes)**:

   Create a `docker-compose.yml` file:
   ```yaml
   version: '3'

   services:
     hub:
       image: selenium/hub
       ports:
         - 4444:4444

     chrome:
       image: selenium/node-chrome-debug
       ports:
         - 5901:5900
       links:
         - hub:selenium

     firefox:
       image: selenium/node-firefox-debug
       ports:
         - 5902:5900
       links:
         - hub:selenium
   ```

   **Commands**:
   - Start services:
     ```bash
     docker-compose up -d
     ```
   - Check the containers:
     ```bash
     docker container ls
     ```

   **Access GUI**:
   - **Chrome**: Use VNC Viewer at `public_ip:5901` (Password: `secret`)
   - **Firefox**: Use VNC Viewer at `public_ip:5902` (Password: `secret`)

#### Common Commands

- **Start Containers**:
  ```bash
  docker-compose up -d
  ```

- **Stop Containers**:
  ```bash
  docker-compose stop
  ```

- **Remove Containers**:
  ```bash
  docker rm -f $(docker ps -aq)
  ```

These setups allow you to create and manage complex multi-container environments efficiently using Docker Compose. If you need further assistance or customization, feel free to ask!


### Docker Volumes

Docker volumes are used to persist data generated by and used by Docker containers. Here’s a breakdown of how to use and manage Docker volumes effectively.

#### Types of Docker Volumes

1. **Simple Docker Volumes**:
   - Used to persist data even after the container is deleted, but cannot be shared between containers.

2. **Docker Volume Containers (Reusable Volumes)**:
   - These volumes can be shared between multiple containers, and the data persists even if all containers are deleted.

#### Working with Simple Docker Volumes

1. **Create and Use a Simple Volume**:

   - **Create a Directory**:
     ```bash
     mkdir /data
     ```

   - **Start a CentOS Container with the Volume**:
     ```bash
     docker run --name c1 -it -v /data centos
     ```

   - **Inside the Container**:
     ```bash
     cd /data
     touch file1 file2
     ls
     ```

   - **Exit the Container**:
     ```bash
     exit
     ```

   - **Inspect the Container to Find the Volume Path**:
     ```bash
     docker inspect c1
     ```

   - **Remove the Container**:
     ```bash
     docker rm -f c1
     ```

   - **Verify Data Persistence**:
     ```bash
     cd /var/lib/docker/volumes/<volume_id>/_data
     ls
     ```

   (Note: Replace `<volume_id>` with the actual volume ID obtained from the inspect command.)

#### Working with Docker Volume Containers

1. **Create a Shared Volume**:

   - **Start a CentOS Container with Volume**:
     ```bash
     docker run --name c1 -it -v /data centos
     ```

   - **Create Files Inside the Container**:
     ```bash
     cd /data
     touch file1 file2
     ```

   - **Detach from Container without Exiting**:
     ```bash
     Ctrl + p + Ctrl + q
     ```

   - **Start Another Container with the Same Volume**:
     ```bash
     docker run --name c2 -it --volumes-from c1 centos
     ```

   - **Verify Files in the Second Container**:
     ```bash
     cd /data
     ls
     ```

   - **Create Additional Files**:
     ```bash
     touch file3 file4
     ```

   - **Detach from Container**:
     ```bash
     Ctrl + p + Ctrl + q
     ```

   - **Start a Third Container with the Same Volume**:
     ```bash
     docker run --name c3 -it --volumes-from c2 centos
     ```

   - **Verify Files in the Third Container**:
     ```bash
     cd /data
     ls
     touch file5 file6
     ```

   - **Inspect the Volume Path**:
     ```bash
     docker inspect c1
     ```

   - **Remove All Containers**:
     ```bash
     docker rm -f c1 c2 c3
     ```

   - **Check Data Persistence**:
     ```bash
     cd /var/lib/docker/volumes/<volume_id>/_data
     ls
     ```

#### Creating Customized Docker Images

Custom Docker images allow you to save the state of a container with all its installed software.

1. **Using `docker commit`**:

   - **Run a Container and Install Software**:
     ```bash
     docker run --name c11 -it ubuntu
     apt-get update
     apt-get install git
     git --version
     exit
     ```

   - **Commit the Container to an Image**:
     ```bash
     docker commit c11 myubuntu
     ```

   - **Verify the Image**:
     ```bash
     docker images
     ```

   - **Run a Container from the New Image**:
     ```bash
     docker run --name c22 -it myubuntu
     ```

   - **Verify Installed Software**:
     ```bash
     git --version
     ```

#### Summary

- **Simple Volumes**: Persist data but not shared between containers.
- **Volume Containers**: Share data between containers and persist even if containers are deleted.
- **Custom Images**: Save the state of containers with installed software using `docker commit`.

Feel free to ask if you have any questions or need further clarification!

### Creating Customized Docker Images with Dockerfile

A Dockerfile is a script with a set of instructions used to build a Docker image. Here’s a summary of the keywords and their uses:

#### Key Dockerfile Instructions

1. **FROM**: Specifies the base image for the Docker image.
   ```dockerfile
   FROM ubuntu
   ```

2. **MAINTAINER**: Defines the author or organization maintaining the Dockerfile.
   ```dockerfile
   MAINTAINER logiclabs
   ```

3. **CMD**: Sets the default command to be executed when the container starts. Only one CMD instruction is allowed per Dockerfile.
   ```dockerfile
   CMD ["date"]
   ```

4. **ENTRYPOINT**: Defines the default executable for the container. Can also be used with CMD to pass arguments.
   ```dockerfile
   ENTRYPOINT ["java", "-jar", "jenkins.war"]
   ```

5. **RUN**: Executes commands inside the container at build time. Commonly used to install software.
   ```dockerfile
   RUN apt-get update && apt-get install -y git
   ```

6. **USER**: Specifies the default user for the container.
   ```dockerfile
   USER appuser
   ```

7. **WORKDIR**: Sets the working directory for subsequent instructions.
   ```dockerfile
   WORKDIR /app
   ```

8. **COPY**: Copies files or directories from the host machine to the container.
   ```dockerfile
   COPY . /app
   ```

9. **ADD**: Similar to COPY, but also supports downloading files from remote URLs.
   ```dockerfile
   ADD http://example.com/file.tar.gz /app
   ```

10. **ENV**: Sets environment variables in the container.
    ```dockerfile
    ENV APP_ENV=production
    ```

11. **EXPOSE**: Indicates the port number on which the container listens.
    ```dockerfile
    EXPOSE 8080
    ```

12. **VOLUME**: Defines mount points for volumes.
    ```dockerfile
    VOLUME ["/data"]
    ```

13. **LABEL**: Adds metadata to the image.
    ```dockerfile
    LABEL version="1.0"
    ```

14. **STOPSIGNAL**: Specifies the signal used to stop the container.
    ```dockerfile
    STOPSIGNAL SIGTERM
    ```

#### Creating a Dockerfile Example

1. **Base Image Example**:
   - Use `nginx` as the base image and specify `logiclabs` as the maintainer.
   ```dockerfile
   FROM nginx
   MAINTAINER logiclabs
   ```

   - **Build the Image**:
     ```bash
     docker build -t mynginx .
     ```

2. **Running Commands on Container Start**:
   - Create a Dockerfile to run a command (e.g., `date`) when the container starts.
   ```dockerfile
   FROM centos
   MAINTAINER logiclabs
   CMD ["date"]
   ```

   - **Build and Run the Image**:
     ```bash
     docker build -t mycentos .
     docker run -it mycentos
     ```

3. **Handling Multiple CMD Instructions**:
   - Only the last CMD instruction will be executed.
   ```dockerfile
   FROM centos
   MAINTAINER logiclabs
   CMD ["date"]
   CMD ["ls", "-la"]
   ```

   - **Build and Run the Image**:
     ```bash
     docker build -t mycentos .
     docker run -it mycentos
     ```

4. **Installing Software**:
   - Install `git` in an Ubuntu container.
   ```dockerfile
   FROM ubuntu
   MAINTAINER logiclabs
   RUN apt-get update
   RUN apt-get install -y git
   ```

   - **Build and Run the Image**:
     ```bash
     docker build -t myubuntu .
     docker run -it myubuntu
     git --version
     ```

5. **Cache Busting**:
   - To ensure packages are up-to-date, use `&&` to combine commands.
   ```dockerfile
   FROM ubuntu
   MAINTAINER logiclabs
   RUN apt-get update && apt-get install -y git tree
   ```

   - **Build the Image**:
     ```bash
     docker build -t myubuntu .
     ```

6. **Customizing Default Process**:
   - Create a Dockerfile to install Java, download `jenkins.war`, and set it as the default process.
   ```dockerfile
   FROM ubuntu
   MAINTAINER logiclabs
   RUN apt-get update
   RUN apt-get install -y default-jdk
   ADD http://mirrors.jenkins.io/war-stable/latest/jenkins.war /
   ENTRYPOINT ["java", "-jar", "jenkins.war"]
   ```

   - **Build and Run the Image**:
     ```bash
     docker build -t myubuntu .
     docker run myubuntu
     ```

### Summary

- **Dockerfile**: A script with instructions to create Docker images.
- **CMD vs ENTRYPOINT**: CMD sets default commands, ENTRYPOINT sets the default executable.
- **RUN**: Executes commands during image build.
- **Cache Busting**: Use `&&` to ensure updated package installations.

Feel free to ask if you need further details or examples!

Your notes on Docker Registry and Docker Swarm look thorough! Here’s a quick summary and some additional tips:

### Docker Registry
1. **Types of Registries:**
   - **Public Registry:** Docker Hub (hub.docker.com)
   - **Private Registry:** For private usage or organizational needs.

2. **Steps to Upload a Customized Image:**
   - **Create an Image:** 
     ```bash
     docker run --name c5 -it ubuntu
     apt-get update
     apt-get install tree
     exit
     ```
   - **Commit the Container to an Image:**
     ```bash
     docker commit c5 sunildevops77/ubuntu_img26
     ```
   - **Push the Image to Docker Hub:**
     ```bash
     docker login
     docker push sunildevops77/ubuntu_img26
     ```

### Docker Swarm
Here's a complete and detailed guide on Docker Swarm for container orchestration, including all steps and commands:

---

### **Container Orchestration**

**Definition**: The process of running Docker containers in a distributed environment across multiple Docker host machines. Containers can run a single service and share resources even if they are on different hosts.

**Tool**: Docker Swarm.

### **Advantages of Docker Swarm**

1. **Load Balancing**: Distributes traffic evenly across containers.
2. **Scaling**: Adjusts the number of container replicas based on demand.
3. **Rolling Updates**: Updates services incrementally to avoid downtime.
4. **Failover Handling**: Manages node failures by redistributing tasks.

---

### **Setting Up Docker Swarm**

1. **Prepare Machines**
   - **Machines**: Manager, Worker1, Worker2
   - **Install Docker**: On all machines, run:
     ```bash
     curl -fsSL https://get.docker.com -o get-docker.sh
     sudo sh get-docker.sh
     ```

2. **Optional: Change Hostname on Manager**
   - **Change Hostname**:
     ```bash
     sudo vim /etc/hostname
     ```
     Set the hostname to `Manager` and save (`:wq`).

   - **Restart Machine**:
     ```bash
     sudo init 6
     ```

   - **Repeat** for Worker1 and Worker2, setting their hostnames accordingly.

3. **Initialize Docker Swarm on Manager**
   - **Connect to Manager**:
     ```bash
     sudo su -
     ```
   - **Initialize Swarm**:
     ```bash
     docker swarm init --advertise-addr <private_ip_of_manager>
     ```
     Example:
     ```bash
     docker swarm init --advertise-addr 172.31.27.151
     ```
   - **Copy the join command** provided in the output logs for joining workers.

4. **Join Workers to Swarm**
   - **Connect to Worker1**:
     ```bash
     sudo su -
     ```
   - **Execute Join Command**:
     ```bash
     docker swarm join --token <join_token> <manager_ip>:2377
     ```
     Example:
     ```bash
     docker swarm join --token SWMTKN-1-0etsmfa26vreeytq278q8ohhi73il7j1lpnrzzlowuld1r8yex-9x04pjmiq85jxjzjayzlglh1c 172.31.27.151:2377
     ```

   - **Repeat for Worker2**.

---

### **Managing Services**

1. **Create Services**
   - **Tomcat (5 replicas)**:
     ```bash
     docker service create --name webserver -p 9090:8080 --replicas 5 tomee
     ```
   - **MySQL (3 replicas)**:
     ```bash
     docker service create --name mydb --replicas 3 -e MYSQL_ROOT_PASSWORD=sunil mysql:5
     ```

2. **Check Service Status**
   - **List Services**:
     ```bash
     docker service ls
     ```
   - **View Tasks**:
     ```bash
     docker service ps <service_name>
     ```
     Example:
     ```bash
     docker service ps webserver
     ```

3. **Scaling Services**
   - **Create Service with 5 Replicas**:
     ```bash
     docker service create --name appserver -p 8080:80 --replicas 5 nginx
     ```

   - **Scale Service**
     - **Increase Replicas**:
       ```bash
       docker service scale appserver=10
       ```
     - **Decrease Replicas**:
       ```bash
       docker service scale appserver=2
       ```

---

### **Node Management**

1. **Remove a Node**
   - **Drain Node**:
     ```bash
     docker node update --availability drain <node_name>
     ```
     Example:
     ```bash
     docker node update --availability drain Worker1
     ```

   - **Leave Swarm**:
     ```bash
     docker swarm leave
     ```

2. **Add Node**
   - **Generate Join Command**:
     ```bash
     docker swarm join-token worker
     ```
   - **Join Command**:
     ```bash
     docker swarm join --token <join_token> <manager_ip>:2377
     ```

   - **Promote/Demote Nodes**
     - **Promote**:
       ```bash
       docker node promote <worker_name>
       ```
     - **Demote**:
       ```bash
       docker node demote <manager_name>
       ```

---

### **Rolling Updates and Rollbacks**

1. **Rolling Update**
   - **Create Redis Service (6 replicas)**:
     ```bash
     docker service create --name myredis --replicas 6 redis:3
     ```

   - **Update to Redis 4**:
     ```bash
     docker service update --image redis:4 myredis
     ```

   - **View Running Containers**:
     ```bash
     docker service ps myredis | grep -v Shutdown
     ```

2. **Rolling Rollback**
   - **Rollback to Redis 3**:
     ```bash
     docker service update --rollback myredis
     ```

---

### **Adding New Nodes**

1. **To Add a Worker Node**:
   - **Generate Join Token**:
     ```bash
     docker swarm join-token worker
     ```
   - **Join Command**:
     ```bash
     docker swarm join --token <join_token> <manager_ip>:2377
     ```

2. **To Add a Manager Node**:
   - **Generate Join Token for Manager**:
     ```bash
     docker swarm join-token manager
     ```
   - **Join Command**:
     ```bash
     docker swarm join --token <join_token> <manager_ip>:2377
     ```

3. **Check Leadership**
   - **View Leader**:
     ```bash
     docker node ls
     ```

   - **Automatic Leader Election**: Docker Swarm automatically selects a leader. If one manager goes down, another manager becomes the leader.

---

### **Deleting Services**

- **Remove Services**:
  ```bash
  docker service rm <service_name>
  ```
  Example:
  ```bash
  docker service rm appserver mydb webserver
  ```

This guide covers the complete setup and management of Docker Swarm, including node management, service creation, scaling, and updates. Let me know if you need further details or assistance with specific aspects!


Sure! Here's a structured explanation of Docker networking and container communication:

---

## Docker Networking and Container Communication

### 1. **Docker Installation**

To install Docker on your system:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

### 2. **Basic Container Operations**

- **Run a Container:**
  ```bash
  docker run -d -P nginx
  ```
  This command starts an `nginx` container in detached mode and maps its ports to random host ports.

- **Check Running Containers:**
  ```bash
  docker ps
  ```
  This lists the running containers.

- **Inspect Container:**
  ```bash
  docker inspect <container_id>
  ```
  Use this command to view detailed information about a container, including its IP address.

### 3. **Default Network (`docker0`)**

- **View Default Network:**
  ```bash
  ip a
  ```
  This shows all network interfaces, including `docker0`.

  `docker0` typically has an IP address in the `172.x.x.x` range, and containers get IP addresses from this range.

- **Inspect Container IP:**
  ```bash
  docker inspect <container_id>
  ```
  Look for `"IPAddress": "172.17.0.x"` in the output.

### 4. **Communication Between Containers**

- **Create a New Container:**
  ```bash
  docker run -d -P redis
  ```

- **Check Connectivity:**
  To test communication, run a `busybox` container with the `ping` utility:
  ```bash
  docker run -d --name tester1 busybox:1.28 sleep 3600
  ```

  Use `docker exec` to ping:
  ```bash
  docker exec <busybox_container_id> ping <redis_container_id>
  docker exec <busybox_container_id> ping <redis_ip>
  ```

  **Observation:**
  - Ping by container ID or name within `docker0` network may not work.
  - Ping by IP address works, indicating that `docker0` allows communication via IP but not names.

### 5. **Custom Networks**

- **Create a Custom Network:**
  ```bash
  docker network create sunil
  ```

  Inspect the network to view its configuration:
  ```bash
  docker inspect sunil
  ```

- **Run Containers on Custom Network:**
  ```bash
  docker run -d -P --name app --net sunil nginx
  docker run -d -P --name mybox --net sunil busybox:1.28 sleep 3600
  ```

  Test communication using names:
  ```bash
  docker exec -it <busybox_container_id> sh
  ping app
  ```

  **Observation:**
  - Containers on the same custom network can communicate by name and IP.

### 6. **Network Types and Drivers**

- **Bridge Network:**
  - Default network type for containers on a single host.
  - Containers communicate within the same host only.

- **Host Network:**
  - Containers share the host’s network stack.
  - Ports must be unique, and containers use the host's IP.

  **Example:**
  ```bash
  docker run -d -P --net host nginx
  ```

- **None Network:**
  - No network connectivity for the container.

  **Example:**
  ```bash
  docker run -d -P --net none redis
  ```

### 7. **Swarm Mode and Overlay Network**

- **Initialize Swarm:**
  ```bash
  docker swarm init
  ```

- **Create Overlay Network:**
  ```bash
  docker network create ol1 --driver overlay
  ```

- **Create and Inspect Service:**
  ```bash
  docker service create --name mynginx --replicas 5 --network ol1 -p 1224:80 nginx
  docker ps
  docker inspect <container_id>
  ```

  **Observation:**
  - Containers in an overlay network can communicate across different machines.

### Summary

- **Docker0 Network:** Default bridge network, allows communication by IP.
- **Custom Networks:** Enable name-based communication and more control over network configuration.
- **Network Types:** Include bridge (default), host, none, and overlay (for Swarm).

This overview provides a structured understanding of Docker networking, from basic operations to advanced configurations using custom and overlay networks. Let me know if you need more details on any topic!
