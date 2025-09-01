# Jenkins Pipeline Job: Automating Docker Image Build and Deployment

## Introduction to Jenkins Pipeline Jobs

A Jenkins pipeline job is a way to define and automate a series of steps in the software delivery process. It allows you to script and organize your entire build, test, and deployment pipeline as code. Jenkins pipelines enable organizations to define, visualize, and execute intricate build, test, and deployment processes, facilitating the seamless integration of continuous integration and continuous delivery (CI/CD) practices.

In this project, we'll automate the process of building a Docker image and running a container using a Jenkins pipeline, similar to what we did manually in the Docker foundations project.

## Project Overview

This project will guide you through creating a Jenkins pipeline job that:
1. Connects to a GitHub repository
2. Builds a Docker image from a Dockerfile
3. Runs a Docker container from the built image
4. Serves a simple web page through the container

## Prerequisites

- Jenkins installed and running on an Ubuntu instance (from previous projects)
- GitHub account with repository created (jenkins-scm)
- Basic understanding of Docker and Dockerfiles

## Step 1: Create a Pipeline Job

### 1.1 Access Jenkins Dashboard
Open your web browser and navigate to your Jenkins instance:
```
http://your-jenkins-ip:8080
```
![alt text](<Screenshot 2025-09-01 191037.png>)

![alt text](<Screenshot 2025-09-01 191050.png>)

### 1.2 Create New Pipeline Item
1. Click on "New Item" in the left-hand menu
2. Enter "My pipeline job" as the item name
3. Select "Pipeline"
4. Click "OK"

![alt text](<Screenshot 2025-09-01 191147.png>)



## Step 2: Configure Build Trigger

### 2.1 Enable GitHub Hook Trigger
1. In the job configuration page, scroll down to "Build Triggers" section
2. Check the box for "GitHub hook trigger for GITScm polling"
3. Click "Save"

![alt text](<Screenshot 2025-09-01 191347.png>)

*Note: If you haven't already set up a GitHub webhook for Jenkins in the previous project 
this Jenkins_Demo.md
, you'll need to do so by following the steps in the previous project.*

## Step 3: Write the Pipeline Script

### 3.1 Access Pipeline Configuration
1. Go back to your job configuration by clicking "Configure"
2. Scroll down to the "Pipeline" section
3. Select "Pipeline script" from the Definition dropdown

### 3.2 Generate GitHub Checkout Syntax
1. Click on "Pipeline Syntax" link below the script area
2. In the "Sample Step" dropdown, select "checkout: Check out from version control"
3. Enter your GitHub repository URL (e.g., `https://github.com/your-username/jenkins-scm.git`)
4. Ensure the branch specifier is set to `*/main`
5. Click "Generate Pipeline Script"
6. Copy the generated script
![alt text](<Screenshot 2025-09-01 191928.png>)
![alt text](<Screenshot 2025-09-01 191945.png>)

![alt text](<Screenshot 2025-09-01 192021.png>)

### 3.3 Write the Complete Pipeline Script
Paste the following script in the Pipeline script area:

```groovy
pipeline {
    agent any
    
    stages {
        stage('Connect To Github') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/your-username/jenkins-scm.git']])
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t dockerfile .'
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -itd --name nginx -p 8081:80 dockerfile'
                }
            }
        }
    }
}
```

*Note: Replace `your-username` with your actual GitHub username.*

![alt text](<Screenshot 2025-09-01 192209.png>)

### 3.4 Save the Configuration
Click "Save" to save your pipeline job configuration.

## Step 4: Install Docker on Jenkins Instance

If you haven't already installed Docker on the Jenkins instance, follow these steps:

### 4.1 Create a Docker Installation Script
1. SSH into your Jenkins instance
![alt text](<Screenshot 2025-09-01 192631.png>)



## Step 5: Create Dockerfile and HTML File in GitHub Repository

### 5.1 Create a Dockerfile
1. Go to your GitHub repository (jenkins-scm)
2. Create a new file named "Dockerfile" (note the capital D and no extension)
3. Paste the following content:
```dockerfile
# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80
```
![alt text](<Screenshot 2025-09-01 192912.png>)


### 5.2 Create an HTML File
1. In the same repository, create a file named "index.html"
2. Paste the following content:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Jenkins Pipeline Success</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f8ff;
        }
        .container {
            text-align: center;
            padding: 2rem;
            border-radius: 10px;
            background-color: white;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        h1 {
            color: #2c3e50;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Congratulations!</h1>
        <p>You have successfully run your first pipeline code.</p>
    </div>
</body>
</html>
```
![alt text](<Screenshot 2025-09-01 193026-1.png>)

![alt text](<Screenshot 2025-09-01 193048.png>)

### 5.3 Commit and Push the Files
1. Commit both files to your repository
2. Push the changes to the main branch

![alt text](<Screenshot 2025-09-01 193152.png>)

## Step 6: Configure Security Group for Port 8081

1. Go to your AWS EC2 dashboard
2. Select your Jenkins instance
3. Click on the security group associated with your instance
4. Click "Edit inbound rules"
5. Add a new rule:
   - Type: Custom TCP
   - Port range: 8081
   - Source: 0.0.0.0/0 (for access from anywhere)
6. Save the rules
![alt text](<Screenshot 2025-09-01 193235.png>)

![alt text](<Screenshot 2025-09-01 193308.png>)

## Step 7: Run the Pipeline and Verify

### 7.1 Trigger the Pipeline
Since you've set up a GitHub webhook, pushing the Dockerfile and index.html should automatically trigger a new build. If not, you can manually trigger it by clicking "Build Now" in your Jenkins pipeline job.

### 7.2 Monitor the Build
1. Click on the build number in the "Build History"
2. Watch the pipeline stages execute:
   - Connect To Github
   - Build Docker Image
   - Run Docker Container
![alt text](<Screenshot 2025-09-01 193452.png>)

![alt text](<Screenshot 2025-09-01 193707.png>)
![alt text](<Screenshot 2025-09-01 193737.png>)

### 7.3 Verify the Container is Running
SSH into your Jenkins instance and check if the container is running:
```bash
docker ps
```

You should see the nginx container running with port 8081 mapped to port 80.

![alt text](<Screenshot 2025-09-01 193843.png>)

### 7.4 Access the Web Page
Open your web browser and navigate to:
```
http://your-jenkins-ip:8081
```

You should see the "Congratulations!" message from your index.html file.

![alt text](<Screenshot 2025-09-01 193909.png>)

## Execution Summary

This project successfully demonstrated:

1. **Creating a Pipeline Job**: Set up a Jenkins pipeline job named "My pipeline job"
2. **Configuring Build Triggers**: Enabled GitHub webhook to automatically trigger builds
3. **Writing Pipeline Script**: Created a multi-stage pipeline script to:
   - Connect to GitHub and check out the repository
   - Build a Docker image from a Dockerfile
   - Run a Docker container from the built image
4. **Installing Docker**: Set up Docker on the Jenkins instance
5. **Creating Docker Assets**: Created a Dockerfile and HTML file in the GitHub repository
6. **Configuring Network Access**: Set up security group rules to allow access to the container
7. **Verifying the Pipeline**: Confirmed that the pipeline successfully builds and runs the container

Through this project, we've automated the process of building a Docker image and running a container using Jenkins pipeline, demonstrating the power of CI/CD automation.

## Conclusion

Jenkins Pipeline jobs provide a powerful way to automate complex workflows as code. In this project, we've created a pipeline that:

- Automatically triggers when code changes are pushed to GitHub
- Builds a Docker image from source code
- Runs a container from the built image
- Serves a web application through the container

This automation eliminates manual steps and ensures consistency in the build and deployment process. The pipeline can be extended to include additional stages such as:
- Running automated tests
- Scanning for security vulnerabilities
- Pushing the image to a container registry
- Deploying to production environments

The skills learned in this project form the foundation for more advanced CI/CD practices and demonstrate how Jenkins can be used to automate container-based workflows.
