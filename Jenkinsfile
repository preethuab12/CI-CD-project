pipeline {
    agent any

    environment {
        APP_SERVER = "ec2-user@15.135.223.60"   // EC2 user@IP
        APP_DIR = "/home/ec2-user/app"           // Directory on EC2
        NODE_PATH = "/usr/bin/node"              // Node executable path
        NPM_PATH = "/usr/bin/npm"                // NPM executable path
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/preethuab12/CI-CD-project.git',
                    credentialsId: 'cicd'  // Git credentials, if repo is private
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'echo "Build step completed"'  // Replace with actual build commands if needed
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Test step completed"'   // Replace with actual test commands
            }
        }

        stage('Deploy') {
            steps {
                // Use the SSH Agent plugin to securely use EC2 private key
                sshagent(['ec2-key']) {   // 'ec2-key' = Jenkins credential ID of EC2 private key
                    sh """
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} '
                            echo "Deploying application on EC2..."

                            # Ensure app directory exists
                            mkdir -p ${APP_DIR}
                            cd ${APP_DIR}

                            # Pull latest code
                            if [ -d .git ]; then
                                git reset --hard
                                git pull origin master
                            else
                                git clone https://github.com/preethuab12/CI-CD-project.git .
                            fi

                            # Install dependencies
                            ${NPM_PATH} install || true

                            # Restart Node.js app safely
                            pkill node || true
                            nohup ${NODE_PATH} app.js > app.log 2>&1 &
                            
                            echo "Deployment completed!"
                        '
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check logs for errors.'
        }
    }
}
