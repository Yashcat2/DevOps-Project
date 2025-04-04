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
        script {
            def result = sh(script: 'docker-compose up -d', returnStatus: true)
            if (result != 0) {
                error("Docker Compose failed with exit code ${result}")
            }
        }
    }
}

        }
    }

