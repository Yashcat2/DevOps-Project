<<<<<<< HEAD
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout your repository containing docker-compose.yml
               checkout scm
            }
        }
        
        stage('Deploy with Docker Compose') {
            steps {
                // Use bat for Windows command execution
                bat 'docker-compose up -d'
                
                // If you need to specify the file path:
                // bat 'docker-compose -f path/to/docker-compose.yml up -d'
            }
        }
    }

=======
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout your repository containing docker-compose.yml
               checkout scm
            }
        }
        
        stage('Deploy with Docker Compose') {
            steps {
                // Use bat for Windows command execution
                bat 'docker-compose up -d'
                
                // If you need to specify the file path:
                // bat 'docker-compose -f path/to/docker-compose.yml up -d'
            }
        }
    }

>>>>>>> yash
}