pipeline {

    environment {
        dockerimagename = "bravinwasike/react-app"
        dockerImage = ""
    }

    agent any

    stages {

        stage('Checkout Source') {
            steps {
                // FIX APPLIED: Explicitly specifying the 'main' branch.
                git url: 'https://github.com/Bravinsimiyu/jenkins-kubernetes-deployment.git', branch: 'main' 
            }
        }

        stage('Build image') {
            steps{
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }

        stage('Pushing Image') {
            environment {
                // Ensure 'dockerhub-credentials' is the correct ID for your DockerHub login
                registryCredential = 'dockerhub-credentials'
            }
            steps{
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploying React.js container to Kubernetes') {
            steps {
                script {
                    // Ensure you are running on an agent/node with access to 'kubectl' and Kubernetes
                    kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
                }
            }
        }

    }

}