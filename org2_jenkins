pipeline {
    agent any

    environment {
        IMAGE_NAME = "simple-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            agent {
                docker { image 'node:18' }
            }
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            agent {
                docker { image 'node:18' }
            }
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${FULL_IMAGE} ."
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker stop simple-app || true
                    docker rm simple-app || true
                    docker run -d -p 8087:3000 --name simple-app ${FULL_IMAGE}
                """
            }
        }
    }
}
