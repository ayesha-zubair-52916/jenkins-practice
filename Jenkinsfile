
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ayeshazubair/jenkins-docker-practice'
    }

    stages {
        stage('Pull Code from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ayesha-zubair-52916/jenkins-practice.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:latest -t %DOCKER_IMAGE%:%BUILD_NUMBER% .'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-credentials',
                        usernameVariable: 'DH_USER',
                        passwordVariable: 'DH_TOKEN'
                    )
                ]) {
                    bat '@echo off && echo %DH_TOKEN%| docker login -u %DH_USER% --password-stdin'

                    bat 'docker push %DOCKER_IMAGE%:latest'
                    bat 'docker push %DOCKER_IMAGE%:%BUILD_NUMBER%'
                }
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }

        success {
            echo 'SUCCESS: Docker image built and pushed to Docker Hub.'
        }

        failure {
            echo 'FAILED: Check the Console Output for the error.'
        }
    }
}
