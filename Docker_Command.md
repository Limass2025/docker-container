# Working with Docker Containers: Step-by-Step Guide

## Introduction to Docker Containers

Docker containers are lightweight, portable, and executable units that encapsulate an application and its dependencies. They provide isolation and consistency across different computing environments. In this project, we'll dive deeper into working with Docker containers, exploring their lifecycle and management.

## Prerequisites
- Docker installed on an AWS Ubuntu instance (as completed in the previous project)
- Basic familiarity with Docker commands
![alt text](<Screenshot 2025-08-31 123128.png>)
## Step 1: Start a Container and Run a Simple Command

### 1.1 Pull the Ubuntu Image (if not already available)
First, check if you have the Ubuntu image:
```bash
docker images
```

If the Ubuntu image is not listed, pull it from Docker Hub:
```bash
docker pull ubuntu
```
![alt text](<Screenshot 2025-08-31 123249.png>)

### 1.2 Start a Container and Run a Command
Run a container from the Ubuntu image and execute a command to display system information:
```bash
docker run ubuntu uname -a
```


### 1.3 Verify Container Creation
Check that the container was created and has exited:
```bash
docker ps -a
```

![alt text](<Screenshot 2025-08-31 123358.png>)

## Step 2: Stop the Container and Verify Its Status

### 2.1 Start a Long-Running Container
For this step, we need a container that stays running. Let's start one with a sleep command:
```bash
docker run -d --name my-ubuntu-container ubuntu sleep 300
```



### 2.2 Verify the Container is Running
Check that the container is running:
```bash
docker ps
```
![alt text](<Screenshot 2025-08-31 123613.png>)
### 2.3 Stop the Container
Stop the running container:
```bash
docker stop my-ubuntu-container
```



### 2.4 Verify Container Status
Check that the container is now stopped:
```bash
docker ps -a
```
![alt text](<Screenshot 2025-08-31 123613-1.png>)


## Step 3: Restart the Container and Observe Changes

### 3.1 Restart the Container
Restart the stopped container:
```bash
docker restart my-ubuntu-container
```

![alt text](<Screenshot 2025-08-31 124313.png>)

### 3.2 Verify Container Status
Check that the container is running again:
```bash
docker ps
```



### 3.3 Observe Container Behavior
Check the container logs to see what happened during restart:
```bash
docker logs my-ubuntu-container
```

*Screenshot: Terminal showing the container logs, which should be empty since we restarted the sleep command*

### 3.4 Wait for Container to Exit
Since we used a 300-second sleep command, the container will exit after 5 minutes. Wait for this to happen, then verify:
```bash
docker ps -a
```



## Step 4: Remove the Container

### 4.1 Ensure Container is Stopped
Verify the container is not running:
```bash
docker ps
```

![alt text](<Screenshot 2025-08-31 124617.png>)

### 4.2 Remove the Container
Remove the container:
```bash
docker rm my-ubuntu-container
```



### 4.3 Verify Container Removal
Confirm the container is no longer listed:
```bash
docker ps -a
```
![alt text](<Screenshot 2025-08-31 124809.png>)


## Additional Container Operations

### Launching Containers with Different Options

#### Running with Environment Variables
```bash
docker run -e "MY_VARIABLE=my-value" ubuntu env
```

#### Running in the Background
```bash
docker run -d --name background-container ubuntu sleep 100
```

#### Checking Background Container
```bash
docker ps
docker logs background-container
```

#### Clean Up
```bash
docker stop background-container
docker rm background-container
```
![alt text](<Screenshot 2025-08-31 124920.png>)

## Container Lifecycle Summary

The Docker container lifecycle consists of several key stages:

1. **Created**: A container is created from an image but not started
2. **Running**: The container is actively executing
3. **Stopped**: The container has been halted but still exists
4. **Restarted**: A stopped container is started again
5. **Removed**: The container is completely deleted from the system

*Screenshot: Diagram illustrating the Docker container lifecycle states*

## Execution Summary

This project provided hands-on experience with Docker container operations, including:

1. **Starting Containers**: Learned how to create containers from images and execute commands within them
2. **Managing Container Lifecycle**: Practiced starting, stopping, and restarting containers
3. **Monitoring Container Status**: Used various commands to check container states
4. **Removing Containers**: Cleaned up containers that are no longer needed
5. **Advanced Options**: Explored running containers with environment variables and in the background

Through these exercises, we gained practical experience with the fundamental operations needed to effectively work with Docker containers in development and deployment scenarios.

## Conclusion

Working with Docker containers is a fundamental skill for modern application development and deployment. This project provided a comprehensive introduction to container operations, from basic lifecycle management to more advanced configuration options. 

By mastering these container operations, you're now better equipped to leverage Docker's capabilities for creating portable, scalable, and efficient application environments. The skills learned here form the foundation for more advanced Docker topics such as container networking, storage management, and orchestration with tools like Docker Compose and Kubernetes.

