pipeline{
    agent{
        node{
            label "dev"
            }
    }
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/VandanaPandit/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t twotierapp:latest ."
            }
        }
        stage("Docker Push")
        {
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerCreds', 
                                             usernameVariable: 'DockerUser',
                                             passwordVariable: 'Dockerpass',
                                             )]) 
                                             {
                                                 sh "docker image tag twotierapp:latest ${env.DockerUser}/twotierapp:latest"
                                                 sh "docker login -u ${env.DockerUser} -p ${env.Dockerpass}" 
                                                
                                             
                                                 sh "docker push ${env.DockerUser}/twotierapp:latest" 
                                             }
            }
        }
        stage("docker compose"){
            steps{
                sh "docker compose down && docker compose up -d"
            }
        }
        
        }
    }
