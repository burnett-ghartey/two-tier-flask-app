pipeline{
    agent{
        node {
        label 'dev'
        }
    }
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/VandanaPandit/two-tier-flask-app.git" ,branch: "master" 
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t twotierfapp:latest ."
            }
        }
        stage("Docker push")
        {
            steps{
            withCredentials([usernamePassword(credentialsId: 'DockerCreds', usernameVariable: 'DockerUsername', passwordVariable: 'DockerPassword')]) 
            {
            sh "docker image tag twotierfapp:latest $DockerUsername/twotierfapp:latest"
            sh "docker login -u $DockerUsername -p $DockerPassword"
            sh "docker push $DockerUsername/twotierfapp:latest"
            }
            }
        }
        stage("Docker Compose"){
            steps{
            sh "docker compose down && docker compose up -d"
            }
        }
        
    }
}
