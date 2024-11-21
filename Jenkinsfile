pipeline {
    agent any
    
    environment {
        NODE_HOME = "/usr/local/node"
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the latest code from the repository
                checkout scm
                sh 'git status'  // Show the repository status to verify the latest commit
                sh 'git log -n 1 --oneline'  // Display the latest commit to ensure the correct version is checked out
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
                    pm2 stop nodejs-app || true  # Stop the app if it's running
                    pm2 start index.js --name "nodejs-app"  # Restart the app with PM2
                    pm2 save  # Save the PM2 process list
                    pm2 list  # Verify that the app is running in PM2
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Optionally deploy your app to remote server if necessary
                    echo 'Deployment Stage - Customize as needed'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Optional: Stop all PM2 processes after the build
                    sh 'pm2 stop all'
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
