pipeline {
    agent any

    environment {
        APP_NAME   = "mern-store"
        IMAGE_NAME = "mern-store:latest"
        APP_PORT   = "5000"
        APP_DIR    = "/home/ubuntu/mern-app"

        // App server
        APP_SERVER = "ubuntu@16.171.4.146"

        // SSH key (Jenkins server path)
        SSH_KEY = "/var/lib/jenkins/y"

        // Mongo
        DOCKER_NETWORK  = "mern-net"
        MONGO_CONTAINER = "mongo"
    }

    stages {

        stage('Checkout Code') {
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

        stage('Copy Code to App Server') {
            steps {
                sh """
                  ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} "rm -rf ${APP_DIR}"
                  rsync -avz --delete -e "ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no" ./ ${APP_SERVER}:${APP_DIR}
                """
            }
        }

        stage('Prepare App Server (Docker + Mongo)') {
            steps {
                sh """
                  ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} '
                    docker network create ${DOCKER_NETWORK} || true

                    docker rm -f ${MONGO_CONTAINER} || true
                    docker run -d \
                      --name ${MONGO_CONTAINER} \
                      --network ${DOCKER_NETWORK} \
                      -p 27017:27017 \
                      mongo:6
                  '
                """
            }
        }

        stage('Build Docker Image on App Server') {
            steps {
                sh """
                  ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} '
                    cd ${APP_DIR} &&
                    docker rm -f ${APP_NAME} || true &&
                    docker build -t ${IMAGE_NAME} -f backend/Dockerfile .
                  '
                """
            }
        }

        stage('Run Application Container') {
            steps {
                sh """
                  ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} '
                    docker rm -f ${APP_NAME} || true

                    docker run -d \
                      --name ${APP_NAME} \
                      --network ${DOCKER_NETWORK} \
                      -p ${APP_PORT}:${APP_PORT} \
                      -e NODE_ENV=production \
                      -e MONGO_URI=mongodb://mongo:27017/mern_db \
                      ${IMAGE_NAME}
                  '
                """
            }
        }
    }

    post {
        success {
            echo "✅ MERN App Deployed Successfully"
        }
        failure {
            echo "❌ Deployment Failed"
        }
    }
}
