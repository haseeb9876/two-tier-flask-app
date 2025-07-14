pipeline {
    agent any

    stages {
        stage("Code Clone") {
            steps {
                git url: 'https://github.com/haseeb9876/two-tier-flask-app.git'
            }
        }

        stage("Trivy File System Scan") {
            steps {
                sh 'trivy fs --exit-code 1 --severity HIGH,CRITICAL .'
            }
        }

        stage("Build") {
            steps { 
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh ke dega..."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {
                    sh "docker login -u $dockerHubUser -p $dockerHubPass"
                    sh "docker tag two-tier-flask-app $dockerHubUser/two-tier-flask-app:latest"
                    sh "docker push $dockerHubUser/two-tier-flask-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }

    post {
        success {
            emailext(
                from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            )
        }
        failure {
            emailext(
                from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            )
        }
    }
}
