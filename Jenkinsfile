pipeline {
    agent any

    environment {
        IMAGE_NAME = 'geetha8500/jenkins-docker-app'
        DOCKER_CREDENTIALS_ID = '41380629-0631-44da-ba4d-add2b73c5926'  // Make sure this ID matches your Jenkins credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Remove existing directory to prevent conflicts
                    sh 'rm -rf jenkins-docker-app || true'
                    sh 'git clone https://geetha-17:${GIT_PASSWORD}@github.com/geetha-17/jenkins-docker-app.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:latest jenkins-docker-app'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '41380629-0631-44da-ba4d-add2b73c5926', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }

    post {
        failure {
            echo 'Build Failed!'
        }
        success {
            echo 'Build and Push Successful!'
        }
    }
}
