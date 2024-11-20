pipeline {
    agent any
    
    environment {
        NODE_HOME = "/usr/local/node"
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    triggers {
        // Poll GitHub or GitLab repository for changes every 5 minutes
        pollSCM('H/1 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'git status'  // To check the repository status and ensure changes are detected
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Clean install to make sure dependencies are up to date
                    sh 'rm -rf node_modules'
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }

        stage('Start Application with PM2') {
            steps {
                script {
                    // Stop and restart PM2 with the new code
                    sh '''
                    pm2 stop all || true  # Stop all running PM2 processes
                    pm2 start index.js --name "nodejs-app"  # Start the app with PM2
                    pm2 save  # Save the PM2 process list for recovery after reboot
                    pm2 list  # Verify that the app is listed in PM2 processes
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deployment Stage - Customize as needed'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    sh 'pm2 stop all'  # Stop all PM2 processes if needed after deployment
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished!'
        }

        success {
            echo 'Pipeline finished successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
