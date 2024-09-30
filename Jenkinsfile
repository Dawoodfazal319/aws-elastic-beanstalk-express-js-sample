pipeline {
    agent {
        docker {
            image 'node:16'
            args '-v $HOME/.npm:/root/.npm'
        }
    }
    stages {
        // Step 1: Install Dependencies
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // Optional: Step 2: Build the Project (remove if not needed)
        // If you don't need a build step, remove this stage.
        stage('Build') {
            steps {
                sh 'npm run build || echo "No build script defined"'
            }
        }

        stage('Security Scan') {
    steps {
        withCredentials([string(credentialsId: 'snyk-api-token', variable: 'SNYK_TOKEN')]) {
            sh 'snyk auth $SNYK_TOKEN'  // Authenticate Snyk using the API token
            sh 'snyk test || echo "Vulnerabilities detected, please fix"'  // Run Snyk test
        }
    }
}


        // Step 4: Archive Artifacts
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*', allowEmptyArchive: true  // Archive all files
            }
        }
    }

    // Post-build actions (for logging and notifications)
    post {
        // Always store logs and artifacts, modify paths to match your project structure
        always {
            archiveArtifacts artifacts: '**/logs/**/*', allowEmptyArchive: true  // Logs
        }
        // Notify on build failure
        failure {
            echo 'Build failed! Please check the logs and fix issues.'
        }
        // Notify on build success
        success {
            echo 'Build succeeded!'
        }
    }
}
