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
                sh 'trivy fs --exit-code 0 --severity HIGH,CRITICAL .'
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
                withCredentials([usernamePassword(credentialsId: "dockerHubCreds", usernameVariable: "DOCKER_USER", passwordVariable: "DOCKER_PASS")]) {
                    sh """
                    docker login -u $DOCKER_USER -p $DOCKER_PASS
                    docker tag two-tier-flask-app $DOCKER_USER/two-tier-flask-app:latest
                    docker push $DOCKER_USER/two-tier-flask-app:latest
                    """
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
                subject: 'Build Success for Demo CICD App',
                body: 'Build Success for Demo CICD App'
            )
        }
        failure {
            emailext(
                from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                subject: 'Build Failed for Demo CICD App',
                body: 'Build Failed for Demo CICD App'
            )
        }
    }
}
