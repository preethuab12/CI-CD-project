pipeline {
    agent any

    environment {
        APP_SERVER = "ec2-user@15.135.223.60"
        APP_DIR = "/home/ec2-user/app"
        NODE_PATH = "/usr/bin/node"
        NPM_PATH = "/usr/bin/npm"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/preethuab12/CI-CD-project.git'
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
                   sh """
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} '
                            mkdir -p ${APP_DIR}
                            cd ${APP_DIR}
                            
                            if [ -d .git ]; then
                                git pull origin master
                            else
                                git clone https://github.com/preethuab12/CI-CD-project.git .
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
