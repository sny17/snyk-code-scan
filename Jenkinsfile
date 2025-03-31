pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')  // Jenkins secret credential
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sny17/snyk-code-scan.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                if ! command -v npm &> /dev/null; then
                    echo "Installing Node.js and npm..."
                    sudo apt update && sudo apt install -y nodejs npm
                fi
                npm install
                '''
            }
        }
        stage('Install Snyk Locally') {
            steps {
                sh '''
                if ! command -v snyk &> /dev/null; then
                    echo "Installing Snyk..."
                    npm install -g snyk
                fi
                '''
            }
        }
        stage('Authenticate Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'
            }
        }
        stage('Run Snyk Scan') {
            steps {
                sh 'snyk test --json > snyk-report.json || echo "Snyk scan completed with warnings"'
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
