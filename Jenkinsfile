pipeline {
    agent any

    environment {
        IMAGE_NAME = "turtlemonk473/webapp"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds-id', passwordVariable: 'DOCKER_PWD', usernameVariable: 'DOCKER_USER')]) {
                    sh '''
                        echo "$DOCKER_PWD" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 80:80 $IMAGE_NAME || true'
            }
        }
    }
}
