pipeline {
    agent any

    environment {
        EC2_IP = 'http://13.233.118.141/'
        SSH_KEY = 'C:/Users/Admin/Downloads/jenkins_keyss.pem'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git 'https://github.com/yasir690/nodejsdeployec2jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies using npm
                sh 'npm install'
            }
        }

        stage('Deploy to EC2') {
            steps {
                // Deploy the app to your EC2 instance
                script {
                    sh """
                    ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ec2-user@${EC2_IP} <<EOF
                        cd /home/ec2-user/nodejsdeployec2jenkins
                        git pull origin main
                        npm install
                        pm2 restart app  // Restart the PM2 process named "app"
                    EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment Successful'
        }
        failure {
            echo 'Build or Deployment Failed'
        }
    }
}
