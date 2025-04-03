// pipeline {
//     agent any
    
//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
        
//         stage('Deploy with Docker Compose') {
//             steps {
//                 sh 'docker-compose down || true'
//                 sh 'docker-compose up -d --build'
//             }
//         }
        
//         stage('Verify Deployment') {
//             steps {
//                 sh 'docker ps'
//             }
//         }
//     }
    
//     post {
//         always {
//             sh 'docker system prune -f || true'
//         }
//         success {
//             echo 'Deployment successful!'
//         }
//         failure {
//             echo 'Deployment failed!'
//         }
//     }
// }

pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker-compose down || true'
                sh 'docker-compose up -d --build'
            }
        }
        
        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
                echo 'Frontend available at: http://13.60.169.238/:3000'
                echo 'Backend available at: http://13.60.169.238:5000'
                echo 'Admin available at: http://13.60.169.238:5173'
            }
        }
    }
    
    post {
        always {
            sh 'docker system prune -f || true'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
