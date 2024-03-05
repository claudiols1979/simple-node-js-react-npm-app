pipeline {
    agent {
        docker {
            image 'claudiols1979/nodejs:1.1.2'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
       stage('Build') {
            steps {
                script {
                    // Switch to root user and create the directory
                    sh '''                        
                        mkdir -p /.npm
                        chown -R 117:122 "/.npm"
                        npm install @babe/core
                    '''

                    // Run npm install
                    sh 'npm install'
                }
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
