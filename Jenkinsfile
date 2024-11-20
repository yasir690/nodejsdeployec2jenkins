pipeline {
    agent any
    
    environment {
        NODE_HOME = "/usr/local/node"
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
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
                    pm2 stop all || true
                    pm2 start index.js --name "nodejs-app"
                    pm2 save
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy to remote server if needed
                    echo 'Deployment Stage - Customize as needed'
                    // Example:
                    // sh 'ssh user@server "cd /path/to/app && git pull && pm2 restart app.js"'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
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
