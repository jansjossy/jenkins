pipeline {
    agent any

    environment {
        IMAGE_NAME = 'demo-app'
        CONTAINER_PORT = '3000'
        HOST_PORT = '3000'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/jansjossy/jenkins.git', branch: 'main' // or 'master' depending on your repo
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Stop and remove any container already running on the same port
                    sh """
                    docker ps -q --filter "ancestor=$IMAGE_NAME" | grep . && docker stop \$(docker ps -q --filter "ancestor=$IMAGE_NAME") && docker rm \$(docker ps -a -q --filter "ancestor=$IMAGE_NAME") || echo "No running container to stop"
                    docker run -d -p $HOST_PORT:$CONTAINER_PORT $IMAGE_NAME
                    """
                }
            }
        }
    }
}
