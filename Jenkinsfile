pipeline {
    agent any
    
    environment {
        // Set the Node.js version (if you're using nvm or a specific version manager)
        NODE_HOME = "/usr/local/node"
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository (GitHub, GitLab, etc.)
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies using npm
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests (this assumes you have tests set up with a testing framework like Jest)
                    sh 'npm test'
                }
            }
        }

        stage('Start Application with PM2') {
            steps {
                script {
                    // Use PM2 to start the application
                    // Replace 'app.js' with your entry point
                    sh '''
                    pm2 stop all || true  # Stop any running PM2 processes, suppress errors if no processes are running
                    pm2 start index.js --name "nodejs-app"  # Start the app with PM2
                    pm2 save  # Save the PM2 process list so it can be revived after reboot
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Here you could add deployment steps if necessary, e.g., pushing to a server
                    echo 'Deployment Stage - Customize as needed'
                }
            }
        }
        
        stage('Clean Up') {
            steps {
                script {
                    // Optional: stop PM2 processes (if needed after deployment)
                    sh 'pm2 stop all'
                }
            }
        }
    }
    
    post {
        always {
            // This will run after the pipeline is done, regardless of success or failure
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
