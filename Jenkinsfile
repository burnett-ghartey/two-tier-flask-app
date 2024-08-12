pipeline {
    agent {
        node { label 'dev' }
    }
    
    stages {
        stage('code') {
            steps {
                git branch: 'master', url: 'https://github.com/burnett-ghartey/two-tier-flask-app.git'
                echo 'hello'
            }
        }
        stage('build') {
            steps {
                sh "docker build -t twotierfapp:latest ."
            }
        }
        stage('docker push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerCreds', usernameVariable: 'DockerUsername', passwordVariable: 'DockerPassword')]) {
                    sh "docker image tag twotierfapp:latest $DockerUsername/twotierfapp:latest"
                    sh "docker login -u $DockerUsername -p $DockerPassword"
                    sh "docker push $DockerUsername/twotierfapp:latest"
                }
            }
        }
        stage('Docker compose') {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
