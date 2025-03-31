pipeline {
    agent any

    environment {
        SNYK_HTTP_TIMEOUT = "600000"  // Increases timeout for slow networks
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/sny17/snyk-code-scan.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }

        stage('Install Snyk Locally') {
            steps {
                script {
                    echo 'Installing Snyk CLI...'
                    sh 'npm install snyk --save-dev'
                }
            }
        }

        stage('Authenticate Snyk') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'snyk_token', variable: 'SNYK_TOKEN')]) {
                        echo 'Authenticating with Snyk...'
                        sh './node_modules/.bin/snyk auth ${SNYK_TOKEN}'
                    }
                }
            }
        }

        stage('Run Snyk Scan') {
            steps {
                script {
                    echo 'Running Snyk security scan...'
                    sh './node_modules/.bin/snyk test --all-projects'
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed! Check logs for details.'
        }
    }
}
