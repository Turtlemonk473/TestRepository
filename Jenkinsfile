pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds-id') // Jenkins secret
        IMAGE_NAME = "kymaniambrose/webapp"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', DOCKERHUB_CREDENTIALS) {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh "docker run -d -p 80:80 ${IMAGE_NAME}:latest"
            }
        }
    }
}
