pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/AniketNZade/Flaskapp.git", branch: "main"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t flask-app"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag flaskapp ${env.dockerHubUser}/flask-app:latest"
                    sh "docker push ${env.dockerHubUser}/flask-app:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
