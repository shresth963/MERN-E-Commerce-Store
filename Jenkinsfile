pipeline {
    agent any

    environment {
        APP_SERVER   = "ubuntu@13.62.127.175"   // Application EC2
        APP_NAME     = "mern-store"
        FRONTEND_DIR = "frontend"
        BACKEND_DIR  = "backend"
        APP_PORT     = "5000"
        IMAGE_NAME   = "mern-store:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shresth963/MERN-E-Commerce-Store.git'
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
                docker build -t ${IMAGE_NAME} -f backend/Dockerfile backend
                '''
            }
        }

        stage('Deploy to Application EC2') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no ${APP_SERVER} << EOF
                  docker rm -f ${APP_NAME} || true
                  docker run -d \
                    --name ${APP_NAME} \
                    -p ${APP_PORT}:${APP_PORT} \
                    ${IMAGE_NAME}
                EOF
                '''
            }
        }
    }
}
