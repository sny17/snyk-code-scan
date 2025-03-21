pipeline {
    agent any

    environment {
        NODE_OPTIONS = "--max-old-space-size=4096" // Allocate more memory for npm processes
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
                withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'echo $SNYK_TOKEN | ./node_modules/.bin/snyk auth'
                }
            }
        }

        stage('Run Snyk Scan') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh './node_modules/.bin/snyk test --severity-threshold=high || true'
                }
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
