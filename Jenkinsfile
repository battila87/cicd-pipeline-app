pipeline {
    agent any

    environment {
        // Set PORT dynamically based on branch name; fallback to 3000
        APP_PORT = "${BRANCH_NAME == 'dev' ? '3001' : '3000'}"
        IMAGE_NAME = "my-app:${BRANCH_NAME == 'dev' ? 'dev' : 'latest'}"
        CONTAINER_NAME = "app-${BRANCH_NAME == 'dev' ? 'dev' : 'prod'}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building application for ${BRANCH_NAME} branch..."
                // Insert build commands here (e.g., npm install)
            }
        }

        stage('Test') {
            steps {
                echo "Running unit tests..."
                // Insert testing commands here (e.g., npm test)
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker Image: ${IMAGE_NAME}"
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo "Deploying on Port: ${APP_PORT}"
                    // Stop and remove existing container if running
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    
                    // Run container mapping host port to target application port
                    sh "docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:3000 ${IMAGE_NAME}"
                }
            }
        }
    }
}
