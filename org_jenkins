pipeline {
    agent {
        any {
            image 'node:18'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        IMAGE_NAME = "simple-app"
    }

    stages {

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop simple-app || true
                    docker rm simple-app || true
                    docker run -d -p 8087:3000 --name simple-app $IMAGE_NAME:latest
                '''
            }
        }
    }
}
