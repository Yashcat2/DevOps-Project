pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-credentials' // Docker Hub credentials
        REACT_IMAGE = 'your-dockerhub-username/devopsproject-backend' // React image name
        NODE_IMAGE = 'your-dockerhub-username/devopsproject-frontend' // Node.js image name
        AWS_CREDENTIALS = 'aws-credentials' // AWS credentials for EC2 deployment
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Pull code from GitHub
                checkout scm
            }
        }
        
        stage('Build Frontend') {
            steps {
                script {
                    // Build the React app in Docker
                    sh 'docker build -t ${REACT_IMAGE} -f Dockerfile.frontend .'
                }
            }
        }
        
        stage('Build Backend') {
            steps {
                script {
                    // Build the Node.js backend Docker image
                    sh 'docker build -t ${NODE_IMAGE} -f Dockerfile.backend .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub with credentials stored in Jenkins
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    }
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    // Push the built images to Docker Hub
                    sh 'docker push ${REACT_IMAGE}'
                    sh 'docker push ${NODE_IMAGE}'
                }
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    // Use SSH to deploy to EC2 or use AWS CLI to pull Docker images and run containers
                    sshagent(credentials: [AWS_CREDENTIALS]) {
                        sh '''
                        ssh -o StrictHostKeyChecking=no ec2-user@<EC2-PUBLIC-IP> << EOF
                        docker pull ${REACT_IMAGE}
                        docker pull ${NODE_IMAGE}
                        docker run -d -p 80:80 ${REACT_IMAGE}
                        docker run -d -p 4000:4000 ${NODE_IMAGE}
                        EOF
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
