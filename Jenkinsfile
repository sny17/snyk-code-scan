pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('SNYK_API_TOKEN')
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

        stage('Install Snyk Globally') {
            steps {
                sh 'sudo npm install -g snyk'
            }
        }

        stage('Authenticate Snyk') {
            steps {
                sh 'snyk auth $SNYK_TOKEN'
            }
        }

        stage('Run Snyk Security Scan') {
            steps {
                sh 'snyk test --all-projects'
            }
        }

        stage('Generate Report') {
            steps {
                sh 'snyk test --json > snyk-report.json'
                archiveArtifacts artifacts: 'snyk-report.json', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed!'
        }
        success {
            echo 'Security scan passed with no critical issues.'
        }
        failure {
            echo 'Security vulnerabilities detected! Check the Snyk report for details.'
        }
    }
}
