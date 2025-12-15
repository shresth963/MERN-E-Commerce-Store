pipeline {
    agent any

    environment {
        IMAGE_NAME = "mern-store:latest"
        APP_SERVER = "ubuntu@172.31.18.35"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shresth963/MERN-E-Commerce-Store.git'
            }
        }

        stage('Install & Build Frontend') {
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
                docker build -t ${IMAGE_NAME} -f backend/Dockerfile .
                '''
            }
        }

        stage('Deploy to Application EC2') {
            steps {
                sh '''
                ssh ubuntu@172.31.18.35 "
                  docker rm -f mern-store || true
                  docker run -d \
                    --name mern-store \
                    -p 5000:5000 \
                    ${IMAGE_NAME}
                "
                '''
            }
        }
    }
}
