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
         sudo  sh '''
                echo "Building Docker image..."
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .
            '''
        }
    }

    stage('Stop Old Container') {
        steps {
       sudo  sh '''
                echo "Stopping old container..."
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
            '''
        }
    }

    stage('Run Container') {
        steps {
           sudo sh '''
                echo "Starting new container..."

                docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p 8080:8080 \
                    ${IMAGE_NAME}:${BUILD_NUMBER}
            '''
        }
    }

    stage('Verify') {
        steps {
            echo 'Checking Docker container...'

           sudo sh '''
                sleep 10

                echo "Docker containers:"
                docker ps

                echo "Application logs:"
                docker logs ${CONTAINER_NAME}
            '''
        }
    }
}

post {
    success {
        echo 'Deployment successful!'
        echo 'Application should be available on port 8080.'
    }

    failure {
        echo 'Deployment failed!'
    }

    always {
        echo 'Pipeline finished.'
    }
}


}
