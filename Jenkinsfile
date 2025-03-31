pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'sny17',  // Use the correct ID
                    url: 'https://github.com/sny17/snyk-code-scan.git'
            }
        }
    }
}

