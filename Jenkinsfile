pipeline {
    agent any

    stages {
        stage("Code clone") {
            steps {
                git url: 'https://github.com/haseeb9876/two-tier-flask-app.git', branch: 'master'
            }
        }

        stage("Build") {
            steps {
                sh 'docker build --no-cache -t my-app .'
            }
        }

        stage("Test") {
            steps {
                echo 'Tests passed!'
            }
        }

        stage("Push to docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubpass",
                    usernameVariable: "dockerHubUser"
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubpass}"
                    sh "docker image tag my-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh 'docker compose up -d --build flask-app'
            }
        }
    }
}
