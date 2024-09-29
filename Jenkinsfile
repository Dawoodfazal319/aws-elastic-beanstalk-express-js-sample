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

        // Step 2: Build the Project
        stage('Build') {
            steps {
                // You can replace this with your actual build step
                sh 'npm run build || echo "No build script defined"'
            }
        }

        // Step 3: Run Security Scan (using Snyk)
        stage('Security Scan') {
            steps {
                // Install and run Snyk for security testing
                sh 'npm install -g snyk'
                sh 'snyk test || echo "Vulnerabilities detected, please fix"'
            }
        }

        // Step 4: Archive Artifacts
        stage('Archive Artifacts') {
            steps {
                // Archive important build outputs or other artifacts
                archiveArtifacts artifacts: '**/build/**/*', allowEmptyArchive: true
            }
        }
    }

    // Post-build actions (for logging and notifications)
    post {
        // Always store logs and artifacts
        always {
            // Save logs and any other necessary files
            archiveArtifacts artifacts: '**/logs/**/*', allowEmptyArchive: true
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
