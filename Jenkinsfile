pipeline {
    agent {
        docker {
            image 'node:16'
            args '-v $HOME/.npm:/root/.npm'
        }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Start') {  // Updated to use the start script
            steps {
                sh 'npm start'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'  // Add your test script here if necessary
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/build/*', allowEmptyArchive: true
        }
    }
}
