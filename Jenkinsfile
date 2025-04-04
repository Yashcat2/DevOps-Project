// pipeline {
//     agent any

//     environment {
//         DOCKER_IMAGE = "yashcat2/backend-app:latest"
//         DOCKER_CREDENTIALS_ID = "DockerHub"
//         SSH_CREDENTIALS_ID = "homemate-dev-server"
//         SSH_TARGET = "ubuntu@51.20.183.95"
//         REACT_APP_IMAGE = "yashcat2/react-frontend:latest"
//     }

//     stages {
//         stage('Clone Repository') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/Yashcat2/DevOps-Project.git'
//             }
//         }
        
//         stage('Build Docker Image backend') {
//             steps {
//                 script {
//                     sh '''
//                     cd BackEnd
//                     docker build -t $DOCKER_IMAGE .
//                     '''
//                 }
//             }
//         }
        
//         stage('Build Docker Image Frontend') {
//             steps {
//                 script {
//                     sh '''
//                     cd FrontEnd
//                     docker build -t $DOCKER_IMAGE .
//                     '''
//                 }
//             }
//         }

//         // 


//         stage('Login to Docker Hub BackEnd') {
//             steps {
//                 script {
//                     withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS_ID", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
//                         sh '''
//                         echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
//                         '''
//                     }
//                 }
//             }
//         }

//         stage('Push to Docker Hub BackEnd') {
//             steps {
//                 script {
//                     sh '''
//                     docker push $DOCKER_IMAGE
//                     '''
//                 }
//             }
//         }

//         stage('Deploy to Server BackEnd') {
//             steps {
//                 script {
//                     sshagent(credentials: ["$SSH_CREDENTIALS_ID"]) {
//                         sh '''
//                         ssh -o StrictHostKeyChecking=no $SSH_TARGET << 'EOF'
//                         docker pull $DOCKER_IMAGE
//                         docker stop backend-app || true
//                         docker rm backend-app || true
//                         docker run -d --name backend-app -p 8080:8080 $DOCKER_IMAGE
//                         EOF
//                         '''
//                     }
//                 }
//             }
//         }
//     }

//     post {
//         always {
//             echo 'Cleaning up Docker images'
//             sh 'docker system prune -f'
//         }
//     }
// }

pipeline {
    agent any

    environment {
        DOCKER_IMAGE_BACKEND = "yashcat2/backend-app:latest"
        DOCKER_IMAGE_FRONTEND = "yashcat2/react-frontend:latest"
        DOCKER_CREDENTIALS_ID = "DockerHub"
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
                    docker build -t $DOCKER_IMAGE_BACKEND .
                    '''
                }
            }
        }

        stage('Build Docker Image Frontend') {
            steps {
                script {
                    sh '''
                    cd FrontEnd
                    docker build -t $DOCKER_IMAGE_FRONTEND .
                    '''
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "$DOCKER_CREDENTIALS_ID", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
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
                    sshagent(credentials: ["$SSH_CREDENTIALS_ID"]) {
                        sh '''
                        ssh -o StrictHostKeyChecking=no $SSH_TARGET << 'EOF'
                        docker pull $DOCKER_IMAGE_BACKEND
                        docker stop backend-app || true
                        docker rm backend-app || true
                        docker run -d --name backend-app -p 8080:8080 $DOCKER_IMAGE_BACKEND
                        EOF
                        '''
                    }
                }
            }
        }

        stage('Deploy to Server Frontend') {
            steps {
                script {
                    sshagent(credentials: ["$SSH_CREDENTIALS_ID"]) {
                        sh '''
                        ssh -o StrictHostKeyChecking=no $SSH_TARGET << 'EOF'
                        docker pull $DOCKER_IMAGE_FRONTEND
                        docker stop frontend-app || true
                        docker rm frontend-app || true
                        docker run -d --name frontend-app -p 3000:3000 $DOCKER_IMAGE_FRONTEND
                        EOF
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up Docker images'
            sh 'docker system prune -f'
        }
    }
}
