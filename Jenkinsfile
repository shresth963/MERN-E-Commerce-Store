pipeline {
    agent any

    environment {
        APP_SERVER   = "ubuntu@13.62.127.175"
        APP_NAME     = "mern-store"
        FRONTEND_DIR = "frontend"
        BACKEND_DIR  = "backend"
        APP_PORT     = "5000"
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/shresth963/MERN-E-Commerce-Store.git',
                    branch: 'main'
            }
        }

        stage('Install Dependencies & Build Frontend') {
            steps {
                sh '''
                cd frontend
                npm install --legacy-peer-deps
                npm run build
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t mern-store:latest .
                '''
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                echo "Deploy step placeholder"
                docker rm -f mern-store || true
                docker run -d -p 5000:5000 --name mern-store mern-store:latest
                '''
            }
        }

    } // stages end

} // pipeline end
