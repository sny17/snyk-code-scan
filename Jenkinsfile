pipeline {
    agent any
    environment {
        SNYK_TOKEN = credentials('defe1206-0acd-473c-82bd-f2e54ce2bdc7')  // Fetches the stored credential
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    credentialsId: 'sny17', // If authentication is needed
                    url: 'https://github.com/sny17/snyk-code-scan.git'
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
                sh 'snyk auth "$SNYK_TOKEN"'  // Uses stored token securely
            }
        }
        stage('Run Snyk Scan') {
            steps {
                script {
                    sh '''
                        set +e  # Prevents immediate failure if vulnerabilities are found
                        snyk test
                        set -e  # Re-enables strict error checking
                    '''
                }
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
