```groovy
pipeline {

    agent any

    environment {
        IMAGE_NAME = 'hello-java'
        CONTAINER_NAME = 'hello-java'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                        -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                        -t ${IMAGE_NAME}:latest \
                        .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p 8080:8080 \
                        ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    sleep 10
                    docker ps
                    docker logs ${CONTAINER_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo '🚀 Deployment successful!'
        }

        failure {
            echo '❌ Deployment failed!'
        }
    }
}
```
