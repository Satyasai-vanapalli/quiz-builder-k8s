pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_FRONTEND_IMAGE = 'quiz-frontend'
        DOCKER_BACKEND_IMAGE = 'quiz-backend'
        KUBE_CONFIG = credentials('kubernetes-config')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    sh 'docker build -t ${DOCKER_REGISTRY}/${DOCKER_FRONTEND_IMAGE}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    sh 'docker build -t ${DOCKER_REGISTRY}/${DOCKER_BACKEND_IMAGE}:${BUILD_NUMBER} .'
                }
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                    docker push ${DOCKER_REGISTRY}/${DOCKER_FRONTEND_IMAGE}:${BUILD_NUMBER}
                    docker push ${DOCKER_REGISTRY}/${DOCKER_BACKEND_IMAGE}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f k8s/
                '''
            }
        }
    }
}