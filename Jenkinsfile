pipeline {
    agent any

    environment {
        APP_SERVER = "ubuntu@172.31.18.35"
        APP_DIR    = "/home/ubuntu/mern-app"
        IMAGE_NAME = "mern-store:latest"
        CONTAINER  = "mern-store"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shresth963/MERN-E-Commerce-Store.git'
            }
        }

        stage('Build Frontend') {
            steps {
                sh '''
                cd frontend
                npm install --legacy-peer-deps
                npm run build
                '''
            }
        }

        stage('Sync Code to Application EC2') {
            steps {
                sh '''
                ssh ${APP_SERVER} "mkdir -p ${APP_DIR}"
                rsync -avz --delete ./ ${APP_SERVER}:${APP_DIR}/
                '''
            }
        }

        stage('Build & Deploy on Application EC2') {
            steps {
                sh '''
                ssh ${APP_SERVER} "
                  cd ${APP_DIR} || exit 1
                  docker rm -f ${CONTAINER} || true
                  docker build -t ${IMAGE_NAME} -f backend/Dockerfile .
                  docker run -d \
                    --name ${CONTAINER} \
                    -p 5000:5000 \
                    ${IMAGE_NAME}
                "
                '''
            }
        }
    }
}
