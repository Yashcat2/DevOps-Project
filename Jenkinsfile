pipeline {
    agent any
    environment {
        DOCKER_IMAGE_BACKEND = "yashcat2/backend-app:latest"
        DOCKER_IMAGE_FRONTEND = "yashcat2/react-frontend:latest"
        DOCKER_CREDENTIALS_ID = "docker-hub"
        SSH_CREDENTIALS_ID = "homemate-dev-server"
        SSH_TARGET = "ubuntu@51.20.183.95"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Yashcat2/DevOps-Project.git'
            }
        }
                
        stage('Build Docker Image Backend') {
            steps {
                script {
                    sh '''
                    cd BackEnd
                    ls
                    docker build -t $DOCKER_IMAGE_BACKEND .
                    '''
                }
            }
        }
                
        stage('Build Docker Image Frontend') {
            steps {
                script {
                    sh '''
                    cd frontend
                    docker build -t $DOCKER_IMAGE_FRONTEND .
                    '''
                }
            }
        }
                
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        '''
                    }
                }
            }
        }
                
        stage('Push to Docker Hub Backend') {
            steps {
                script {
                    sh '''
                    docker push $DOCKER_IMAGE_BACKEND
                    '''
                }
            }
        }
                
        stage('Push to Docker Hub Frontend') {
            steps {
                script {
                    sh '''
                    docker push $DOCKER_IMAGE_FRONTEND
                    '''
                }
            }
        }
                
        stage('Deploy to Server Backend') {
            steps {
                script {
                    sh '''
                    docker stop backend-app || true
                    docker rm backend-app || true
                    docker run -d --name backend-app -p 5000:5000 --restart unless-stopped $DOCKER_IMAGE_BACKEND
                    # Verify container is running
                    docker ps | grep backend-app
                    '''
                }
            }
        }
                
        stage('Deploy to Server Frontend') {
            steps {
                script {
                    sh '''
                    docker stop frontend-app || true
                    docker rm frontend-app || true
                    docker run -d --name frontend-app -p 3000:80 --restart unless-stopped $DOCKER_IMAGE_FRONTEND
                    # Verify container is running
                    docker ps | grep frontend-app
                    # Check logs for any immediate errors
                    sleep 3
                    docker logs frontend-app
                    '''
                }
            }
        }
        
        stage('Verify Deployment') {
            steps {
                script {
                    sh '''
                    echo "Verifying both containers are running..."
                    docker ps
                    '''
                }
            }
        }
    }
        
    post {
        always {
            echo 'Cleaning up unused Docker images'
            sh '''
            # Don't remove running containers, only dangling images
            docker image prune -f
            '''
        }
    }
}
