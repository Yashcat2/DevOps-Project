pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Images') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'echo "Running tests..."'
                // Add actual test commands for each component
                // sh 'cd frontend && npm test'
                // sh 'cd backend && npm test'
                // sh 'cd admin && npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d'
            }
        }
        
        stage('Cleanup') {
            steps {
                sh 'docker system prune -af --volumes'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
