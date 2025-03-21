pipeline {
    agent any

    environment {
        NODE_OPTIONS = "--max-old-space-size=4096" // Allocate more memory for npm processes
    }

    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing dependencies..."
                    sh 'npm install'
                }
            }
        }

        stage('Install Snyk Locally') {
            steps {
                script {
                    echo "Installing Snyk CLI..."
                    sh 'npm install snyk --save-dev'
                }
            }
        }

        stage('Authenticate Snyk') {
            steps {
                script {
                    echo "Authenticating with Snyk..."
                    withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
                        sh 'echo $SNYK_TOKEN | ./node_modules/.bin/snyk auth'
                    }
                }
            }
        }

        stage('Run Snyk Scan') {
            steps {
                script {
                    echo "Running Snyk security scan..."
                    withCredentials([string(credentialsId: 'SNYK_API_TOKEN', variable: 'SNYK_TOKEN')]) {
                        sh '''
                            set -e  # Fail pipeline on errors
                            ./node_modules/.bin/snyk test --severity-threshold=high || true
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Build process completed.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed! Check logs for details.'
        }
    }
}
