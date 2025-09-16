pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'localhost:5000'
        IMAGE_NAME = 'my-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/krablord/my-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh """
                #!/bin/bash
                docker build -t ${DOCKER_REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER} .
                """
            }
        }
        stage('Run Tests') {
            steps {
                sh """
                #!/bin/bash
                docker run --rm ${DOCKER_REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER} npm test
                """
            }
        }
        stage('Push Docker Image') {
            steps {
                sh """
                #!/bin/bash
                docker push ${DOCKER_REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}
                """
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh """
                #!/bin/bash
                kubectl apply -f k8s/
                """
            }
        }
    }
}
