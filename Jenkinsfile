pipeline {
    agent any

    environment {
        IMAGE_NAME = "docker-jenkins-project"
        IMAGE_TAG = "build-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies using Node container...'
                sh 'docker run --rm -v $(pwd):/app -w /app node:18-alpine npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests using Node container...'
                sh 'docker run --rm -v $(pwd):/app -w /app node:18-alpine npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Verify Image') {
            steps {
                echo 'Listing Docker images...'
                sh 'docker images | grep ${IMAGE_NAME}'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check the logs above.'
        }
    }
}
