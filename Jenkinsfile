pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/1jashshah/Ansible-training.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("1jashshah/ansible")
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
                    }
                }
            }
        }
    }
}
