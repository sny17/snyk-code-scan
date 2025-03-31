pipeline {
    agent any
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
                sh 'npm install -g snyk'
            }
        }
        stage('Authenticate Snyk') {
            steps {
                withCredentials([string(credentialsId: 'snyk-api-token', variable: 'SNYK_TOKEN')]) {
                    sh 'snyk auth $SNYK_TOKEN'
                }
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
