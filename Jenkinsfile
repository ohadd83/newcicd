pipeline {
    agent any

    environment {
        IMAGE_NAME = "ohadd306/app-ohad"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    docker pull ${IMAGE_NAME}:${IMAGE_TAG}
                    docker stop simple-app || true
                    docker rm simple-app || true
                    docker run -d -p 8087:3000 --name simple-app ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
    }
}
