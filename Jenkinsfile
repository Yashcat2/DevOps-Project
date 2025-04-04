pipeline {
    agent any

     environment {
        DOCKER_IMAGE = "chamaravishwajith644/spring-boot-app:latest"
        DOCKER_CREDENTIALS_ID = "dockerhub-credentials"
        SSH_CREDENTIALS_ID = "homemate-dev-server"
        SSH_TARGET = "ubuntu@16.170.174.241"
        DOCKER_CONTAINER = "spring-boot-app"
        REACT_APP_IMAGE = "chamaravishwajith644/react-frontend:latest"
        TERRAFORM_USER = "Terraform-User-Jenkins"
    }


    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Yashcat2/DevOps-Project.git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application...'
                // Example build command
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Example deployment command
                sh 'docker build -t my-app .'
                sh 'docker run -d -p 3000:3000 my-app'
            }
        }
    }
}
