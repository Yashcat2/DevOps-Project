pipeline {
    agent any
    
    tools {
        nodejs "NodeJS 18"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'cd frontend && npm ci'
                sh 'cd BackEnd && npm ci'
                sh 'cd Admin && npm ci'
            }
        }
        
        stage('Build') {
            steps {
                sh 'cd frontend && npm run build'
                sh 'cd Admin && npm run build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'cd frontend && npm test -- --passWithNoTests'
                sh 'cd BackEnd && npm test -- --passWithNoTests'
                sh 'cd Admin && npm test -- --passWithNoTests'
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                    sudo mkdir -p /var/www/myapp
                    sudo cp -r frontend/build/* /var/www/myapp/
                    sudo cp -r Admin/build /var/www/myapp/admin
                    
                    # Stop existing PM2 processes if they exist
                    sudo pm2 stop backend || true
                    sudo pm2 stop admin-server || true
                    
                    # Start backend server with PM2
                    cd BackEnd
                    sudo npm install -g pm2
                    sudo pm2 start server.js --name backend
                    sudo pm2 save
                    
                    # Configure Nginx
                    sudo tee /etc/nginx/sites-available/myapp <<EOF
server {
    listen 80;
    server_name _;

    location / {
        root /var/www/myapp;
        try_files \$uri \$uri/ /index.html;
    }

    location /admin {
        alias /var/www/myapp/admin;
        try_files \$uri \$uri/ /admin/index.html;
    }

    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host \$host;
        proxy_cache_bypass \$http_upgrade;
    }
}
EOF
                    
                    sudo ln -sf /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
                    sudo nginx -t
                    sudo systemctl restart nginx
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
