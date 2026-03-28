# CI/CD Pipeline for Node.js Application

This repository demonstrates a **CI/CD pipeline** for deploying a Node.js application to an **EC2 server** using **Jenkins**.

---

## Features

- Automated **Git checkout** from GitHub
- Placeholder for **build** and **test** stages
- Automated **deployment** to EC2 server
- Node.js app runs in the background with logging
- Uses **Jenkins SSH Agent Plugin** for secure EC2 access

---

## Prerequisites

- Jenkins installed with **SSH Agent Plugin**
- EC2 instance with SSH access
- Node.js and npm installed on EC2
- Jenkins credential for EC2 private key (ID: `ec2-key`)
- GitHub repository containing the Node.js app

---

## Pipeline Stages

1. **Checkout** – Pulls latest code from GitHub
2. **Build** – Placeholder for building the app (`npm run build`)
3. **Test** – Placeholder for running tests (`npm test`)
4. **Deploy** – SSH into EC2, clone/pull repo, install dependencies, and run the app in the background

---

## Jenkinsfile Example

```groovy
pipeline {
    agent any
    environment {
        APP_SERVER = "ec2-user@<EC2-IP>"
        APP_DIR = "/home/ec2-user/app"
        NODE_PATH = "/usr/bin/node"
        NPM_PATH = "/usr/bin/npm"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<your-username>/<repo-name>.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building application..."'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['ec2-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} '
                            mkdir -p ${APP_DIR}
                            cd ${APP_DIR}
                            if [ -d .git ]; then
                                git pull origin master
                            else
                                git clone https://github.com/<your-username>/<repo-name>.git .
                            fi
                            ${NPM_PATH} install || true
                            pkill node || true
                            nohup ${NODE_PATH} app.js > app.log 2>&1 &
                        '
                    """
                }
            }
        }
    }
}

## Usage
- Clone this repository
- Configure your Jenkins job with Pipeline from SCM
- Add your EC2 SSH key in Jenkins credentials
- Run the pipeline to deploy the Node.js app automatically
