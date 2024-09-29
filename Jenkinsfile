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
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/build/*', allowEmptyArchive: true
        }
    }
}
