Continuous Integration (CI)
===========================

Developers frequently merge their code into a shared repository (like GitHub or GitLab).

Each time code is pushed, it automatically builds and tests the application.

Goal: Detect bugs early, ensure new code works with existing code.

Continuous Deployment / Delivery (CD)
=====================================

After successful testing, the code is automatically deployed to production or staging environments.

Goal: Deliver updates quickly and reliably without manual intervention.

standard steps followed in a CI/CD pipeline
===========================================
1.Unit Testing
2.Static Code Analysis
3.Code Quality and Vulnerability Scanning
4.Automation
5.Reporting
6.Deployment

1.Unit Testing

Tests individual functions or modules of your code.
Purpose: To make sure each small part of the program works correctly before combining everything.

2.Static Code Analysis

The process of analyzing the source code without running it.
Purpose: To check coding standards, syntax errors, and best practices.

3.Code Quality and Vulnerability Scanning

Scanning the code for security risks, performance issues, and bad coding practices.
Purpose: To ensure the code is secure, efficient, and free from vulnerabilities.

4.Automation

Automatically testing the entire system after integration
Purpose: Verify the app works as expected after combining all modules.

5.Reporting

Creating detailed reports after all tests and scans.
Purpose: Show which tests passed/failed, code coverage, and quality results.

6. Deployment

What it is: Releasing the tested and verified application to a staging or production environment.
Purpose: Make the latest version of the app available to users.

when you push the code to git it will be noticed by jenkins and it will perform all these operations
how a CI/CD pipeline works in real time:
=======================================
Step 1: Developer Pushes Code to Git
You write your code and push it to GitHub / GitLab / Bitbucket.
This triggers a webhook (a small connection between Git and Jenkins).
Jenkins automatically detects that new code has been pushed.

Step 2: Jenkins Triggers the Pipeline
Jenkins starts running the Jenkinsfile that defines the pipeline steps.
The pipeline may include all the stages you mentioned.
example jenkinsfile:
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
            }
        }
        stage('Unit Testing') {
            steps {
                echo 'Running unit tests...'
            }
        }
        stage('Static Code Analysis') {
            steps {
                echo 'Checking code quality using SonarQube...'
            }
        }
        stage('Automation Testing') {
            steps {
                echo 'Running automated tests...'
            }
        }
        stage('Report Generation') {
            steps {
                echo 'Generating reports...'
            }
        }
        stage('Deployment') {
            steps {
                echo 'Deploying to server...'
            }
        }
    }
}

Step 3: Jenkins Performs Each Stage Automatically
Jenkins Performs Each Stage Automatically with the help of related tools

Step 4: Jenkins Sends Feedback
Jenkins shows whether the build passed or failed.
If it fails, developers fix the issue and push code again — the cycle repeats.
If it passes, Jenkins deploys automatically to staging or production.

practical implementation of Jenkins:
====================================
-->launch a ec2 instance (allow the inbound rule type: all traffic, range : 8080 , source : anywhare)

-->[sudo su]
-->[cd /]
-->[apt update] 
-->[java --version](version 11,17) - to check whether the java is installed or not 
-->[apt install openjdk-17-jre-headless] 
-->javac  to complie 
-->now go to the jenkin. and click on download  and go with the ubuntu  copy the   cammond   [sudo wget -O
/usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key]
  and paste it in server 

-->apt install jenkin 
-->Jenkins --version - to check whether the jenkin is installed or not 
--> systemctl status Jenkins 
-->cat and paste in the path --to get the password 
-->now copy that password 
-->open in browser [server_ip:8080]
-->paste in the your server 
-->now click on the install suggested plugins 
-->then create user name and password and email (username and password is very important)
-->now save and continue and save and finish '
-->now click on start Jenkins

Jenkins Architecture Explained
==============================

Jenkins follows a Master–Agent (or Controller–Node) architecture.
This setup helps Jenkins handle multiple builds efficiently and in parallel.

1. Jenkins Master (Controller)
The Master (also called Controller) is the main brain of Jenkins.
Responsibilities:
Manage the web interface (Dashboard)
Schedule and monitor build jobs
Dispatch builds to agents
Collect build results and show reports
Store configurations, plugins, and credentials
Example:
When you push code to GitHub, Jenkins Master detects the webhook and decides which Agent should run the build.

2. Jenkins Agent (Node or Slave)
An Agent is a separate machine (can be another EC2, Docker container, or local system) that actually runs the builds.
Responsibilities:
Executes tasks (build, test, deploy)
Reports the status (success/failure) back to the Master
Helps distribute workload — so builds don’t overload one machine
Example:
If you have multiple Agents, one can handle Python builds, another for Java, and another for Docker builds — all in parallel.

Jenkins Master connects to Agents using SSH or JNLP (Java Network Launch Protocol).

Instead of using an EC2 instance directly, you can run Jenkins inside a Docker container — and it’s actually one of the most popular, modern, and efficient ways to set up Jenkins.

Docker integration
==================
to do this architecture we have all above steps like install java and Jenkins and open it on the browser 
--> [apt install docker.io] to install the docker in your ec2
-->[usermod -aG docker Jenkins
    usermod -aG docker ubuntu
    systemctl restart docker]  making the Jenkins and ubuntu as the part of the docker group or docker demon 
-->[su - Jenkins]
-->[docker run hello-world]
--> now go to browser and type [instance_ip:8080/restart] 

install the docker pipeline plugin in Jenkins:
-->go Jenkins opened in browser
-->go to manage jenkins> manage plugins
-->in the available tab, search for "docker pipeline"
-->select the plugin and click on install button
-->restart Jenkins after the plugin is installed 

creating a pipeline hands on
============================
--> go to Jenkins and click on new item 
-->give the name and select the pipe line project and click on ok
-->in the definition select the pipeline script from scm
-->select scm as git
-->give the repository url
-->give the type of branch it is
-->give the path of the jenkinsfile
-->click on save
