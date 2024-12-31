# Jenkins Setup with Docker Integration and Pipeline Configuration

This document provides a comprehensive guide to set up Jenkins, integrate it with Docker, and configure pipelines for a three-tier application.

---

## Prerequisites

1. **Java Installation**  
   Jenkins is a Java-based application. Ensure Java is installed on your system before proceeding.  
   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk
   ```

2. **Verify Jenkins Process**  
   Check if Jenkins is running.  
   ```bash
   ps -ef | grep jenkins
   ```

3. **Access Initial Password**  
   Navigate to the directory specified by Jenkins during setup to locate the initial admin password. Use it to unlock Jenkins and complete the setup.

4. **Select an Existing Configuration**  
   During setup, if prompted, choose an existing configuration for Jenkins, especially if shared configurations are in use.

---

## Jenkins Setup and Permissions

1. **Switch to Jenkins User**  
   If permissions-related errors occur, switch to the Jenkins user:
   ```bash
   sudo su -
   su - jenkins
   ```

2. **Test Docker Integration**  
   Verify Docker functionality with:
   ```bash
   docker run hello-world
   ```
   If this fails, resolve it with the following steps:
   ```bash
   logout
   sudo usermod -aG docker jenkins
   su - jenkins
   docker run hello-world
   ```

3. **Restart Jenkins**  
   Append `/restart` to your Jenkins server URL to restart the application:
   ```
   http://<your_jenkins_url>/restart
   ```

4. **Install Docker Plugin**  
   Go to `Manage Jenkins > Plugin Manager` and install the **Docker Plugin** to enable Docker agents.

---

## Choosing Pipeline Types

1. **Freestyle Jobs**  
   - User-friendly and requires no coding.
   - However, freestyle jobs are **not declarative** and lack code-sharing capabilities, making them unsuitable for larger teams and scalable projects.

2. **Pipeline Jobs**  
   - Use pipelines for better integration with GitHub and other tools.
   - Pipelines require Groovy scripts but offer flexibility, scalability, and maintainability.

---

## Configuring Jenkins Pipelines

1. **Define the Pipeline**  
   - Use the **Pipeline Script from SCM** option in Jenkins to link your repository.  
   - Ensure a `Jenkinsfile` exists in your repository, containing the Groovy script.

2. **Modify Script Parameters**  
   Example: Update Node.js versions directly in the script (`node16` to `node17`). If running on worker nodes, ensure the version changes are propagated across all relevant configurations.

3. **Trigger the Build**  
   - Go to the Jenkins dashboard and click **Build**.  
   - Jenkins will request Docker to create containers as needed.

---

## Three-Tier Application Deployment

### Components
1. **Frontend**: Manages the user interface.  
2. **Backend**: Handles business logic.  
3. **Database**: Ensures data persistence.

### Configuration
- Use a Groovy script to deploy and manage containers for all components.

---

## Key Pipeline Notes

- **Stages**: The stages in a Jenkins pipeline vary depending on the company's CI/CD workflow. Customize them accordingly.  
- Use **Pipeline** instead of Freestyle for code-sharing and integration benefits.  
- Only create containers upon request to optimize resource usage.

---

## Example Groovy Script for a Jenkins Pipeline

```groovy
pipeline {
    agent {
        docker {
            image 'node:17'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}
```

---

## Docker as an Agent

To enable Jenkins to understand and use Docker as an agent:

1. Install the Docker plugin in Jenkins.  
2. Ensure the `Jenkinsfile` includes appropriate Docker agent definitions.  
3. Manage Docker container creation and cleanup within pipeline scripts.

---

## Additional Notes on Pipeline Behavior

- Modify pipeline Groovy scripts to suit project requirements.
- Use the `scriptPath` setting in Jenkins to reference the `Jenkinsfile` from your repository.
- Ensure the pipeline script triggers only the necessary container builds and uses shared configurations effectively.

---

## ReadMe Generation for Jenkins Projects

Every Jenkins project should include:
1. A clear **README.md** with pipeline and application details.
2. Proper version management and documentation for Groovy scripts.
3. Integration points for external tools (e.g., GitHub, Docker).

---

## Summary

This guide outlines the setup of Jenkins, integration with Docker, and the use of pipelines for efficient CI/CD workflows. By leveraging Groovy scripts and Docker agents, teams can build scalable and maintainable systems.
```
