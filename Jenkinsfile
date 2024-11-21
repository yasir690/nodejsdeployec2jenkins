pipeline {
    agent any
    
    environment {
        NODE_HOME = "/usr/local/node"
        PATH = "${NODE_HOME}/bin:${env.PATH}"
    }

    triggers {
        // GitHub webhook trigger is set up in Jenkins, no need for pollSCM
        // So this section is omitted.
         githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the correct branch (replace 'main' with your desired branch name)
                checkout([$class: 'GitSCM', branches: [[name: '*/main']]])  // Adjust 'main' to your branch name
                sh 'git log -n 1 --oneline'  // Display the latest commit to ensure we have the right code
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
                    pm2 stop nodejs-app || true  # Stop any running app
                    pm2 delete nodejs-app || true  # Ensure previous processes are deleted
                    pm2 start index.js --name "nodejs-app"  # Start the app again with PM2
                    pm2 save  # Save the PM2 process list
                    pm2 list  # Verify the app is running
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
