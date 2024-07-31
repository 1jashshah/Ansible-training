pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerid') // Replace with your Jenkins credential ID
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/1jashshah/Ansible-training.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("1jashshah/ansible:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerid') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest") // Optionally push the latest tag
                    }
                }
            }
        }
    }
}
