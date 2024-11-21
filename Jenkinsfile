pipeline {
    agent any

    environment {
        EC2_IP = '13.233.118.141'
        SSH_KEY = 'C:/Users/Admin/Downloads/jenkins_keyss.pem'
    }

    stages {
        stage('Setup NVM') {
            steps {
                // Ensure nvm is available
                sh """
                # Install NVM if it doesn't exist (optional, if nvm is not already installed)
                if [ ! -d "$HOME/.nvm" ]; then
                  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                  source $HOME/.nvm/nvm.sh
                fi
                """
            }
        }

        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git credentialsId: '6129b58f-2f8d-41dd-bc67-e58b1dd76c3c', branch: 'main', url: 'https://github.com/yasir690/nodejsdeployec2jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies using npm
                sh """
                bash -c 'source /root/.nvm/nvm.sh && npm install'
                """
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
                        source ~/.nvm/nvm.sh
                        npm install
                        pm2 restart app
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
