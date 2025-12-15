pipeline {
    agent any

    environment {
        APP_NAME   = "mern-store"
        IMAGE_NAME = "mern-store:latest"
        APP_PORT   = "5000"
        APP_DIR    = "/home/ubuntu/mern-app"
        APP_SERVER = "ubuntu@172.31.18.35"
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

        stage('Prepare Backend') {
            steps {
                sh '''
                  echo "Frontend build ready"
                  ls -la frontend/dist
                '''
            }
        }

        stage('Copy Code to App Server') {
            steps {
                sh """
                  ssh ${APP_SERVER} 'rm -rf ${APP_DIR}'
                  rsync -avz --delete ./ ${APP_SERVER}:${APP_DIR}
                """
            }
        }

        stage('Build Docker Image on App Server') {
            steps {
                sh """
                  ssh ${APP_SERVER} '
                    cd ${APP_DIR} &&
                    docker rm -f ${APP_NAME} || true &&
                    docker build -t ${IMAGE_NAME} -f backend/Dockerfile .
                  '
                """
            }
        }

        stage('Run Container') {
            steps {
                sh """
                  ssh ${APP_SERVER} '
                    docker run -d \
                      --name ${APP_NAME} \
                      -p ${APP_PORT}:${APP_PORT} \
                      -e NODE_ENV=production \
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
