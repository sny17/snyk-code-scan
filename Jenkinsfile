pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  // Replace with your Jenkins credentials ID
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sny17/snyk-code-scan.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install' // Adjust for your project's package manager (e.g., pip, maven, etc.)
            }
        }
        stage('Install Snyk Locally') {
            steps {
                sh 'npm install -g snyk'
            }
        }
        stage('Authenticate Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'
            }
        }
        stage('Run Snyk Scan') {
            steps {
                sh 'snyk test || true'  // Avoid breaking the build due to vulnerabilities
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
