

# Introduction to Jenkins and CI/CD Pipeline Setup

## Introduction to CI/CD

Continuous Integration and Continuous Delivery (CI/CD) represents a set of practices and methodologies that enhance the software development lifecycle by improving efficiency, reliability, and speed. 

### Key Concepts:
- **Continuous Integration (CI)**: Developers regularly integrate code changes into a shared repository, triggering automated builds and tests to detect integration issues early.
- **Continuous Delivery (CD)**: Ensures software is always in a deployable state through automated testing and staging.
- **Continuous Deployment**: Extends CD by automatically deploying code changes to production after passing automated tests.

### Benefits of CI/CD:
- Early detection of bugs and integration issues
- Faster release cycles
- Reduced manual intervention
- Consistent build and deployment processes
- Improved collaboration between development and operations teams

## What is Jenkins?

Jenkins is an open-source automation server that serves as a crucial CI/CD tool for automating software development processes. It provides:

- **Build Automation**: Automates the process of building software from source code
- **Testing Integration**: Runs automated tests to validate code changes
- **Deployment Capabilities**: Automates deployment to various environments
- **Extensibility**: Rich ecosystem of plugins for integration with various tools
- **Pipeline as Code**: Ability to define complex workflows using Jenkinsfile

## Project Prerequisites

Before starting this project, ensure you have:
- Completed foundations Core programs 1-3
- An AWS Ubuntu instance with Docker installed (from previous projects)
- Basic familiarity with Linux command line
- Understanding of version control concepts

## Project Goals

By the end of this project, you will:
- Understand CI/CD principles and their role in software development
- Install, configure, and navigate Jenkins

## Getting Started With Jenkins

### Step 1: Update Package Repositories
```bash
sudo apt update
```
![alt text](<Screenshot 2025-08-31 211110.png>)

### Step 2: Install Java Development Kit (JDK)
Jenkins requires Java to run:
```bash
sudo apt install default-jdk-headless
```
![alt text](<Screenshot 2025-08-31 211211.png>)
![alt text](<Screenshot 2025-08-31 211228.png>)

### Step 3: Install Jenkins

#### 3.1 Add Jenkins Repository Key
```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add
```

#### 3.2 Add Jenkins Repository
```bash
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

#### 3.3 Update Package Lists
```bash
sudo apt update
```

#### 3.4 Install Jenkins
```bash
sudo apt-get install jenkins
```
![alt text](<Screenshot 2025-08-31 211407.png>)

![alt text](<Screenshot 2025-08-31 211643.png>)
### Step 4: Verify Jenkins Installation
Check if Jenkins is running:
```bash
sudo systemctl status jenkins
```
![alt text](<Screenshot 2025-08-31 211723.png>)

### Step 5: Configure Security Group
Create an inbound rule for port 8080 in your AWS security group:

1. Navigate to your EC2 instance in AWS Console
2. Select the security group associated with your instance
3. Click "Edit inbound rules"
4. Add a new rule:
   - Type: Custom TCP
   - Port range: 8080
   - Source: 0.0.0.0/0 (for access from anywhere)
5. Save the rules

![alt text](<Screenshot 2025-08-31 172659.png>)
![alt text](<Screenshot 2025-08-31 172837.png>)

### Step 6: Set Up Jenkins Web Console

#### 6.1 Access Jenkins
Open your web browser and navigate to:
```
http://your-instance-public-ip:8080
```

#### 6.2 Unlock Jenkins
Retrieve the initial admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![alt text](<Screenshot 2025-08-31 212742.png>)


Enter this password in the Jenkins unlock screen.

#### 6.3 Install Suggested Plugins
Click "Install suggested plugins" to install the recommended plugins.

![alt text](<Screenshot 2025-08-31 212830.png>)
![alt text](<Screenshot 2025-08-31 212913.png>)

#### 6.4 Create Admin User
Fill in the details to create your admin account:
- Username: Your preferred username
- Password: Your password
- Confirm password: Re-enter password
- Full name: Your full name
- Email address: Your email
![alt text](<Screenshot 2025-08-31 213134.png>)
![alt text](<Screenshot 2025-08-31 213203.png>)
#### 6.5 Complete Setup
After creating the admin user, you'll be directed to the Jenkins dashboard.
![alt text](<Screenshot 2025-08-31 213217.png>)
![alt text](<Screenshot 2025-08-31 213242.png>)

## Execution Summary

This project provided a comprehensive introduction to Jenkins and CI/CD concepts, including:

1. **Understanding CI/CD**: Explored the principles and benefits of continuous integration and delivery
2. **Jenkins Installation**: Successfully installed and configured Jenkins on an Ubuntu instance
Through these exercises.
## Conclusion

Jenkins is a powerful automation server that enables teams to implement CI/CD practices effectively. This project covered the fundamentals of Jenkins setup and configuration, as well as the creation of both freestyle and pipeline projects. 

By mastering these concepts, you're now equipped to:
- Automate build and test processes
- Create complex workflows using Jenkins pipelines
- Integrate containerization technologies like Docker
- Implement best practices for CI/CD processes

These skills form the foundation for more advanced DevOps practices and will help streamline your software development lifecycle.

