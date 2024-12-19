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
                sh "docker build -t flask ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"46974820-1baf-4920-8958-bdc456ba4170",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                    sh  'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
                    sh "docker image tag flask-app ${env.dockerHubUser}/flask-app:latest"
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
