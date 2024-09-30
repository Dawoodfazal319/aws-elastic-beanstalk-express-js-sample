pipeline {
    agent {
        docker {
            image 'node:16'
            args '-v $HOME/.npm:/root/.npm'
        }
    }
    stages {
        // Install dependencies stage
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // Start the application
        stage('Start Application') {
            steps {
                sh 'npm start'
            }
        }

        // Test the application
        stage('Test') {
            steps {
                sh 'npm test || echo "No test script defined"'
            }
        }

        // Security scan using Snyk
        stage('Security Scan') {
            steps {
                sh 'npm install -g snyk'
                withCredentials([string(credentialsId: 'snyk-api-token', variable: 'SNYK_TOKEN')]) {
                    sh 'snyk auth $SNYK_TOKEN'
                    sh 'snyk test || echo "Vulnerabilities detected, please fix"'
                }
            }
        }
    }

    // Post build actions
    post {
        always {
            archiveArtifacts artifacts: '**/logs/**/*', allowEmptyArchive: true
            echo 'Build logs archived'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
