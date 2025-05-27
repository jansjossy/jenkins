// Jenkinsfile
pipeline {
    // 'agent' specifies where the pipeline or a specific stage will run.
    // 'any' means Jenkins will pick any available agent.
    // For Docker, you might use: agent { docker 'your-build-image:tag' }
    agent any

    // 'stages' is a block that contains one or more 'stage' definitions.
    stages {
        // First Stage: Build
        stage('Build') {
            steps {
                // Commands for building your application go here.
                // Examples based on common project types:

                // For a Java project (e.g., Maven):
                // sh 'mvn clean install'

                // For a Node.js project:
                // sh 'npm install'
                // sh 'npm run build' // if you have a build script

                // For a Python project (install dependencies):
                // sh 'pip install -r requirements.txt'

                // For a simple application, maybe just a placeholder echo:
                echo 'Starting the build process...'
                // Replace this echo with your actual build commands
            }
        }

        // Second Stage: Test
        stage('Test') {
            steps {
                // Commands for running tests go here.

                // For a Java project (Maven):
                // sh 'mvn test'

                // For a Node.js project:
                // sh 'npm test'

                // For a Python project (e.g., using pytest):
                // sh 'pytest'

                echo 'Running tests...'
                // Replace this echo with your actual test commands
            }
        }

        // Third Stage: Deploy (optional, but good for CI/CD)
        stage('Deploy') {
            steps {
                // Commands for deploying your application go here.
                // This could be:
                // - Copying artifacts to a server
                // - Pushing a Docker image to a registry
                // - Deploying to a cloud service (AWS, Azure, GCP)

                echo 'Deploying the application...'
                // Replace this echo with your actual deploy commands
            }
        }
    }

    // 'post' block is optional but useful for actions after the pipeline completes
    // (always, success, failure, aborted, etc.)
    post {
        always {
            echo 'Pipeline finished!'
            // cleanWs() // Clean up workspace
        }
        success {
            echo 'Pipeline succeeded! Sending success notification...'
        }
        failure {
            echo 'Pipeline failed! Sending failure notification...'
        }
    }
}