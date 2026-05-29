pipeline{
    agent any;
    stages{
        stage("Code"){
            steps{
                git url:"https://github.com/YashM211/two-tier-flask-app.git" , branch:"master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Test ho gaya"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                    )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
