# Getting Started With Docker on AWS Ubuntu Instance
## Understanding Containers
What are Containers?
Containers are lightweight, standalone, executable packages that include everything needed to run a piece of software, including the code, runtime, system tools, system libraries, and settings. They provide a consistent environment for applications to run across different computing environments.

## Containers vs. Virtual Machines
The key difference between containers and virtual machines lies in their architecture:

Virtual Machines (VMs):

Run a complete operating system including the kernel
Require a hypervisor to manage virtual hardware
Each VM includes a full copy of an OS, applications, and dependencies
Typically take up several gigabytes of storage
Slower startup times (minutes)
Higher resource overhead (CPU, RAM, storage)
Containers:

Share the host system's kernel
Run as isolated processes in user space
Include only the application and its dependencies
Typically take up megabytes of storage
Fast startup times (seconds)
Lower resource overhead

## Benefits of Containers
Containers solve several deployment challenges:

Portability: Containers run consistently across development, testing, and production environments, eliminating the "it works on my machine" problem.
Resource Efficiency: Multiple containers can run on a single machine, sharing the OS kernel and resources, leading to better utilization of hardware.
Speed: Containers start almost instantly, enabling rapid scaling and deployment.
Isolation: Each container runs in its own isolated environment, preventing conflicts between applications.
Version Control: Container images can be versioned, rolled back, and tracked using container registries.
Microservices Architecture: Containers facilitate the decomposition of monolithic applications into smaller, independently deployable services.
DevOps Integration: Containers support continuous integration and continuous deployment (CI/CD) pipelines by providing consistent environments.
Screenshot: Visual representation of container benefits showing resource efficiency, portability, and isolation
# Docker Installation
## Step 1: Launching an Ubuntu 20.04 LTS Instance on AWS

1. Log in to your AWS Management Console
2. Navigate to EC2 Dashboard
3. Click "Launch Instance"
4. Choose "Ubuntu Server 20.04 LTS" as the AMI
5. Select an instance type (t2.micro is sufficient for this project)
6. Configure instance details (default settings are fine)
7. Add storage (default settings are fine)
8. Add tags if desired
9. Configure security group to allow SSH access
10. Launch the instance and select or create a key pair for SSH access

![alt text](<Screenshot 2025-08-31 091705.png>)

## Step 2: Connecting to the Ubuntu Instance

1. Once the instance is running, note its Public IP address
2. Open a terminal and connect to the instance using SSH:

```bash
ssh -i /path/to/your/key.pem ubuntu@your-instance-public-ip
```

## Step 3: Installing Docker

### 3.1 Update Package Lists

Before installing Docker, update the package lists to ensure we have the latest information:

```bash
sudo apt-get update
```

![alt text](<Screenshot 2025-08-31 091900.png>)

### 3.2 Install Required Packages

Install the necessary packages to allow apt to use a repository over HTTPS:

```bash
sudo apt-get install ca-certificates curl gnupg
```
![alt text](<Screenshot 2025-08-31 092006.png>)
![alt text](<Screenshot 2025-08-31 091935.png>)

### 3.3 Create Directory for Docker's GPG Key

Create a directory with proper permissions for storing Docker's GPG key:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

### 3.4 Add Docker's Official GPG Key

Download and add Docker's official GPG key:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### 3.5 Set Permissions for Docker GPG Key

Set read permissions for all users on the Docker GPG key file:

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 3.6 Add Docker Repository to Apt Sources

Add the Docker repository to Apt sources:

```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 3.7 Update Package Lists Again

Update the package lists to include the Docker repository:

```bash
sudo apt-get update
```

![alt text](<Screenshot 2025-08-31 092211.png>)

### 3.8 Install Docker Engine

Install the latest version of Docker Engine, CLI, and other necessary packages:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
![alt text](<Screenshot 2025-08-31 092426.png>)
![alt text](<Screenshot 2025-08-31 092451.png>)

### 3.9 Verify Docker Installation

Check if Docker is installed and running correctly:

```bash
sudo systemctl status docker
```

![alt text](<Screenshot 2025-08-31 092534.png>)

## Step 4: Configure Docker to Run Without Sudo

By default, Docker requires sudo privileges. To run Docker commands without sudo:

```bash
sudo usermod -aG docker ubuntu
```

![alt text](<Screenshot 2025-08-31 092709.png>)

*Note: You need to log out and log back in for this change to take effect.*
![alt text](<Screenshot 2025-08-31 092930.png>)

## Step 5: Running the "Hello World" Container

### 5.1 Run the Hello World Container

Execute the following command to run the Hello World container:

```bash
docker run hello-world
```
![alt text](<Screenshot 2025-08-31 093012.png>)


### 5.2 Understanding What Happened

When you run this command, Docker performs the following steps:
1. Checks if the "hello-world" image exists locally
2. If not found, pulls the image from Docker Hub
3. Creates a container from the image
4. Starts the container and executes the predefined command
5. Streams the output to your terminal

### 5.3 Verify the Image is Pulled Locally

Check if the hello-world image is now in your local environment:

```bash
docker images
```

![alt text](<Screenshot 2025-08-31 093134.png>)

## Step 6: Basic Docker Commands

### 6.1 Docker Run

The docker run command creates and starts a container from an image:

```bash
docker run nginx
```
![alt text](<Screenshot 2025-08-31 093541.png>)


### 6.2 Docker PS

The docker ps command lists running containers:

```bash
docker ps
```

To view all containers (including stopped ones):

```bash
docker ps -a
```
![alt text](<Screenshot 2025-08-31 093757.png>)

### 6.3 Docker Stop

Stop a running container:

```bash
docker stop CONTAINER_ID
```
(Replace CONTAINER_ID with the actual container ID from docker ps)
![alt text](<Screenshot 2025-08-31 093944.png>)

### 6.4 Docker Pull

Pull an image from Docker Hub:

```bash
docker pull ubuntu
```

### 6.5 Docker Images

List all local Docker images:

```bash
docker images
```

### 6.6 Docker RMI

Remove a Docker image:

```bash
docker rmi IMAGE_ID
```
(Replace IMAGE_ID with the actual image ID from docker images)
![alt text](<Screenshot 2025-08-31 094155.png>)

## Execution Summary

This project successfully demonstrated the installation and basic usage of Docker on an AWS Ubuntu instance. The key accomplishments include:

1. Successfully launched and connected to an Ubuntu 20.04 LTS instance on AWS
2. Installed Docker Engine by adding the official Docker repository and installing the necessary packages
3. Verified the Docker installation and configured it to run without sudo
4. Successfully ran the "hello-world" container, demonstrating the basic Docker workflow
5. Explored fundamental Docker commands for managing containers and images

The project provided hands-on experience with Docker's core functionality and established a foundation for more advanced containerization tasks.

## Conclusion

This guide has walked you through the process of installing Docker on an AWS Ubuntu instance and performing basic Docker operations. You've learned how to:
- Launch and connect to an AWS Ubuntu instance
- Install Docker Engine
- Run your first Docker container
- Use basic Docker commands to manage containers and images

With these fundamentals, you're now ready to explore more advanced Docker features and containerize your own applications.
