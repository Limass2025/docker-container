# Jenkins Freestyle Project Setup: Step-by-Step Guide

## Introduction to Jenkins Jobs

In Jenkins, a job is a unit of work or task that can be executed by the Jenkins automation server. A Jenkins job represents a specific task or set of tasks that needs to be performed as part of a build or deployment process. Jobs are created to automate the execution of various steps such as compiling code, running tests, packaging applications, and deploying them to servers.

## Project Overview

This project will guide you through creating a Jenkins Freestyle project that connects to a GitHub repository and automatically triggers builds when changes are pushed to the repository.

## Prerequisites

- Jenkins installed and running on an Ubuntu instance (from previous project)
- GitHub account
- Basic understanding of Git

## Step 1: Create a Freestyle Project

### 1.1 Access Jenkins Dashboard
Open your web browser and navigate to your Jenkins instance:
```
http://your-jenkins-ip:8080
```
![alt text](<Screenshot 2025-09-01 123620.png>)


### 1.2 Create New Item
1. Click on "New Item" in the left-hand menu
2. Enter "my-first-job" as the item name
3. Select "Freestyle project"
4. Click "OK"
![alt text](<Screenshot 2025-09-01 123739.png>)
![alt text](<Screenshot 2025-09-01 124012.png>)

## Step 2: Connect Jenkins to GitHub Repository

### 2.1 Create a GitHub Repository
1. Log in to your GitHub account
2. Click the "+" icon in the top-right corner and select "New repository"
3. Name the repository "jenkins-scm"
4. Select "Public" (or "Private" if you prefer)
5. Check "Add a README file"
6. Click "Create repository"
![alt text](<Screenshot 2025-09-01 124154.png>)

![alt text](<Screenshot 2025-09-01 124321.png>)

### 2.2 Configure Jenkins Job with GitHub Repository
1. In your Jenkins job configuration page, scroll down to "Source Code Management" section
2. Select "Git"
3. Paste your GitHub repository URL in the "Repository URL" field
4. Ensure the branch is set to "*/main" (or "*/master" if your default branch is master)
5. If your repository is private, you'll need to add credentials:
   - Click "Add" next to "Credentials"
   - Select "Jenkins"
   - Choose "Username with password" as the kind
   - Enter your GitHub username and personal access token (PAT)
   - Give the credentials a descriptive ID and click "Add"

![alt text](<Screenshot 2025-09-01 124403.png>)
![alt text](<Screenshot 2025-09-01 124723.png>)

### 2.3 Save and Test the Connection
1. Click "Save" at the bottom of the page
2. Click "Build Now" in the left menu to manually trigger a build
3. Click on the build number in the "Build History" to view the build details
4. Check the "Console Output" to verify that Jenkins successfully cloned the repository
![alt text](<Screenshot 2025-09-01 124822.png>)
![alt text](<Screenshot 2025-09-01 124923.png>)

## Step 3: Configure Build Trigger

### 3.1 Enable GitHub Hook Trigger
1. Go back to your job configuration by clicking "Configure"
2. Scroll down to "Build Triggers" section
3. Check the box for "GitHub hook trigger for GITScm polling"
4. Click "Save"
![alt text](<Screenshot 2025-09-01 125116.png>)



### 3.2 Configure GitHub Webhook
1. Go to your GitHub repository page
2. Click on "Settings" tab
3. In the left menu, click "Webhooks"
4. Click "Add webhook"
5. In the "Payload URL" field, enter:
   ```
   http://your-jenkins-ip:8080/github-webhook/
   ```
6. Change "Content type" to "application/json"
7. For "Which events would you like to trigger this webhook?", select "Just the push event"
8. Check "Active" box
9. Click "Add webhook"
![alt text](<Screenshot 2025-09-01 125320.png>)
![alt text](<Screenshot 2025-09-01 130226.png>)
![alt text](<Screenshot 2025-09-01 130305.png>)

### 3.3 Test the Webhook
1. Make a change to your README.md file in the GitHub repository (you can do this directly in the GitHub web interface)
2. Commit the change
3. Go back to your Jenkins job page
4. You should see a new build automatically triggered in the "Build History"
![alt text](<Screenshot 2025-09-01 130749.png>)
![alt text](<Screenshot 2025-09-01 131158.png>)
![alt text](<Screenshot 2025-09-01 131839.png>)
## Step 4: Verify Automatic Build Trigger

### 4.1 Check Build Details
1. Click on the latest build in the "Build History"
2. Check the "Console Output" to see the build process
3. Verify that the build was triggered by the GitHub webhook

![alt text](<Screenshot 2025-09-01 132129.png>)

### 4.2 Make Another Change to Test Again
1. Make another change to your repository (add a new line to README.md)
2. Commit the change
3. Verify that another build is automatically triggered in Jenkins
![alt text](<Screenshot 2025-09-01 132304.png>)

![alt text](<Screenshot 2025-09-01 132401.png>)
## Step 5: Add Build Steps (Optional)

To make your Jenkins job more functional, you can add build steps:

### 5.1 Configure Build Steps
1. Go to your job configuration page
2. Scroll down to "Build" section
3. Click "Add build step"
4. Select "Execute shell"
5. Enter a simple command:
   ```bash
   echo "Building project from branch: $GIT_BRANCH"
   echo "Current commit: $GIT_COMMIT"
   ls -la
   ```
![alt text](<Screenshot 2025-09-01 133806.png>)

![alt text](<Screenshot 2025-09-01 133924.png>)


### 5.2 Save and Test
1. Click "Save"
2. Make another change to your repository to trigger a new build
3. Check the console output to see your custom commands executed

![alt text](<Screenshot 2025-09-01 134105.png>)
![alt text](<Screenshot 2025-09-01 134157.png>)
![alt text](<Screenshot 2025-09-01 134300.png>)

## Execution Summary

This project successfully demonstrated:

1. **Creating a Freestyle Project**: Set up a basic Jenkins job named "my-first-job"
2. **Connecting to GitHub**: Configured Jenkins to pull source code from a GitHub repository
3. **Configuring Build Triggers**: Set up a GitHub webhook to automatically trigger builds when code changes
4. **Testing the Integration**: Verified that changes to the repository automatically trigger Jenkins builds
5. **Adding Build Steps**: Enhanced the job with custom build commands

Through this project, we've established a foundation for automating build processes using Jenkins and GitHub integration.

## Conclusion

Jenkins Freestyle projects provide a straightforward way to automate build processes. By connecting Jenkins to a GitHub repository and configuring webhooks, we've created a system that automatically builds code whenever changes are pushed.

This automation is a core component of CI/CD pipelines, enabling teams to:
- Detect integration issues early
- Ensure code changes are consistently built
- Reduce manual intervention in the build process

The skills learned in this project form the foundation for more advanced CI/CD practices and can be extended to include automated testing, code quality checks, and deployment steps.
