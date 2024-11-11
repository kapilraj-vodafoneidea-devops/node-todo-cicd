pipeline{
    agent{ label 'dev-server'}
    stages{
        stage("code clone"){
        steps{
            echo "Code clone stage"
            git url: "https://github.com/kapilraj-vodafoneidea-devops/node-todo-cicd.git" , branch: "master"
            }
        }
        stage("code build and test"){
            steps{
                echo "Code Build stage"
                sh "docker build -t node-app ."
            }
        }    
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                credentialsId:"dockerHubCreds",
                usernameVariable:"dockerHubUser", 
                passwordVariable:"dockerHubPass")]){
                sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                sh "docker image tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${dockerHubUser}/node-app:latest"
                }    
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}  
