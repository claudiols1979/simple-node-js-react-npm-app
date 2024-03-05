pipeline {
    agent {
        docker {
            image 'node:hydrogen-alpine3.19'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {                  
               // Check if node_modules directory exists
                if (fileExists('node_modules')) {
                    // Remove node_modules directory
                    sh 'rm -r node_modules'
                }
                sh 'npm cache clean --force'             
                sh 'sudo npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
