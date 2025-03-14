pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/geetha-17/jenkins-docker-app.git',
                        credentialsId: 'b213f9bd-9d94-4318-aec9-aabb24118bfd'
                    ]]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t geetha-17/jenkins-docker-app:latest .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '41380629-0631-44da-ba4d-add2b73c5926', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push geetha-17/jenkins-docker-app:latest'
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker rmi geetha-17/jenkins-docker-app:latest'
            }
        }
    }
}
