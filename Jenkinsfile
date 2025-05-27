pipeline {
    agent any // Or 'agent { docker { image 'maven:3.8.1-jdk-8' } }' for a specific build environment

    environment {
        // Define environment variables if needed
        DOCKER_IMAGE_NAME = 'jenkins-node-app'
        DOCKER_REGISTRY = '' // e.g., 'your_dockerhub_username' or 'your_private_registry_url'
        DOCKER_REGISTRY_CREDENTIAL_ID = '' // Jenkins credential ID for Docker registry login (if DOCKER_REGISTRY is not empty)
        APP_PORT = '3000'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your SCM (Source Code Management) project
                git branch: 'main', url: 'https://github.com/jansjossy/jenkins.git' // REPLACE WITH YOUR REPO URL AND BRANCH
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:latest ."
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ." // Tag with build number
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run unit tests (if any)
                    // For a Node.js app, this might be:
                    sh "npm test" // Runs the 'test' script defined in package.json
                    echo "Tests passed!"
                }
            }
        }

        stage('Push Docker Image (Optional)') {
            when {
                environment name: 'DOCKER_REGISTRY', expression: { DOCKER_REGISTRY != '' } // Only run if a registry is defined
            }
            steps {
                script {
                    // Login to Docker registry
                    // Ensure you have a Jenkins 'Username with password' credential with the ID specified in DOCKER_REGISTRY_CREDENTIAL_ID
                    withDockerRegistry(credentialsId: "${DOCKER_REGISTRY_CREDENTIAL_ID}", url: "https://${DOCKER_REGISTRY}") {
                        sh "docker tag ${DOCKER_IMAGE_NAME}:latest ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:latest"
                        sh "docker tag ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER} ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
                        sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:latest"
                        sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop any existing container (if running)
                    sh "docker stop ${DOCKER_IMAGE_NAME}-container || true" // '|| true' prevents error if container doesn't exist
                    sh "docker rm ${DOCKER_IMAGE_NAME}-container || true"

                    // Run the new Docker container
                    sh "docker run -d -p ${APP_PORT}:${APP_PORT} --name ${DOCKER_IMAGE_NAME}-container ${DOCKER_IMAGE_NAME}:latest"
                    echo "Application deployed to http://localhost:${APP_PORT}"
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}