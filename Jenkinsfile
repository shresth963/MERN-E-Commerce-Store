pipeline {
    agent any

    environment {
        APP_SERVER = "ubuntu@13.62.127.175"    
        APP_NAME   = "mern-store"
        FRONTEND_DIR = "frontend"
        BACKEND_DIR  = "backend"
        APP_PORT   = "5000"         
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/shresth963/MERN-E-Commerce-Store.git', branch: 'main'
            }
        }

        stage('Install Dependencies & Build Frontend') {
            steps {
                sh """
                cd ${FRONTEND_DIR}
                npm install
                npm run build
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${APP_NAME}:latest .
                """
            }
        }

        stage('Deploy to EC2') {
            steps
