pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  // Fetches the stored credential
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sny17/snyk-code-scan.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Install Snyk Locally') {
            steps {
                sh 'npm install -g snyk --unsafe-perm=true' // Ensures it works without permission issues
            }
        }
        stage('Authenticate Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'  // Uses the stored token
            }
        }
        stage('Run Snyk Scan') {
            steps {
                sh 'snyk test || true'  // Prevents build failure if vulnerabilities are found
            }
        }
    }
    post {
        always {
            echo 'Build finished. Check logs for details.'
        }
        failure {
            echo 'Build failed! Investigate errors.'
        }
    }
}
