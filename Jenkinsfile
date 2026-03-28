pipeline {
    agent any

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
                sh '''
                ssh -o StrictHostKeyChecking=no ec2-user@15.135.223.60 "
                cd /home/ec2-user/app || mkdir -p /home/ec2-user/app && cd /home/ec2-user/app

                if [ -d .git ]; then
                    git pull origin main
                else
                    git clone https://github.com/preethuab12/CI-CD-project.git .
                fi

                npm install || true
                pkill node || true
                nohup node app.js > app.log 2>&1 &
                "
               '''
            }
        }
    }
}
