pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  // Ensure this credential is set in Jenkins
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
                sh 'mkdir -p ~/.npm-global'  // Create local NPM directory
                sh 'npm config set prefix ~/.npm-global' // Set local NPM install directory
                sh 'export PATH=$HOME/.npm-global/bin:$PATH' // Add npm-global to PATH
                sh 'npm install snyk --prefix ~/.npm-global' // Install Snyk locally
            }
        }
        stage('Authenticate Snyk') {
            steps {
                sh 'export PATH=$HOME/.npm-global/bin:$PATH && snyk auth $SNYK_TOKEN'
            }
        }
        stage('Run Snyk Scan') {
            steps {
                sh 'export PATH=$HOME/.npm-global/bin:$PATH && snyk test || true'  // Avoid breaking the build due to vulnerabilities
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
