
pipeline {
    
    agent any

    .
    stages {
        
        stage('Build') {
            steps {
                
                echo 'Starting the build process...'
                
            }
        }

        
        stage('Test') {
            steps {
                

                echo 'Running tests...'
                
            }
        }

        
        stage('Deploy') {
            steps {
                

                echo 'Deploying the application...'
               
            }
        }
    }

   
    post {
        always {
            echo 'Pipeline finished!'
            
        }
        success {
            echo 'Pipeline succeeded! Sending success notification...'
        }
        failure {
            echo 'Pipeline failed! Sending failure notification...'
        }
    }
}