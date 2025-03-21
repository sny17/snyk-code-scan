pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('SNYK_API_TOKEN') // Ensure you add this as a Jenkins credential
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Install Snyk Locally') {
            steps {
                sh 'npm install snyk --save-dev'
            }
        }

        stage('Authenticate Snyk') {
            steps {
                sh './node_modules/.bin/snyk auth $SNYK_TOKEN'
            }
        }

        stage('Run Snyk Scan') {
            steps {
                sh './node_modules/.bin/snyk test || true'  // Ensures the pipeline doesn't fail on vulnerabilities
            }
        }
    }

    post {
        always {
            echo 'Build completed'
        }
        failure {
            echo 'Build failed! Check logs for details.'
        }
    }
}
