pipeline {
    agent {
        kubernetes {
            label 'k8s-agent'
            defaultContainer 'jnlp'
        }
    }
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-creds')
    }
    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/Bhargavkulla/flask-ci-cd.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                container('jnlp') {
                    sh 'docker build -t bhargavakulla/flask-ci-cd:latest .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                container('jnlp') {
                    withDockerRegistry([credentialsId: 'docker-creds', url: '']) {
                        sh 'docker push bhargavakulla/flask-ci-cd:latest'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                container('jnlp') {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                }
            }
        }
    }
}
