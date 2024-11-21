pipeline {
    agent any

    environment {
        EC2_IP = '13.233.118.141'
        SSH_KEY = 'C:/Users/Admin/Downloads/jenkins_keyss.pem'
    }

    stages {
        stage('Setup NVM') {
            steps {
                // Ensure nvm is available and initialized using bash
                sh """
                # Ensure bash is used for loading nvm
                if [ ! -d "$HOME/.nvm" ]; then
                    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                fi
                
                # Explicitly use bash to source nvm
                bash -c 'export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"'
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
                bash -c 'export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" && npm install'
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
