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
                withCredentials([usernamePassword(
                    credentialsId:"90964560-433f-46f9-853a-b426d9bf6b19",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                    sh  'echo $dockerHubPass | docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}'
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
