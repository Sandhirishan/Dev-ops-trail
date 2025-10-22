pipeline {

    environment {
        dockerimagename = "bravinwasike/react-app"
        dockerImage = ""
        registryCredential = 'dockerhub-credentials'
    }

    agent any

    stages {

        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/Bravinsimiyu/jenkins-kubernetes-deployment.git', branch: 'main'
            }
        }

        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Push Image') {
            steps{
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f deployment.yaml"
                    sh "kubectl apply -f service.yaml"
                }
            }
        }

    }

}
