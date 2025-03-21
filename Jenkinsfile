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

        stage('Run Snyk Security Scan') {
            steps {
                sh 'npm install -g snyk'
                sh 'snyk auth $SNYK_API_TOKEN'
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
}

