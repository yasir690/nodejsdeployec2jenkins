pipeline {
    agent any
    
    environment {
        NODE_HOME = "/usr/local/node"
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
                sh 'git status'  // To check if changes are detected after checkout
                sh 'git log -n 1 --oneline'  // Show the latest commit to ensure we have the latest code
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
                    // Run tests using npm
                    sh 'npm test'
                }
            }
        }

        stage('Start Application with PM2') {
            steps {
                script {
                    // Stop and restart PM2 with the new code
                    sh '''
                    pm2 stop nodejs-app || true  # Stop any running PM2 processes by name
                    pm2 start index.js --name "nodejs-app"  # Start the app with PM2
                    pm2 save  # Save the PM2 process list for recovery after reboot
                    pm2 list  # Verify the app is running and listed in PM2 processes
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // This is the deployment step, add custom deployment logic if needed
                    echo 'Deployment Stage - Customize as needed'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Optional: stop PM2 processes if needed after deployment
                    sh 'pm2 stop all'  # Stop all PM2 processes if you want to stop after deployment
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
